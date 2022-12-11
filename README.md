# Just Another Chat

## Visão Geral

A empresa ABC, provedora de serviços de email, percebeu que poderia aumentar o engajamento de seus usuário através da criação de um chat integrado ao seu ecosistema. Essa empresa já possui uma base com milhões de usuário, e utilizando o chat, ela poderia se consolidar ainda mais aumentando sua base, e consequentemente, ganhando mercado.

A ideia é que um chat seja disponibilizado junto ao email no seu sistema web, e que exista um aplicativo independente disponível para iOS e Android. Pessoas físicas ou jurídicas também poderão fazer uso do chat para notificações automáticas integradas à outros sistema. 

## Guias arquiteturais

### Requisitos funcionais

 - A transmissão de mensagens deve ser instantânea(real time);
 - A troca de mensagens deve ser de um para um(mensagem direta), ou um para muitos(grupo);
 - A mensagem deve ter 5 estados(não enviada, enviada, recebida, e lida). Para grupos, a mensagem deve mudar o próximo estado quando tal estado compreender todos os membros(ex: uma mensagem não lida se torna lida quando todos os membros tiverem lido);
 - Deve ser possível adicionar reações a uma mensagem. No caso de grupo, um contador deve registrar a quantidade;
 - As mensagens dos grupos devem ser agrupadas em forma de threads
 - Deve ser possível responder a uma mensagem; 
 - No caso de grupos, a mensagem de resposta será publicada apenas na mesma thread da mensagem respondida;
 - Deve ser possível mencionar usuários que fazem parte de um grupo;
 - Cada mensagem deve conter o horário da publicação;
 - Cada conversa deve conter a data da última mensagem;
 - Cada conversa deve conter a quantidade de mensagens não lidas;
 - Deve ser possível publicar mensagens de texto, áudio, vídeo, imagem e arquivo;
 - Cada grupo deve conter uma descrição e a quantidade de membros;
 - Na descrição do usuário, deve ser mostrado quando foi a última vez que esteve ativo e se está online;
 - Grupos devem possuir usuários com papel administrativo;
 - Administradores poderão moderar conteúdo e adicionar/remover membros;
 - Chat deve fornecer apis públicas;
 - Cada usuário deve conseguir fixar até 3 conversas;
 - As conversas devem ser ordenadas por data da última mensagem decrescente;
 - Novas mensagens devem gerar notificações;
 - Notificações de conversas podem ser mutadas;
 - Menções devem gerar notificações mesmo que a conversa esteja mutada;

### Restrições
 - A autenticação deve ser efetuada utilizando serviço de autenticação já utilizado pelos outros serviços da empresa ABC;

### Atributos de qualidade
 - Escalabilidade
 - Disponibilidade
 - Desempenho
 - Segurança
 - Privacidade

### Cenários
 - Escalabilidade -> Aumento no throughput -> O sistema deve se manter responsivo após um aumento de 5 vezes a média de acessos em condições normais;
 - Desempenho -> Tempo de resposta de transação -> O tempo de resposta das transações deve ser de no máximo 500ms;
 - Disponibilidade -> uptimpe -> O sitema deve estar disponível por no mínimo 99% do tempo;
 - Desempenho -> Tempo de envio de notificações -> O sistema deverá notificar o usuário sobre uma nova mensagem em no máximo 2s a partir do momento da publicação;
 - Desempenho -> Tempo de envio de mensagem -> O usuário deverá receber a notificação em seu chat em no máximo 2s a partir do momento da publicação;
 - Desempenho -> Tempo de publicação de mensagem de texto -> A publicação de mensagens de texto deve acontecer em no máximo 500ms;
 - Desempenho -> Tempo de publicação de mensagem de mídia -> A publicação de mensagens de mídia deve acontecer em no máximo 2min;
 - Segurança -> Acesso não autenticado -> apis devem rejeitar requisições com status 401;
 - Privacidade -> ???

## Decisões de Design

