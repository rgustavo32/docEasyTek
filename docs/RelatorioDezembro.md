# Relatório Mensal — Entregas e Fechamento (Dezembro/2025)

## 1) Visão Geral do Mês
Durante o mês, foram executadas atividades de **diagnóstico e mitigação de falhas crônicas**, **melhorias de confiabilidade em automação**, **atendimentos corretivos emergenciais**, além de **evoluções relevantes em projetos de energia e instrumentação**. As entregas tiveram foco em:
- aumento de **disponibilidade** e redução de falhas recorrentes,
- melhoria de **segurança operacional** (controle de parâmetros por senha),
- criação de base técnica para indicadores (ex.: **golpes/ton**, energia por setor),
- estabilização de rotinas e coleta de dados (**Modbus**), reduzindo perdas/intermitências.

---

## 2) Entregas por Projeto / Frente

### 2.1) 5013663 — Trepidação crônica LCT 2,5 (Esteira imantada / Servomotores)
**Diagnóstico (rodada de testes)**
- Executada bateria de testes alternando **Manual ↔ Automático**:
  - 40 alternâncias sem comando JOG: **0 ocorrências** de trepidação.
  - 5 testes com comando **JOG** antes do retorno ao Automático: **4 falhas**.
- Conclusão prática: a trepidação ocorre quando, ao rearmar em automático, existe **necessidade de correção de posição**; quando não há correção pendente, a trepidação não ocorre.
- Iniciado estudo aprofundado das lógicas **FC54 e FC116** para identificar a origem do comportamento.

**Solução desenvolvida para rearme em automático (resultado principal)**
- Desenvolvida lógica para eliminar a trepidação durante o rearme em automático:
  - retorno ao Automático realiza **movimento de referência**;
  - software **força um erro grande controlado** antes da requisição de posicionamento (ajuste do destino para **posição atual + 5000 pulsos**), reduzindo drasticamente a chance de “overcorrection” e ressonância.
- **Resultado:** falha de trepidação durante o rearme automático (principal fonte de ocorrência) **eliminada em 100%**.
- **Percepção operacional:** operadores relataram aproximadamente **90% de melhora** no comportamento geral.

**Atendimento corretivo (evidências adicionais e plano de ação)**
- Em atendimento com material **1000 mm** e empilhador operando a **150 m/min**, foi observada **correia frouxa** (solicitado reaperto), reforçando hipótese de operação acima da nominal contribuindo para afrouxamento.
- Observado que a ressonância inicia no acionamento dos **empurradores** (transferência para pallet), devido a pequeno movimento que desloca a posição atual do destino configurado.
- Reduzidas velocidades e validado funcionamento em ~**75 m/min** (com ajustes), demonstrando que **150 m/min não é necessário**.
- **Iniciado plano de limite de velocidade** para reduzir recorrência e desgaste mecânico.

**Ganhos possíveis / impactos**
- Redução significativa de paradas e intervenções por falha crônica.
- Aumento de estabilidade operacional no rearme e melhoria percebida pela operação.
- Base técnica para padronização de limites de velocidade e ajustes de processo (tensão/receitas).

**Pendências / próximos passos**
- Análise detalhada dos casos residuais que ocorrem **durante o processo em automático**, associados a:
  - velocidades acima do recomendado,
  - ajustes de empilhamento e referenciamento,
  - receitas específicas,
  - tensão das esteiras e possível afrouxamento por partidas acima da nominal.

---

### 2.2) Contador de golpes — Cizalha Rotativa LCT08 (base para “golpes/ton”)
- Iniciado estudo para determinar vida útil de facas por **golpes por tonelada processada** (suspeita de desgaste acelerado).
- Entrega do mês: implementação do **contador de golpes** na IHM (base do KPI futuro).
- Identificado desafio de contagem: rotina de **100 ms** perdia eventos quando sensor atuava por ~**80 ms**.
- Ajustada lógica para captura confiável do evento de golpe, eliminando perda de contagem por pulso curto.

**Ganhos possíveis / impactos**
- Base confiável para quantificar desgaste e comparar desempenho de facas.
- Redução de “achismo” na troca/afiação, permitindo critério por histórico.

**Pendências / próximos passos**
- Inclusão de **sensor/entrada de espessura** e **largura** da chapa para consolidar o indicador final **golpes/ton**.

---

### 2.3) Prensa 01 — Segurança e diagnóstico de contagem
**Segurança de parâmetros por senha (IHM antiga)**
- Alterada a IHM para permitir alteração de **parâmetros de processo apenas por senha**, prevenindo alterações indevidas e melhorando rastreabilidade operacional.
- Principal dificuldade: receitas armazenadas na IHM antiga, com backup delicado:
  - pen drive e HD externo não funcionaram;
  - solução viável foi **leitor de cartão CF**.
