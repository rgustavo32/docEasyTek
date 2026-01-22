# Projeto — Procedimento e Treinamento  
## Calibração da Folga da Faca — Cizalha Rotativa — LCT 2,5

Este documento define os requisitos para desenvolvimento de um **procedimento técnico oficial** e de um **treinamento operacional** referentes à **calibração da folga entre facas da cizalha rotativa** da linha **LCT 2,5**, com aplicação conceitual também a máquinas de **corte transversal** com arquitetura semelhante.

O objetivo é garantir um método **padronizado, seguro, repetível e auditável**, reduzindo dependência de conhecimento tácito e prevenindo ajustes incorretos que possam comprometer qualidade, segurança e vida útil do equipamento.

---

## 1) Contexto Consolidado (Premissas Técnicas da Máquina)

As informações abaixo devem ser consideradas como **verdades de máquina** para este procedimento:

- A folga entre facas é obtida por **movimento axial do berço** (deslocamento ao longo do eixo).
- Relação mecânica fixa do conjunto:
  - **25 mm de movimento axial ⇒ 1 mm de movimento radial (folga)**.
- Existe recorrência de dúvidas na planta quanto à responsabilidade entre:
  - **Mecânica** (troca e medição das facas),
  - **Automação/Elétrica** (calibração e aplicação dos parâmetros),
  - **Operação** (validação do corte).
- Para resolver este problema, foram definidas duas frentes:
  1. **Procedimento técnico ilustrado**, explicando como o sistema interpreta os parâmetros e aplica o ajuste.
- O procedimento de calibração (referência interna: **5013677**) é considerado **complexo** e depende de informações detalhadas coletadas junto à equipe de manutenção.
- Houve envolvimento técnico direto com:
  - José Augusto  
  - Danilo  
  - Francisco  
  - Marinho  
  As dúvidas levantadas durante estes atendimentos devem ser incorporadas ao conteúdo do procedimento e do treinamento.
- O método de ajuste utiliza:
  - **Interpolação linear baseada em dois pontos**, relacionando folga desejada com variável de controle (posição/offset).
  - **Compensação Δb**, para absorver variações dimensionais após troca ou amolação das facas, evitando recalibração completa quando a geometria do conjunto não é alterada.
- O sistema possui **duas facas**, portanto:
  - O procedimento deve considerar o efeito combinado das variações dimensionais de ambas.
  - Não devem ser adotadas simplificações que considerem apenas uma faca.
- Existe um **modelo matemático em desenvolvimento** capaz de gerar:
  - gráficos em plano cartesiano,
  - visualização de offsets,
  - simulação de efeitos de parâmetros,  
  que deverá ser utilizado como apoio visual no procedimento e como recurso didático no treinamento.

---

## 2) Entregáveis Esperados

### 2.1 Procedimento Técnico (Manual de Manutenção)

O procedimento deverá conter, no mínimo:

- Objetivo e escopo de aplicação
- Pré-requisitos técnicos
- Regras de segurança
- Verdades de máquina (mecânica e controle)
- Papéis e responsabilidades:
  - Mecânica
  - Automação/Elétrica
  - Operação
- Passo a passo para:
  - calibração após troca de faca
  - calibração após amolação/usinagem
  - calibração após manutenção mecânica que afete folga
- Critérios de validação do ajuste
- Sintomas de ajuste incorreto e diagnóstico
- Ações corretivas recomendadas
- Checklist final de liberação do equipamento

---

### 2.2 Roteiro de Treinamento

O treinamento deverá contemplar:

- Definição de público-alvo:
  - mecânicos
  - eletricistas/automação
  - operadores
- Módulos sequenciais:
  - fundamentos mecânicos
  - fundamentos de controle
  - uso correto da IHM
  - validação do corte
- Objetivos de aprendizagem por módulo
- Exercícios práticos com exemplos reais
- Perguntas de checagem de entendimento
- Atividades de validação em máquina (quando aplicável)

---

### 2.3 Requisitos para Telas e Parâmetros de IHM

Deverá ser definido:

- Quais campos devem existir na IHM para calibração:
  - medidas de faca
  - folga desejada
  - offsets aplicados
- Limites mínimos e máximos permitidos
- Validações de entrada
- Perfis de acesso e necessidade de senha
- Fluxo operacional padronizado para:
  - pós-troca de faca
  - pós-manutenção
- Registro de histórico de calibração (log):
  - usuário
  - data/hora
  - valores aplicados

---

## 3) Modo de Desenvolvimento do Material

Antes da elaboração final do procedimento e do treinamento, deverá ser realizada uma etapa de **levantamento de lacunas técnicas**, com foco em:

- arquitetura de controle
- método real de medição em campo
- limites físicos do equipamento
- comportamento observado após ajustes

Somente após este levantamento o conteúdo final deverá ser consolidado.

Sempre que possível, o material deverá:

- utilizar figuras explicativas,
- apresentar exemplos numéricos,
- demonstrar consequências de ajustes incorretos,
- permitir replicação do método por qualquer técnico habilitado.

---

## 4) Informações Necessárias para Consolidação do Procedimento

As informações abaixo devem ser levantadas e confirmadas antes da finalização do treinamento.

### 4.1 Máquina, Hardware e Sinais

Devem ser confirmados:

- Plataforma de PLC (Siemens)
- Software de programação
- Plataforma de IHM
- Principais sinais envolvidos:
  - entrada de medida da faca
  - setpoint de folga
  - posição do berço
  - permissivos de calibração
  - alarmes de limite
- Existência de sensor físico de posição:
  - encoder
  - LVDT
  - posicionador
  - ou cálculo por contagem
- Existência de modo específico de calibração:
  - como é habilitado
  - quem pode habilitar

---

### 4.2 Mecânica e Metrologia

Devem ser definidos:

- Instrumentos utilizados para medição das facas
- Pontos de referência de medição
- Procedimento correto de medição
- Qual faca é medida:
  - superior
  - inferior
  - ambas
- Existência de:
  - medida nominal
  - medida atual pós-amolação
- Onde estas medidas são registradas atualmente
- Erros comuns de medição observados em campo

---

### 4.3 Método Matemático e Calibração

Devem ser claramente documentados:

- Quais são os dois pontos usados na interpolação linear
- Como esses pontos são obtidos em campo
- Definição clara das variáveis:
  - X (entrada)
  - Y (saída/controle)
- Cálculo correto do Δb considerando duas facas
- Limites máximos e mínimos permitidos:
  - para Δb
  - para ajuste final de posição

---