### Class diagram
```mermaid
classDiagram
    class Conversation{
        lastMessageDate
        author
        type
        createDate
        members
    }
    class DirectConversation{
        messages
    }
    class GroupConversation{
        name
        picture
        description
        threads
    }
    class Thread{
        author
        messages
        createDate
        conversation
    }
    Conversation <|--DirectConversation
    Conversation <|--GroupConversation
    GroupConversation "1" *-- "0..*" Thread: has

    class ConversationMember{
        member
        conversation
        date
        memberType
        conversationMuted
        conversationPinned
    }
    class User {
        name
        picture
        state
        lastActiveTime
    }
    Conversation "1" *-- "1..*" ConversationMember : has
    ConversationMember "1..*" --> "1" User : relates to
    Conversation "1..*" --> "1" User : is authored by
    Thread "1..*" --> "1" User : is authored by

    class Message{
        text
        publishDate
        deleted
        conversation
        author
        repliedMessage
    }
    class TextMessage
    
    class MediaMessage{
        mediaUrl
        size
    }

    class ImageMessage
    class DocumentMessage
    
    class VideoMessage{
        duration
    }
    class AudioMessage{
        duration
    }

    Message <|-- TextMessage
    Message <|-- MediaMessage
    MediaMessage <|-- ImageMessage
    MediaMessage <|-- DocumentMessage
    MediaMessage <|-- AudioMessage
    MediaMessage <|-- VideoMessage

    Message "1..*" --> "1" Message : replies
    DirectConversation "1" *-- "0..*" Message : has
    Thread "1" *-- "0..*" Message : has

    class MessageView{
        message
        user
        messageStatus
        viewDate
        receiveTime
    }
    class Reaction{
        type
        message
        user
        date
    }

    MessageView "1..*" --> "1" User : is viewed by
    MessageView "1..*" --> "1" Message : views
    Reaction "1..*" --> "1" User : is reacted by
    Reaction "1..*" --> "1" Message : is reacts to

    class UserState{
        ONLINE
        OFFLINE
    }
     User..UserState

    class MemberType{
        MEMBER
        ADMIN
    }
    ConversationMember..MemberType

    class MessageStatus{
        PENDING
        SENT
        RECEIVED
        READ
    }
    MessageStatus..MessageView

```

### System context diagram

```mermaid
    C4Context
      title System Context diagram for Chat System
      Person(user, "Chat User", "A user authenticated on chat system.")
      Person(3rd, "3rd Party System", "Third party systems interacting through apis.")

      System(chat, "Chat", "System of chat which allows users to exchange messages.")

      Rel(user, chat, "uses")
      Rel(3rd, chat, "uses")
```

### Container diagram

```mermaid
    C4Container
      title Container diagram for Chat System
      Person(user, "Chat User", "A user authenticated on chat system.")
      Person(3rd, "3rd Party System", "Third party systems interacting through apis.")

    Container_Boundary(chat, "Chat System") {
        
        Container_Ext(mobile_app, "Mobile App", "iOS/Android", "Mobile app used by users to interact on chat")
        Container_Ext(web_app, "Web App", "Node,Javascript", "Web app used by users to interact on chat")
        
        Container(auth, "Authentication Service", "Service", "Service responsible for authenticating users")

        Container(lb, "Load Balancer", "Nginx", "Balance load to chat api cluster")
        ContainerDb(file_storage, "File storage service", "File storage service", "Service to store uploaded files")
        Container(backend_api, "Backend API", "Java, Docker container", "Provides chat functionality via api")
        Container(notification_service, "Notification Service", "Java, Docker container/Serverless Function", "Service responsible for sending push notifications")
        ContainerDb(db, "Chat DB", "NOSQL Database", "Chat system main database")
        ContainerDb(notification_db, "Notification DB", "NOSQL Database", "Database to support dealing with notifications")
        Container_Ext(fcm_apns, "FCM & APNS", "FCM & APNS", "Mobile platforms notification services")
    }

    Rel(user, mobile_app, "Uses")
    Rel(user, web_app, "Uses")
    Rel(web_app, lb, "Uses")
    Rel(mobile_app, lb, "Uses")
    Rel(web_app, auth, "Uses")
    Rel(mobile_app, auth, "Uses")

    Rel(3rd, lb, "Uses")
    Rel(lb, backend_api, "Balances load to")
    Rel(backend_api, db, "Reads from and writes to")
    Rel(notification_service, fcm_apns, "Reads from and writes to")
    Rel(notification_service, notification_db, "Sends notifications to")
    Rel(backend_api, notification_service, "Uses")
    Rel(mobile_app, file_storage, "Uploads to")
    Rel(web_app, file_storage, "Uploads to")
    Rel(backend_api, file_storage, "Load uploaded files from")

    UpdateLayoutConfig($c4BoundaryInRow="2")
```