- Realizado **backup das receitas** e, em seguida, **backup da aplicação** antes da alteração.

**Diagnóstico de contagem de peças (falha intermitente)**
- Iniciado estudo da lógica de contagem para entender por que a Prensa 01 envia **peças a menos**.
- Mapeadas rotinas e ciclos do envelope de contagem baseados em **magnetização/desmagnetização** da esteira.
- Por ser defeito **intermitente**, definido acompanhamento por mais dias para fechar diagnóstico.

**Ganhos possíveis / impactos**
- Segurança operacional aumentada (redução de erro humano e mudanças indevidas).
- Base técnica construída para correção definitiva do erro intermitente de contagem.

---

### 2.4) 5013670 — Gerenciamento de Energia (SE03 — Fase 01)
**Evolução da coleta Modbus e escalabilidade**
- Desenvolvida versão **anti-colisão** para coleta no barramento Modbus.
- Desenvolvida função dinâmica para coleta contínua de **qualquer número** de multimedidores (replicável nas demais subestações).
- Corrigida falha de **reconexão** (não reconectava após timeout).
- Expandido range de coleta de **7 para 160** grandezas.
- Criada estrutura robusta para **loop contínuo** de coleta.
- Inseridos multimedidores da SE03 **#2 ao #7** no loop.

**Evoluções na aplicação / dados**
- Criado seletor de equipamentos: seleção de multimedidor para **isolar consumo por setor/linhas de corte**.
- Inclusão de consumo (kWh) no banco como **série temporal**.
- Criado gráfico de barras de consumo **agrupado por hora** (layout a definir).
- Criada flag de saneamento para evitar erros na análise (**overrange**, eliminação de **valores negativos**).

**Correção de divergência de medição (campo)**
- Notadas divergências nos dados pós-etapas da fase 01.
- Verificado que os **TCs estavam invertidos** na subestação; correção realizada via configuração do multimedidor, parâmetros ajustados e **reset dos contadores de energia**.

**Ganhos possíveis / impactos**
- Coleta mais estável e escalável (redução de perdas e travamentos no Modbus).
- Melhor qualidade de dados e maior confiança para decisões energéticas.
- Visibilidade de consumo por setor (base para gestão e redução de custos).

---

### 2.5) 5014930 — Atendimento corretivo (Mandril / Desenrolador — Desbobinamento)
- Atendimento corretivo em falha intermitente no desenrolamento:
  - após 2–3 JOGs funcionava; depois motor não acionava (corrente 0), porém o freio abria (bug de software).
  - em chapas finas não ocorria; em chapas >4 mm, “efeito mola” fazia bobina desenrolar.
- Identificada a condição que causava o bug e efetuada alteração no software.
- Operador relatou que o problema **não voltou a ocorrer**.

**Ganhos possíveis / impactos**
- Eliminação de risco de desenrolamento indevido (segurança e qualidade).
- Aumento de disponibilidade do desenrolador e redução de intervenções emergenciais.

---

### 2.6) Procedimentos / Treinamento — Cizalha Rotativa (Folga de faca) e LCT2,5
**5013677 — Procedimento de calibração da folga**
- Iniciada e mantida a criação do procedimento (tema complexo, exige alta precisão).
- Coleta contínua de informações detalhadas com o time.
- Desenvolvido **modelo matemático** para gerar imagens no plano cartesiano, para uso:
  - no procedimento técnico,
  - como recurso interativo no treinamento.

**5013669 — Alinhamento com equipe e calibração (LCT 2,5 / Folga de faca)**
- Seguido envolvimento com o time para coletar as principais dúvidas sobre o funcionamento da **folga de faca** em máquinas de corte transversal e esclarecer o sistema de calibração da **LCT 2,5**.
- Participaram: **José Augusto, Danilo, Francisco e Marinho**. As informações levantadas serão utilizadas para compor **procedimento** e **treinamento**.
- Foi identificado questionamento sobre responsabilidade **mecânica x eletrônica** no ajuste de calibre e definidas duas linhas de abordagem:
  1) criação de recurso na **IHM** para recalibração pós-troca de faca pelo mecânico (informando a nova medida da faca);
  2) criação de procedimento técnico, com **imagens**, detalhando como a máquina interpreta parâmetros e executa o ajuste conforme especificação.


