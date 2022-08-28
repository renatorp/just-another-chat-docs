## Context and scope

Overview do que está sendo feito e dos porquês

## Goals and non-goals

Pequela lista de bullet points descrevendo quais são os goals e non-goals

## The actual design

Começar com overview e depois entrar nos detalhes. Descrever trade-offs feitos. Sugerir soluções e mostrar porque uma solução particular satisfaz os "goals"

Boas práticas e repeating topics que tem feito sentido em uma grande porcentagem de design docs:

### System-context-diagram

Esse diagrama mostra o novo sistema como parte de um conjunto de sistemas diferentes, e permite que os leitores contextualizem o novo design dento do ambiente existente e já familiarizado.

### APIs

Se o sistema expõe uma api, fazer um esboço de quais serão essas apis é uma boa ideia.

### Data Storage

Descrever de que forma que os dados serão armazenados.

### Code and pseudo-code

Design doc raramente deve ter código ou pseudo-código, a menos que seja um algorítimo inovador.

### Degree of constraint

Enumerar o que pode ser feito e como tudo isso pode ser combinado para alcançar o objetivo. Podem existir múltiplas soluções, mas nenhuma delas é ótima, assim o documento deve focar em selecionar a melhor dado um conjunto de trade-offs.

## Alternatives considered

Essa sessão lista designs alternativo que poderiam alcançar resultados similares. Focar nos trade-offs que cada uma das alternativas e em mostrar como isso levou à decisão de selecionar o design que é o tópico primário do documento.

Mostrar explicitamente porque a solução selecionada é melhor para alcançar os objetivos do projeto, e como outras soluções introduzerm trade-offs que não são desejáveis

## Cross-cutting concerns

Garantir que cross-cutting concerns estão sendo considerados, tais como segurança, observabilidade e privacidade. Essa é uma sessão curta que descreve como o design impacta o concern e como o concern é endereçado.


## Title and People

Título, autores e revisores, e a data de última atualização do documento.


## DESIGN DOCS - MAINTENANCE
As the author, do yourself a favor and re-read your own design docs a year or 2 later. What did you get right? What did you get wrong? What would you do to decide differently today? Answering these questions is a great way to advance as an engineer and improve software design skills over time.