### Component diagram

```mermaid
C4Container
    title Component diagram for Chat System Backend API
    Container_Ext(mobile_app, "Mobile App", "iOS/Android", "Mobile app used by users to interact on chat")
    Container_Ext(web_app, "Web App", "Node,Javascript", "Web app used by users to interact on chat")
    Person(3rd, "3rd Party System", "Third party systems interacting through apis.")

    Container_Boundary(api, "Chat System API") {
        Component(messagec, "Message Controller", "MVC Rest Controller", "Provides Message api endpoints")
        Component(conversationc, "Conversation Controller", "MVC Rest Controller", "Provides Conversation api endpoints")
        Component(userc, "User Controller", "MVC Rest Controller", "Provides User api endpoints")
        Component(integrationc, "Integration Controller", "MVC Rest Controller", "Provides api endpoints for 3rd party systems")

        Component(chatservice, "Chat Service", "Service", "Service to handle chat business rules")
        Component(chatdao, "Chat DAO", "DAO", "Provides access to chat db")
        Component(storage, "Storage Service", "Service", "Provides mechanisms for using external file storage")
        Component(event, "Event Service", "Service", "Provides mechanisms for notifying changes in the system")
     
        ComponentQueue(eventqueue, "Event Queue", "Queue of application events")

        Rel(messagec, chatservice, "Uses")
        Rel(conversationc, chatservice, "Uses")
        Rel(userc, chatservice, "Uses")
        Rel(integrationc, chatservice, "Uses")

        Rel(chatservice, chatdao, "Uses")
        Rel(chatservice, storage, "Uses")
        Rel(chatservice, event, "Uses")
    }

    Container_Boundary(intcomp, "Chat System Internal Components") {
        ContainerDb(db, "Chat DB", "NOSQL Database", "Chat system main database")
        ContainerDb(file_storage, "File storage service", "File storage service", "Service to store uploaded files")
        Container(notification_service, "Notification Service", "Java, Docker container/Serverless Function", "Service responsible for sending push notifications")
    }

    Rel(mobile_app, messagec, "Uses", "JSON/HTTPS")
    Rel(mobile_app, conversationc, "Uses", "JSON/HTTPS")
    Rel(mobile_app, userc, "Uses", "JSON/HTTPS")
    Rel(web_app, messagec, "Uses", "JSON/HTTPS")
    Rel(web_app, conversationc, "Uses", "JSON/HTTPS")
    Rel(web_app, userc, "Uses", "JSON/HTTPS")
    Rel(3rd, integrationc, "Uses", "JSON/HTTPS")

    Rel(chatdao, db, "Uses")
    Rel(event, eventqueue, "Send events to")
    Rel_Back(eventqueue, notification_service, "Consume events from")
    Rel(storage, file_storage, "Send/get files from/to")

UpdateLayoutConfig($c4ShapeInRow="3")
    
```

## Abordagens arquiteturais
 - Contadores materializados (desempenho)
 - Utilização de protocolo full-duplex (desempenho)
 - Banco de dados não relacional (escalabilidade)
 - Cluster de servidores acessados por load balancer de alta disponibilidade (disponibilidade)
 - Utilização de serviço de armazenamento para uploads de arquivos
 - Redução da qualidade de vídeos enviados pelo app (desempenho);


## Riscos
 - Tamanho da mídia postada consumir consideravelmente pacote de dados da operadora de internet móvel do usuário;
 - Processamento local de vídeos comprometer desempenho ou bateria de smartphones com pouco recurso de hardware;
 - Time de desenvolvedores ter dificuldades em lidar com protocolo com o qual não possuem familiaridade;
 - Processamento de notificações de grupos que possuem muitos membros causar atraso no envio das notificações;

## Não-riscos
 - Usuários deixarem de utilizar o App;

## Pontos de sensibilidade
 - Quantidade de nós no cluster;

## Tradeoffs
 - O bit rate definido para os videos serem convertidos antes de serem enviados;


### APIs

TODO

### Data Storage

TODO

### Degree of constraint

TODO

## Alternatives considered

TODO

## Cross-cutting concerns
Segurança
Privacidade

## Authors
10/09/2020 - Criação. Autor: renato.ribeiro
