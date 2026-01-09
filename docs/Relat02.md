# RELATÓRIO DE ANÁLISE DE CAUSA RAIZ  
**Máquina:** Prensa01 – Conjunto Desenrolador / Carro de Carga / Aplanadora  
**Local:** Área de Prensa  
**Elaborado por:** Rodolfo Silva   
**Data:** 13/11/2025

---

## 1. Objetivo

Este relatório tem como objetivo:

- Documentar o **defeito crônico** de “trancos” na região do mandril da Prensa01 durante o movimento do carro de carga.
- Apresentar o **caminho de análise** até a identificação da causa raiz.
- Registrar a **ação corretiva aplicada em campo** e suas limitações.
- Recomendar um **cenário ideal de tratamento do problema**, com foco em:
  - Gestão e rastreabilidade de instrumentos (pressostatos).
  - Boas práticas de calibração.
  - Conformidade com normas de qualidade e auditorias.

---

## 2. Descrição do problema

### 2.1 Sintoma percebido

Durante o atendimento à Prensa01 foi observado:

- A máquina apresentava **trancos na região do mandril**, onde a bobina de chapa se encontra.
- Os trancos ocorriam quando o **operador movimentava o carro de carga** (carro hidráulico do desenrolador).
- O fenômeno era claramente perceptível na retomada do processo após o movimento do carro.

### 2.2 Condição particular do defeito

Durante os testes foi identificado que:

- O problema **não ocorria** quando o carro de carga estava **com bobina (carga elevada – dezenas de toneladas)**.
- O problema se manifestava principalmente com o carro **sem carga** ou com carga reduzida.

Este comportamento reforçou a hipótese de **influência de dinâmica hidráulica e/ou mecânica** na origem do defeito.

---

## 3. Equipamentos envolvidos

- **Prensa01** – linha principal.
- **Desenrolador / Mandril** – região onde a bobina é fixada.
- **Carro de carga (hidráulico)** – responsável por posicionar a bobina no mandril.
- **Aplanadora** – dispositivo à frente do desenrolador, responsável por planar a chapa.
- **Pressostatos da Aplanadora** – responsáveis por informar ao sistema o estado de “Aplanadora Travada”.

---

## 4. Análise técnica e caminho até a causa raiz

### 4.1 Observação do comportamento dinâmico

Durante os testes em campo foi constatado:

1. Ao **movimentar o carro de carga**, a **Aplanadora interrompia seu funcionamento**.
2. Essa parada temporária gerava uma **folga entre o desenrolador e a Aplanadora**.
3. Ao soltar o seletor de movimento do carro, a Aplanadora **retomava o processo**, tracionando novamente o material.
4. No instante em que a folga era absorvida, ocorria o **tranco na região do mandril**.

Ou seja, o tranco era a consequência de uma **sequência de eventos**:

> Movimento do carro de carga → Aplanadora pausa → formação de folga → retomada da Aplanadora → tranco na absorção da folga.

### 4.2 Hipótese hidráulica

Pelo fato de o carro de carga ser **hidráulico** e o problema não ocorrer com carga pesada:

- Foi levantada a hipótese de **variação de pressão no circuito hidráulico** durante o movimento do carro, afetando intertravamentos da Aplanadora.
- Em condição de **maior massa (bobina pesada)**, o sistema tende a se comportar de maneira mais estável, amortecendo parcialmente variações de pressão.
- Em condição **sem carga**, o sistema fica mais sensível a pequenas oscilações, evidenciando defeitos de ajuste e calibração.

### 4.3 Investigação no programa (automação)

Ao analisar o programa de automação, verificou-se que:

- Durante o movimento do carro de carga, alguns **pressostatos envolvidos no intertravamento da Aplanadora** perdiam momentaneamente o sinal de **“Aplanadora Travada”**.
- Essa perda de sinal era interpretada pelo sistema como **condição insegura**, ocasionando a parada da Aplanadora.
- Ao cessar o movimento do carro de carga, os pressostatos voltavam a enviar o sinal de travamento, permitindo a retomada da máquina.

**Conclusão parcial:**  
O tranco não era um defeito isolado do mandril, mas sim um **efeito colateral de um intertravamento hidráulico/eletrônico**, causado pela **instabilidade do sinal dos pressostatos** da Aplanadora.

### 4.4 Causa raiz

Com base nos testes e na lógica de automação, a causa raiz foi identificada como:

> **Calibração inadequada / sensibilidade excessiva dos pressostatos responsáveis pelo sinal de “Aplanadora Travada”.**

O ponto de ajuste dos pressostatos (setpoint):

- Estava **muito próximo da pressão real de trabalho**.
- Qualquer variação momentânea de pressão, causada pelo movimento do carro de carga, era suficiente para **fazer o pressostato “desarmar”**.
- Isso desativava o sinal de travamento e **parava a Aplanadora**, gerando o efeito de folga e, posteriormente, os trancos.

