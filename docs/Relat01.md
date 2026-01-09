### ESTUDO DE REARRANJO DE MOTORES – LCT08 E LCT16  
**Objetivo:** reduzir o tempo de resposta em eventos de queima de motor, saindo de **dias** para **horas** (ou, no máximo, 2 turnos), por meio de um novo layout de motores e melhorias de base mecânica e manutenção.

---

#### 1. Contexto e objetivo do estudo

Após a queima de um motor na **LCT08**, houve dificuldade no restabelecimanto do equipamento à produção, além de parada de outro equipamento no processo;

- Um equipamento parou pela **queima do motor**.  
- O segundo equipamento parou por ser o **fornecedor do motor de substituição**, que foi removido dele para atender a linha crítica.

Esse cenário evidenciou:

- Alta **dependência de poucos motores específicos**.  
- **Baixa intercambialidade mecânica** entre equipamentos.  
- Impacto significativo em **produtividade** e **disponibilidade de linha**.

Diante disso, foi realizado um estudo para:

- Reaproveitar melhor os motores existentes.  
- Definir um **layout de motores reserva e originais** mais inteligente.  
- Adequar bases mecânicas e parâmetros de drive para permitir trocas mais rápidas.  

---

#### 2. Descrição do evento inicial – LCT08

A falha teve origem na **aplanadora 15/100 da LCT08**, cuja configuração original era:

- **Motor original LCT08 – Aplanadora 15/100**  
  - Velocidade nominal: **1.509 rpm**  
  - Potência: **190 kW**  
  - Dados de armadura/campo: **[400V - 523A / 310V - 5,89A]**

Diante da queima, optou-se por utilizar o motor da **aplanadora 15/150 da LCT16** como solução emergencial.

---

#### 3. Caso 1 – Motor 15/150 LCT16 realocado para a 15/100 LCT08

**Motor original da Aplanadora 15/150 – LCT16**  
- Velocidade nominal: **1.240 rpm**  

Este motor foi removido da LCT16 e instalado na **aplanadora 15/100 da LCT08**.

##### 3.1 Diferença de velocidade nominal

Comparando as velocidades nominais:

- Motor original LCT08: **1.509 rpm**  
- Motor da LCT16 realocado: **1.240 rpm**

A velocidade do motor original da LCT08 é:

- Diferença absoluta: 1.509 – 1.240 = **269 rpm**  
- Diferença relativa: 269 / 1.240 ≈ **21,7% a mais** que a velocidade do motor da LCT16.

> Ou seja, o motor da LCT16, instalado na LCT08, estava **cerca de 21,7% mais lento** do que o motor original daquela linha.

##### 3.2 Ajustes no drive e recuperação de produtividade

Para compensar a diferença de velocidade, foram realizados **ajustes de parâmetros no drive**, com aumento de **13% na referência de velocidade**, monitorando:

- Tensão  
- Corrente  
- Comportamento térmico e mecânico

**Resultados de produtividade da aplanadora 15/100 – LCT08:**

- Condição degradada inicial (após troca, antes de otimizar):  
  - **20 m/min**

- Condição após ajustes de drive:  
  - **27 m/min**

- Velocidade de referência original da linha:  
  - **30 m/min**

Cálculos de desempenho:

- Ganho de produtividade após ajuste:  
  - (27 – 20) / 20 = **35% de aumento** em relação à condição degradada.

- Recuperação da referência original:  
  - 27 / 30 = **90% da velocidade nominal de projeto**.

- Gap residual:  
  - (30 – 27) / 30 = **10% abaixo da velocidade original**.

> Na prática, com o aumento de parâmetros no drive e ajustes nas varíaveis de software, foi possível recuperar **90% da velocidade original da linha**, visando o menor impacto produtivo possível. Esses 10% só foram perdidos nos materiais que são processados pela **Aplanadora 15/100**, não impactando na produtividade dos materiais processados na **Aplanadora 17/63**, que manteve sua performance. Na prática, a perda de produtividade total com esse motor foi menor que 10%

---

#### 4. Caso 2 – Uso do motor reserva (676 rpm) na Aplanadora 15/150 LCT16