**Ganhos possíveis / impactos**
- Padronização de setup, redução de variações e retrabalhos.
- Redução de tempo de ajuste e aumento de previsibilidade no processo.
- Transferência de conhecimento estruturada (procedimento + treinamento).
### 2.7) LCT08 — Troca de IHM obsoleta (PanelBuilder → FactoryTalk View / Rockwell)
- Concluída, juntamente com a equipe, a **substituição da IHM obsoleta** da LCT08 (PanelBuilder) por uma IHM atual **FactoryTalk View** (Rockwell).
- Executada a **conversão do software**, com **testes**, **comissionamento** e validação em campo.
- Realizados **ajustes solicitados pelos operadores** após entrada em operação (refino de usabilidade e comportamento).
- Realizado **treinamento prático** com o técnico para que a equipe tenha autonomia em repetir o procedimento (atividade não ficou dependente de suporte externo).

**Ganhos possíveis / impactos**
- Redução de risco de indisponibilidade por obsolescência (hardware/software sem suporte).
- Maior estabilidade e facilidade de manutenção (padrão atual Rockwell / FT View).
- Padronização e redução de tempo de atendimento futuro pela equipe interna treinada.

---

### 2.8) Rockwell — Procedimento de comunicação de IHM e transferência de conhecimento
- Criado **procedimento técnico** sobre **comunicação de IHM Rockwell** (padrão de configuração/boas práticas).
- Realizado envolvimento técnico e alinhamento com **dois técnicos** da equipe, reforçando padronização e disseminação do conhecimento.

**Ganhos possíveis / impactos**
- Redução de tempo de diagnóstico e comissionamento em futuras IHMs.
- Menor risco de erros de configuração e inconsistência entre equipamentos.
- Aumento de autonomia do time e padronização do método de trabalho.

---

### 2.9) LCL08 — Início da conversão de software de IHM obsoleta
- Iniciado o trabalho de **conversão do software** da IHM da **LCL08** (IHM obsoleta), preparando base para futura modernização/substituição.
- Estruturadas premissas e pontos críticos para garantir que o processo seja executado com segurança (mínimo risco na troca).

**Ganhos possíveis / impactos**
- Antecipação do risco de obsolescência e redução de impacto em paradas futuras.
- Base pronta para acelerar a etapa de testes e comissionamento quando a troca ocorrer.
- Maior previsibilidade na modernização (escopo mais claro e menos retrabalho).

---

### 2.10) LCT08 — Revisão de software e plano de ação de melhorias
- Realizada **revisão do software** da LCT08, com identificação de oportunidades de melhoria (processo, confiabilidade e manutenção).
- Criado **plano de ação** para correção das oportunidades levantadas, com direcionamento das próximas etapas de implementação.

**Ganhos possíveis / impactos**
- Redução de falhas recorrentes por ajustes preventivos e correções direcionadas.
- Melhoria de desempenho operacional e redução de intervenções corretivas.
- Aumento de previsibilidade e priorização técnica com plano estruturado.


---

## 3) Principais ganhos consolidados do mês (resumo executivo)
- **Eliminação (100%)** da trepidação no rearme automático da LCT 2,5, com **~90% de melhora** percebida pela operação.
- Avanço significativo em diagnóstico e mitigação de falhas residuais por processo (velocidade, tensão, receitas, empurradores).
- **Base confiável** para estudo de desgaste de facas na LCT08 via contagem de golpes (preparação para KPI “golpes/ton”).
- Fortalecimento do projeto de energia (SE03) com **coleta robusta e escalável**, maior qualidade dos dados e capacidade de isolamento de consumo por setor.
- Melhoria de **segurança operacional** na Prensa 01 (parâmetros por senha) com execução cuidadosa de backups em IHM antiga.
- Correção de falha crítica/intermitente no desenrolador, eliminando comportamento inseguro de desenrolamento por bug.

---

## 4) Pendências e próximos passos
- LCT 2,5:
  - fechar diagnóstico e mitigação dos casos de trepidação durante processo em automático;
  - consolidar plano de **limite de velocidade** e padronização de ajustes (tensão/receitas/referenciamento).
- LCT08:
  - incluir espessura e largura para concluir KPI **golpes/ton**.
- Prensa 01:
  - acompanhar mais dias e fechar diagnóstico do erro intermitente de contagem de peças.
- Energia (SE03 / replicação):
  - definir layout final do gráfico por hora e replicar arquitetura de coleta nas demais subestações.
- Procedimentos/treinamentos:
  - consolidar documento final de calibração de folga e preparar material do treinamento com apoio do modelo matemático.

---
**Responsável:** Rodolfo Silva  
**Período:** Dezembro/2025