Adicionalmente, foi verificado no histórico de manutenção que a **bomba hidráulica do circuito havia sido substituída alguns meses antes**. Existe a possibilidade de que a bomba atualmente instalada **não possua a mesma curva de performance** (vazão × pressão) da bomba original de projeto, alterando o perfil de pressurização do sistema e aproximando ainda mais o ponto de atuação do pressostato da faixa real de operação.

Diante disso, recomenda-se que sejam realizados **novos testes de calibração e validação do setpoint** considerando a **bomba atualmente instalada**, assegurando que o ajuste do pressostato esteja adequado às condições hidráulicas vigentes e garantindo estabilidade de processo e segurança operacional.


---

## 5. Ação corretiva aplicada em campo

Durante o atendimento, o manutentor executou a seguinte intervenção:

- Com o auxílio de uma **chave allen**, realizou um ajuste de aproximadamente **meia volta no parafuso de calibração do pressostato**.
- Após o ajuste empírico, o problema de trancos **deixou de se manifestar** nas condições testadas em campo.

### 5.1 Resultados imediatos

- O sintoma foi **eliminado** no curto prazo.
- A máquina passou a operar **sem trancos** na região do mandril ao movimentar o carro de carga.

### 5.2 Limitações da solução adotada

Embora eficaz na eliminação imediata do sintoma, essa abordagem apresenta limitações importantes:

- **Ausência de referência:** não foi utilizada nenhuma medida de pressão (manômetro ou instrumento calibrado) para determinar o novo setpoint.
- **Falta de rastreabilidade:** o ajuste ficou registrado apenas na prática (“meia volta para dentro/para fora”), sem valor numérico em bar/psi.
- **Dependência de pessoa:** a “solução” fica na memória de quem ajustou, e não no **sistema de gestão de manutenção/instrumentação**.
- **Risco de desvio de projeto:** não há garantia de que o novo ponto de ajuste:
  - Atenda às especificações originais do fabricante.
  - Mantenha o mesmo nível de segurança funcional.

---

## 6. Riscos e impactos de um ajuste não controlado

### 6.1 Riscos de processo

- Possibilidade de o problema **retornar** caso haja:
  - Variações de carga.
  - Alterações de viscosidade do óleo (temperatura).
  - Substituição futura de componentes hidráulicos.
- Potencial de surgirem **novos efeitos colaterais**, como:
  - Aplanadora deixando de parar quando de fato deveria.
  - Sobrecarga mecânica em componentes a jusante.

### 6.2 Riscos para manutenção e segurança

- Sem valor nominal de setpoint, não há garantia de que o pressostato esteja ajustado **dentro da faixa segura de operação**.
- Em alguns cenários, ajuste excessivo pode:
  - Exigir pressões maiores que o previsto, estressando o sistema hidráulico.
  - Reduzir margens de proteção em situações de falha real.

### 6.3 Riscos de conformidade e auditoria

Em ambientes com **certificação de qualidade (ex.: ISO 9001) e/ou exigências específicas de clientes**, ajustes não documentados podem gerar:

- **Não conformidades** em auditorias internas e de clientes.
- Questionamentos sobre:
  - Controle de **instrumentos de medição e monitoramento**.
  - Gestão de **mudanças de processo** (Management of Change).
  - **Rastreabilidade** de intervenções em dispositivos críticos.

### 6.4 Riscos de perda de conhecimento

- Sem cadastro formal do instrumento e sem registro da calibração:
  - **Histórico de falha e solução** se perde com o tempo.
  - Cada novo problema tende a ser tratado como “caso novo”, elevando custo e tempo de diagnóstico.
  - A empresa fica **dependente de memória informal** de colaboradores.

---

## 7. Cenário ideal – Tratamento recomendado

### 7.1 Gestão de instrumentos (pressostatos)

Recomenda-se a implementação de um **sistema mínimo de gestão de instrumentos**, contemplando:

1. **Cadastro formal de instrumentos críticos**, incluindo:
   - Código interno / TAG (ex.: PRS-APLAN-01).
   - Função (ex.: “Confirmação de Aplanadora Travada”).
   - Localização na máquina.
   - Fabricante, modelo, faixa de operação e histerese.
   - Tipo de contato (NA/NF) e lógica no CLP.

2. **Classificação por criticidade**, priorizando:
   - Instrumentos que geram intertravamentos de segurança.
   - Instrumentos que impactam qualidade e continuidade de processo.

3. **Histórico de intervenções**, contendo:
   - Data da intervenção.
   - Técnico responsável.
   - Descrição do ajuste/calibração.
   - Valor de setpoint antes/depois (quando aplicável).
   - Motivo da intervenção (ex.: defeito crônico – trancos no mandril).

### 7.2 Procedimento sugerido para calibração de pressostatos

A seguir, um exemplo de **procedimento robusto** para calibração desse tipo de pressostato:

1. **Consulta a documentação técnica**
   - Verificar no manual do fabricante e no projeto da máquina:
     - Faixa de pressão recomendada.
     - Pressão de disparo (setpoint) nominal.
     - Histerese típica (diferença entre liga/desliga).