**Motor reserva (almoxarifado), utilizado na Aplanadora 15/150 – LCT16**  
- Velocidade nominal: **676 rpm**  
- Potência: **271 kW**  
- Corrente de armadura/campo: **16,2 A**  
- Medidas mecânicas: **diferentes do original** (adaptações necessárias)

Mesmo apresentando:

- **Velocidade nominal bem inferior** ao motor original da LCT16 (1.240 rpm),  
- E exigindo **adaptações mecânicas**,

foi possível atender à **velocidade máxima requerida do equipamento**, pois o **limitante de produtividade é a cisalha**, e não a capacidade máxima do motor.

##### 4.1 Comparativo de velocidade nominal – Motor original x motor reserva

- Motor original LCT16: **1.240 rpm**  
- Motor reserva instalado: **676 rpm**

Cálculo da diferença:

- Diferença absoluta: 1.240 – 676 = **564 rpm**  
- Diferença relativa (queda): 564 / 1.240 ≈ **45,5% inferior** à velocidade original.

Portanto:

- O motor reserva opera com cerca de **54,5% da velocidade nominal** do motor original.  
- Ainda assim, com ajustes via software, foi possível que a **velocidade máxima do equipamento (18 m/min)** fosse atendida.

> Isso evidencia que a **velocidade nominal de 1.240 rpm era bem superior à necessidade real do equipamento**, abrindo espaço para uso de motor de menor rpm como solução permanente, sem perda de produtividade na ponta (cisalha limitando).

---

#### 5. Proposta de novo layout de motores

O estudo sugere uma **reorganização do papel de cada motor**, visando:

- Reduzir paradas prolongadas por falta de motor compatível.  
- Criar uma **cadeia de reserva** mais inteligente.  
- Melhorar a **intercambialidade** entre equipamentos críticos (LCT08, LCT16, cisalha, LCLs, etc.).

##### 5.1 Motor 1 – 676 rpm / 271 kW / 752 – 16,2 A (Armadura – Campo)

- Situação atual:  
  - Instalado na **aplanadora 15/150 da LCT16** como solução de contingência.  

- Proposta:  
  - **Aproveitar o investimento** feito na base adaptada da LCT16.  
  - Passar a considerar esse motor como **motor original** da **aplanadora 15/150 da LCT16**.

Justificativa:

- A velocidade nominal de 676 rpm, mesmo sendo ~45,5% inferior à original, **atende plenamente a necessidade real**, limitada pela cisalha (18 m/min).  
- A base e adaptações já foram executadas, reduzindo esforço futuro em novas trocas.

##### 5.2 Motor 2 – 1.240 rpm / 200 kW / 558 – 14,6 A (Armadura – Campo)

- Situação atual:  
  - Motor originalmente da **aplanadora 15/150 da LCT16**, atualmente instalado na **aplanadora 15/100 da LCT08**.

- Proposta:  
  - Considerar sua relocação futura para:  
    - **Desenrolador do decapado**, caso seja reativado, **ou**  
    - **Reserva de motores** da mesma capacidade para outros equipamentos.

- Ação necessária:  
  - **Levantamento de medidas dimensionais e capacidade eletromecânica** dos motores da Cisalha da:  
    - LCL 4,5  
    - LCL 8,0  
    - Demais motores e equipamentos compatíveis
  - Objetivo: identificar quais equipamentos podem ser atendidos por esse motor como **reserva estratégica**.

##### 5.3 Motor 3 – 1.975 rpm / 190 kW / 490 – 12,9 A (Armadura – Campo)

- Situação atual:  
  - Motor originalmente do **Desenrolador do Decapado Mecânico**.

- Proposta:  
  - Utilizá-lo como **motor reserva** para:  
    - **Aplanadora 15/150 da LCT16**  
    - **Aplanadora 15/100 da LCT08**  
    - Outros motores com **mesmas dimensões e capacidades eletromecânicas** ou também com **bases mecânicas intercambiáveis e capacidades eletromecânicas abaixo** do que ele entrega.

- Recomenda-se:  
  - Estender o **estudo de compatibilidade** para as **LCLs**, visando incluir outros equipamentos potenciais que possam ser atendidos por este motor como reserva.

