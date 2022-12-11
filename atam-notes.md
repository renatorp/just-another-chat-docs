 ## ATAM
 
 revela o quão bem uma arquitetura satisfaz dados atributos de qualidade,
  fornece uma visão de como estes atributos interagem e impactam uns com os outros.
  O objetivo principal é entender as consequências das decisões arquiteturais com relação aos requisitos arquiteturais do sistema.

 Após a análise das abordagens arquiteturais, teremos como saída do método os cenários priorizados, as questões elaboradas para entender e avaliar a arquitetura, a árvore de utilidade, os riscos, não-riscos, pontos de sensibilidade e os tradeoffs descobertos.

 1- Apresentação dos interesses de negócio: apresentação dos objetivos que motivam o desenvolvimento da arquitetura, as principais _restrições_ e os _requisitos_ que influenciam de forma significante na mesma;

 2- Apresentação da arquitetura: a arquitetura é apresentada através de _visões_(componente, módulo, implantação) relacionandas com os interesses de negócio; 

 3- Identificação das abordagens arquiteturais: São apresentadas as abordagens, padrões, estilos e táticas utilizadas na arquitetura; (baseando-se nos guias arquiteturais)

 4- Geração da árvore de utilidade: Os atributos de qualidade principais da aplicação como um todo são utilizados para identificar e priorizar os requisitos arquiteturais e cenários mais significativos (ASR's[requisitos significantes do ponto de vista de arquitetura]);

 5- Análise das abordagens arquiteturais: Baseando-se nos fatores prioritários identificados na etapa anterior, as abordagens arquiteturais que atendem a estes fatores são escolhidas e analisadas; ( as abordagens arquiteturais identificadas anteriormente e os cenários priorizados na etapa anterior são utilizados para examinar o quanto as decisões arquiteturais tomadas consideram a importância dos requisitos de qualidade. Isto é feito através da revisão das decisões de design e da identificação de riscos, pontos de sensibilidade e tradeoffs) Atributo de Qualidade | Cenário | Abordagem Arquitetural
 Após relacionar os cenários com as abordagens arquiteturais identificadas, um
conjunto de questões é elaborado a respeito de como cada abordagem irá atender aos respectivos atributos de qualidade relacionados.
Depois riscos, não riscos, pontos de sensibilidade e tradeoffs

 6- Brainstorm e priorização de cenários: Um conjunto de cenários é gerado pelos stakeholders para representar seus interesses e entendimento dos requisitos de qualidade. A lista de cenários gerados é priorizada e o resultado da priorização é comparado com os da árvore de utilidade. Se não houver correspondência entre eles, são adicionados à árvore;

 7- Analisar abordagens arquiteturais: A quinta etapa e repetida agora considerando os cenários identificados na etapa anterior;

 8- Apresentar resultados: Os resultados são apresentados baseando-se nas informações coletadas nas etapas anteriores (estilos, cenários, riscos, pontos de sensibilidade, tradeoffs, etc).

 O riscos são decisões que foram feitas mas que suas consequências não são totalmente conhecidas.

Pontos de sensibilidade são parâmetros na arquitetura no qual algum atributo de qualidade está altamente relacionado.

Um ponto de tradeoff é encontrado na arquitetura quando um parâmetro da arquitetura contém mais de um ponto de sensibilidade onde diferentes atributos de qualidade são afetados pela mudança do parâmetro.


## EXEMPLOS

### RISCOS
A quantidade de dados buscadas somadas à frequência de idas ao servidor comprometer a performance da aplicação como um todo.
O balanceador de carga é um ponto único de falha.

### NÃO RISCOS
A ferramenta de monitoramento dos servidores não gera nenhum impacto.

### Pontos de sensibilidade
S1. O tamanho dos dados que serão compactados para envio pela rede.
A quantidade de nós do cluster pode não ser o suficiente para atender a todas a filiais de um país ao longo do tempo.

### TRADEOFFS 
 EX: A utilização de tabelas temporárias requer criação de queries nativas, o que reduz a portabilidade do sistema.
 A utilização de um cliente desktop impacta na implantabilidade da aplicação, uma vez que requer a instalação do programa em todos os computadores.