2. **Preparação do sistema**
   - Garantir condições seguras de trabalho (bloqueios, sinalização, NR-12).
   - Instalar **manômetro/calibrador hidráulico** em ponto adequado do circuito (preferencialmente previsto em projeto).
   - Colocar a máquina em modo de teste/simulação, quando disponível, para evitar produção de refugo.

3. **Ajuste controlado**
   - Com o circuito pressurizado, aumentar ou reduzir pressão lentamente até o ponto em que:
     - O pressostato muda de estado (**ponto de atuação**).
   - Registrar o valor lido no manômetro.
   - Ajustar o parafuso de calibração em passos pequenos, sempre:
     - Lendo a nova pressão de atuação.
     - Conferindo no IHM/CLP o momento exato em que o bit de “Aplanadora Travada” comuta.

4. **Validação dinâmica**
   - Repetir o teste **simulando a condição real de processo**, ou seja:
     - Movimentar o carro de carga.
     - Monitorar a pressão e o status do pressostato em tempo real.
   - Confirmar que, nas variações dinâmicas normais:
     - O pressostato **permanece estável** (sem perda intermitente do sinal).
     - O sistema cumpre sua função de proteção em situações de falha real.

5. **Registro e rastreabilidade**
   - Documentar:
     - Valor de pressão final de ajuste (em bar/psi).
     - Tolerância aceitável (±X%).
     - Condições em que o teste foi realizado (com/sem carga, temperatura aproximada do óleo).
   - Atualizar o cadastro do instrumento e o **plano de manutenção preventiva**:
     - Periodicidade recomendada de verificação (ex.: anual ou conforme criticidade).

6. **Treinamento e padronização**
   - Padronizar o procedimento em um **POP (Procedimento Operacional Padrão)**.
   - Treinar a equipe de manutenção para:
     - Evitar ajustes empíricos sem medição.
     - Sempre registrar qualquer alteração em instrumentos críticos.

### 7.3 Recomendações complementares

- Avaliar, em conjunto com engenharia, a possibilidade de:
  - Inserir **acumuladores hidráulicos** ou melhorias no circuito para reduzir picos/dips de pressão (caso ainda necessário).
  - Revisar lógica de intertravamento a fim de:
    - Filtrar sinais muito intermitentes (uso de tempos de validação, histerese lógica, etc.), sem comprometer a segurança.

- Durante a análise deste caso ficou evidente que a **ausência de um alarme claro na IHM para falha/instabilidade dos pressostatos da Aplanadora** dificultou a identificação rápida do defeito. O sintoma era percebido mecanicamente (tranco no mandril), mas não havia uma indicação direta na IHM de que o sinal de “Aplanadora Travada” estava sendo perdido intermitentemente, o que aumentou o tempo de diagnóstico.

- Como melhoria no **plano de ação**, recomenda-se:
  - **Revisar a filosofia de alarmes da IHM da Prensa01**, incluindo:
    - Criação de um **alarme específico** para perda do sinal de “Aplanadora Travada” (por tempo superior a um limite configurado).
    - Exibição na IHM do **status individual dos pressostatos críticos**, com identificação clara (TAG, descrição e condição ATIVO/INATIVO).
    - Registro desses alarmes em **histórico de eventos**, permitindo rastreio em futuras análises de falha.
  - Formalizar essa melhoria no **plano de manutenção e no procedimento de análise de falhas**, de modo que, em ocorrências futuras, a equipe tenha:
    - Indicação visual imediata de pressostato em condição anômala.
    - Base de dados histórica para correlação entre eventos hidráulicos, alarmes e condições de processo.

- Inserir esse evento no **histórico de análise de falhas recorrentes**, classificando como:
  - **Defeito crônico** relacionado à calibração de instrumentação hidráulica, gestão de mudanças e **melhorias de alarmística/IHM**.

---

## 8. Conclusão

A análise realizada na Prensa01 identificou que o problema de **trancos na região do mandril** estava diretamente relacionado a:

> **Perda intermitente do sinal de “Aplanadora Travada”, causada por calibração inadequada dos pressostatos responsáveis por esse intertravamento, sensíveis às variações de pressão durante o movimento do carro de carga.**

A intervenção de campo (ajuste de meia volta no parafuso de calibração) foi eficaz para eliminar o sintoma imediato, porém:

- Não garante rastreabilidade do ponto de ajuste.
- Não assegura, por si só, conformidade com práticas recomendadas de gestão de instrumentos.
- Pode gerar questionamentos em futuras **auditorias de qualidade** ou de clientes.

Recomenda-se fortemente que o cliente:

1. **Implemente um controle formal de instrumentos** (especialmente pressostatos críticos).
2. **Estabeleça um procedimento padronizado de calibração**, com uso de instrumentos de referência e registro dos valores.
3. **Inclua esse tipo de evento em sua rotina de análise sistêmica de falhas**, evitando soluções puramente empíricas.

Dessa forma, além de resolver o defeito crônico observado, a empresa avança em **maturidade de manutenção, confiabilidade de processo e governança sobre suas variáveis críticas**, reduzindo risco operacional e fortalecendo sua imagem perante clientes e auditorias.

---