##### 5.4 Motor 4 – 1.509 rpm / 190 kW / 523 – 5,89 A (Armadura – Campo)

- Situação atual:  
  - Permanece como **motor original da aplanadora 15/100 da LCT08**.



---

#### 6. Recomendações mecânicas – Bases intercambiáveis

Para viabilizar trocas rápidas, recomenda-se:

- **Aplanadora 15/100 – LCT08**  
  - Desenvolver e padronizar **base intercambiável** entre:  
    - **Motor 4 (1.509 rpm)** e  
    - **Motor 3 (1.975 rpm)**

- **Aplanadora 15/150 – LCT16**  
  - Desenvolver e padronizar **base intercambiável** entre:  
    - **Motor 1 (676 rpm)** e  
    - **Motor 3 (1.975 rpm)**
- **Outros sub-equipamentos que possam receber o Motor 3 como reserva em um evento, como cisalhas, etc.**
- **Documentar medidas e dimensões mecâcnicas de todos os motores críticos para fácil tomada de decisão futura, em equipamentos que ainda não fizerem parte de nenhum estudo de intercambiabilidade**

Benefícios:

- Trocas mecânicas mais rápidas, com mínima adaptação no campo.  
- Possibilidade de **manobras de motores** entre equipamentos dentro de no máximo 1 dia.  
- Redução do tempo de parada de **dias para horas**, desde que o motor reserva esteja disponível e testado.

---

#### 7. Recomendações de manutenção e confiabilidade

Para que o plano seja sustentável, recomenda-se:

1. **Plano de Manutenção Preventiva específico para o Motor 3 (reserva):**  
   - Medição periódica de **grandezas elétricas** (tensão, corrente, resistência de isolamento).  
   - **Limpeza** interna e externa.  
   - Testes de funcionamento sob carga controlada, quando possível.

2. **Rotina de medições elétricas para todos os motores críticos do estudo:**  
   - Tensão de armadura e campo.  
   - Corrente de armadura e campo.  
   - Resistência de isolação (megômetro).  
   - Registro em histórico para acompanhar **tendências de degradação**.

3. **Inspeções visuais sistemáticas:**  
   - Verificação de sujidade excessiva.  
   - Presença de **faíscamento** na região do coletor.  
   - Condição de **isoladores e escovas**.  
   - Temperatura de Carcaça
   - Avaliação de ruídos anormais e vibração.

Benefícios:

- Possibilidade de Migrar atendimentos corretivos não programados, que acarretam perda de produção para atendimentos corretivos programados, onde paradas programadas sem perda de produção podem ser agendadas como finais de semana, por exemplo
- Redução de Custos de Manutenção, uma vez que monitorada uma degradação de performance técnica, serviços de reparos podem ser contratados em caráter não emergencial.
- Criação de Procedimentos e Documentações Padrão para troca de motores desses equipamentos

---

#### 8. Extensão do estudo para outros equipamentos

Recomenda-se estender a metodologia deste estudo para:

- Demais motores de **alto impacto produtivo**, bem como subequipamentos considerados críticos.  
- Linhas como **LCL 4,5 e LCL 8,0**, entre outras, visando:

  - Mapear **reservas compatíveis**.  
  - Definir **bases intercambiáveis padrão**.  


---

#### 9. Indicadores e próximos passos

**Indicadores sugeridos:**

- **Tempo médio de resposta à queima de motor (atual x após implantação)**  
  - Situação atual: > 4 dias para restabelecer 2 equipamentos.  
  - Meta: **horas** ou, no máximo, **2 turnos** para recompor a capacidade produtiva.

- **Disponibilidade de linha** (%).  
- **Número de trocas corretivas não planejadas x planejadas.**  
- **Número de motores com bases intercambiáveis padronizadas.**

**Próximos passos:**

1. Completar levantamento dimensional e elétrico dos motores da **cisalha, LCL 4,5, LCL 8,0** e demais linhas críticas.  
2. Detalhar desenhos das **bases intercambiáveis** propostas.  
3. Formalizar o **plano de manutenção preventiva** dos motores de reserva.  
4. Atualizar a **documentação técnica** dos equipamentos com o novo layout de motores (diagramas, listas de sobressalentes, procedimentos de troca).  

---

