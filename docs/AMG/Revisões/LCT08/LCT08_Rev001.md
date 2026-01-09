# GUIA DE REVISÃO DE SOFTWARE – LCT08 (RSLogix 5000)

> **Objetivo geral:**  
> Revisar o software da LCT08 eliminando forças, jumpers, bits M0/M1, linhas desativadas e saídas duplicadas, restaurando a rastreabilidade de falhas e a segurança do processo – **sempre estudando o impacto antes de remover qualquer coisa**.

---

## 1. Método GERAL de análise (vale para todos os itens)

Antes de mexer em qualquer item do checklist, seguir este fluxo:

1. **Localizar no software**  
   - Seguir o caminho completo:  
     **Task → Programa → Rotina → Rung (Linha)**  
   - Entender em que parte do processo essa lógica atua (desenrolador, carro de carga, aplanadora, enderezador, mesa de despuntes, guiado, etc.).

2. **Entender a função original**
   - A lógica/bit é:
     - Uma **permissiva**?
     - Um **alarme**?
     - Um **recurso de segurança**?
     - Parte da **sequência de partida/parada**?
     - Apenas **indicação**?

3. **Descobrir por que foi alterado (jumpeado/forçado/bloqueado)**
   - Foi para “fazer rodar” em emergência?
   - Havia sensor defeituoso ou mal posicionado?
   - O intertravamento era exagerado e travava a produção?
   - Havia falha de ajuste (tempo, ganho, limite)?

4. **Conferir condição de campo**
   - O dispositivo ainda existe fisicamente?
   - Está ligado corretamente na borneira?
   - Fiação, conectores e sensores estão íntegros?
   - O estado real do processo bate com o que se esperava na lógica?

5. **Avaliar impacto de voltar ao comportamento correto**
   - Se eu tirar o jumper / force / M0 / M1:
     - Vai **travar a máquina**?
     - Vai **expor um problema real** (falha que deve aparecer)?
     - Vai **gerar um alarme necessário**?
     - Pode **causar movimento inesperado** se estiver mal parametrizado?

6. **Definir a solução correta**
   - Corrigir sensor/campo.
   - Ajustar o intertravamento (lógica, temporização, comparadores).
   - Reorganizar as permissivas.
   - Centralizar comando de saída duplicada em **uma única rung “mestre”**.
   - Atualizar parâmetros, setpoints e condições de liberação.

7. **Testar**
   - Testar em modo **manual**.
   - Testar em modo **automático**.
   - Testar **partidas e paradas**.
   - Testar **emergências e resets**.
   - Simular falhas quando for seguro fazê-lo.

8. **Documentar**
   - O que foi encontrado.
   - Causa raiz.
   - O que foi alterado (campo e software).
   - Resultado dos testes.
   - Responsável e data.

> **Regra de ouro:**  
> Nada é apagado ou desjumpeado “porque está feio”.  
> Tudo é estudado, corrigido e testado.

---

## 2. Conceitos por categoria

### 2.1 Forces (forçamento de entradas/saídas)

**O que é:**  
Um force substitui o valor real (do campo) por um valor fixo no CLP.

**Por que aparece:**
- Para contornar temporariamente falha de sensor ou fiação.
- Para “simular” condição em testes.
- Para derrubar intertravamento “incômodo”.

**Riscos:**
- Esconde problemas reais de campo.
- Em sinais de segurança, pode **invalidar totalmente a proteção**.
- Leva o software a mentir sobre o estado real da máquina.
- Dificulta diagnóstico futuro.

**Regra:**  
Force **não é solução permanente**. Sempre deve ter motivo registrado, tempo limitado e plano de remoção.

---

### 2.2 Jumpers (bits/instruções jumpeados)

**O que é:**  
Quando uma condição que deveria depender de sensores, cálculos ou permissivas passa a ser simplesmente forçada ON ou OFF (bit setado direto ou contato colocado em paralelo).

**Por que aparece:**
- Sensor defeituoso/bem posicionado.
- Intertravamento “duro demais” que travava a operação.
- Gambiarra para liberar movimento sem corrigir a causa.

**Riscos:**
- Lógica deixa de representar o processo real.
- Pode permitir que o equipamento opere **sem condições seguras**.
- Dificulta investigação de falhas.

**Regra:**  
Antes de remover jumper, é obrigatório entender **qual condição real ele está substituindo**.

---

### 2.3 Bits M0 / M1 (rungs bloqueadas artificialmente)

**O que é:**  
Uso de bits especiais (como M0 ou M1) para “matar” linhas inteiras de lógica, impedindo sua atuação.

**Por que aparece:**
- Para desligar intertravamento que causava parada.
- Para eliminar alarmes “chatos”.
- Para desativar partes da máquina sem atualizar o projeto oficial.

**Riscos:**
- Alarmes e intertravamentos importantes deixam de funcionar.
- Segurança pode estar comprometida.
- Operação passa a depender de memória tribal, não de software.

**Regra:**  
Nunca remover M0/M1 sem entender se a lógica original estava certa e apontava um problema real.

---

### 2.4 Saídas duplicadas

**O que é:**  
Mesmo endereço de saída (coil/tag) sendo comandado em mais de uma rung.

**Riscos:**
- Lógicas “brigando” pelo mesmo dispositivo.
- Erros intermitentes, difíceis de reproduzir.
- Atuação errática do campo (liga/desliga sem padrão claro).

**Regra:**  
Sempre deve haver **uma única rung “mestre”** por saída física. As demais condições viram permissivas internas.

---

### 2.5 Programas/rotinas sem uso

**O que é:**  
Códigos que existem no projeto, mas não são chamados por nenhuma Task/Rotina principal.

**Riscos:**
- Funções importantes podem estar desativadas sem ninguém saber.
- Confusão para quem faz manutenção.
- Aumento de complexidade sem benefício.

**Regra:**  
Decidir se o código é **legado (e deve ser claramente marcado)** ou **deve ser integrado corretamente** ao fluxo de execução.

---

### 2.6 Linhas desativadas

**O que é:**  
Rungs comentadas, bypassadas ou mantidas com bit “fechado apenas para deletar”.

**Riscos:**
- Intertravamentos e proteções projetadas podem ter sido desligados.
- Indefinição sobre o que realmente faz parte do controle.

**Regra:**  
Antes de apagar, entender se a função não é importante para segurança ou integridade de processo.

---

## 3. Checklist detalhado

> **Uso recomendado:**  
> Transformar cada lista abaixo em tabela (Excel, Planilha ou sistema) com colunas como:  
> *Status* / *Causa raiz* / *Correção* / *Responsável* / *Data de conclusão*.

---

### 3.1 Entradas forçadas

**Como revisar:**

1. Verificar se o force ainda está ativo.
2. Classificar a entrada (segurança, intertravamento, indicação).
3. Verificar condições de campo (sensor, cabo, bornes, atuadores).
4. Corrigir causa raiz.
5. Remover force e testar sequência.

**Itens (Task → Programa → Rotina → Linha / Endereço aproximado):**

- [ ] `GEN_PARO_EMERGENCIA_P2` – forçada **ON**  
      - Módulo: `PI:3:I.Data.22` (exemplo conforme mapeamento do TXT)  
      - Função: parada de emergência geral setor P2.
- [ ] `GEN_EMERGENCIA` – forçada **OFF**  
      - Módulo: `Local:8:I.Data.31`  
      - Função: cadeia de emergência geral.
- [ ] `Local:8:I.Data.31` – demais bits forçados (confirmar na Force Table).
- [ ] `Local:9:I.Data.17` – entrada forçada (permissiva ou intertravamento).
- [ ] `Local:9:I.Data.18` – entrada forçada.
- [ ] `P1:2:I.Data.13` – entrada forçada.
- [ ] `PI:3:I.Data.22` – (além de GEN_PARO_EMERGENCIA_P2, conferir demais bits).

> Para cada uma:
> - [ ] Identificada função original.  
> - [ ] Causa raiz determinada.  
> - [ ] Correção de campo aplicada.  
> - [ ] Force removido.  
> - [ ] Testes realizados e documentados.

---

### 3.2 Saídas duplicadas

**Como revisar:**

1. Fazer **cross-reference** da saída.
2. Listar todas as runGs onde ela é escrita.
3. Definir qual rung será a **mestre**.
4. Transformar as outras em lógica auxiliar/permissiva.
5. Testar funcionamento em todos os modos.

**Itens:**

- [ ] `P1:6:O.Data.25` – saída física com múltiplas bobinas.  
- [ ] `PV_GENERAL.13` – bit usado em mais de uma rotina/saída.  
- [ ] `PV_GENERAL.12` – idem.  
- [ ] `SICK:O2.Data[0].1` – saída de módulo SICK escrita em mais de um lugar.

> Para cada saída:
> - [ ] Mapeadas todas as bobinas no cross-reference.  
> - [ ] Definida rung mestre.  
> - [ ] Lógica reorganizada.  
> - [ ] Testes ok.

---

### 3.3 Jumpers (bits/instruções jumpeadas – bit a bit)

**Como revisar (passos específicos):**

1. Localizar a rung exata: **Task → Programa → Rotina → Rung (Linha)**.
2. Entender de onde o bit deveria vir (sensor, cálculo, intertravamento).
3. Encontrar a razão histórica do jumper.
4. Corrigir causa raiz (campo ou lógica).
5. Remover jumper, voltar a usar condição real e testar.

---

#### 3.3.1 T10_AUXILIARES

**Task:** (Principal da LCT08, conforme projeto)  
**Programa:** `T10_AUXILIARES`

- **Rotina:** `P10_APLANADORA2 > APLANADORA2`  
  - Rung 43:
    - [ ] `ap2_engr_cardans_mot` – jumpeado.
    - [ ] `ap2_engr_pinones_mot` – jumpeado.

- **Rotina:** `P09_APLANADORA1 > APLANADORA1`  
  - Rung 43:
    - [ ] `ap1_rod_tractor_abajo_det` – jumpeado.

- **Rotina:** `P07_ENDEREZADOR_CIZALLA > SUP_PV`  
  - Rung 1:
    - [ ] `tyg_rod_tractor_abajo_det` – jumpeado.

- **Rotina:** `P07_ENDEREZADOR_CIZALLA > ENDEREZADOR_CIZALLA`  
  - Rung 34:
    - [ ] `tyg_detec_cola_bobina_det` – jumpeado.  
    - [ ] `tyg_rod_tractor_abajo_det` – jumpeado.  
    - [ ] `tyg_cuchilla_arriba_det` – jumpeado.  
    - [ ] `end_bloque_arriba_det` – jumpeado.

- **Rotina:** `P05_GUIADO_AUTOMATICO > GUIADO_AUTOMATICO`  
  - Rung 31:
    - [ ] `emg_centrador_ok` – jumpeado.  
    - [ ] `des_rod_apoyo_arriba_det` – jumpeado.

- **Rotina:** `P03_DESENROLLADOR > DESENROLLADOR`  
  - Rung 31:
    - [ ] `SUP_ALARMA1.1` – jumpeado.

- **Rotina:** `P02_CARRO_CARGA > DESENROLLADOR`  
  - Rung 1:
    - [ ] `TAGDT50.3` – jumpeado.

---

#### 3.3.2 T11_VARIOS

**Programa:** `T11_VARIOS`  
**Rotina:** `P01_VARIOS > R01_ALARMAS`

- Rung 102:
  - [ ] `tra_automatico` – jumpeado.

---

#### 3.3.3 T04_REG_ALIMENTADOR

**Programa:** `T04_REG_ALIMENTADOR`  
**Rotina:** `P01_REG_TRATOR_ALIM_MOT_11M1 > R02_CONTROL`

- Rung 4:
  - [ ] `TRA_COND_MANU` – jumpeado.

---

#### 3.3.4 T01_REG_DESENROLLADOR

**Programa:** `T01_REG_DESENROLLADOR`  

- **Rotina:** `P01_REG_DESENROLLADOR_MOT_3M1 > R02_CON...`  
  - Rung 21:
    - [ ] `Desliga_tracao_desenrolador.DN` – jumpeado.

- **Rotina:** `P01_REG_DESENROLLADOR_MOT_3M1 > R02_CON...LE`  
  - Rung 16:
    - [ ] `Desliga_tracao_desenrollador.DN` – jumpeado.

(Usar o nome exato da rotina conforme projeto, o TXT registra abreviação.)

---

#### 3.3.5 T12_GENERAL – Instruções jumpeadas

**Programa:** `T12_GENERAL`  
**Rotina:** `P01_GENERAL > R01_MARCHAS`

- Rung 2:
  - [ ] Instrução `(LES) PV_APL1_ALT_SUP` – condição jumpeada.  
    - Rever critério de marcha que depende desse comparador.

---

### 3.4 Programas/rotinas sem uso

**Como revisar:**

1. Ver se a rotina está associada a alguma `Main Routine` de Task.
2. Ver se faz parte de especificação original (consultar documentação, fornecedor, integrador).
3. Decidir:
   - [ ] Ativar/Integrar corretamente.  
   - [ ] Declarar como “LEGADO / NÃO UTILIZADO” e documentar.

**Item identificado:**

- [ ] `T10_AUXILIARES > ENCODER_FACA` – programa/rotina sem uso.

---

### 3.5 Linhas desativadas

**Como revisar:**

1. Ver se a função da rung é de **segurança, parada, alarme ou sequência essencial**.
2. Ver por que foi desativada (equipamento removido, lógica incompleta, gambiarra temporária).
3. Se a função ainda é necessária:
   - Corrigir lógica e reativar.
4. Se não é mais necessária:
   - Documentar e remover de forma limpa, não apenas “comentar”.

**Item do TXT:**

- [ ] `T12_GENERAL > BARREIRAS > PORTAS_SEGURANCA > Rung 5` – linha desativada.

---

### 3.6 Lógicas impedidas por M0 (e similares)

**Como revisar (além do método geral):**

1. Identificar exatamente **qual bit (M0, M1 ou outro)** está bloqueando a rung.
2. Ver em que condições ele é setado/resetado.
3. Entender o problema original que levou alguém a usar esse bit.
4. Remover o bloqueio somente após corrigir a causa real.

---

#### 3.6.1 T12_GENERAL

**Programa:** `T12_GENERAL`

- **Rotina:** `P01_GENERAL > CRONOMETRO`
  - [ ] Rung 10 – marcada como “bit fechado, apenas deletar”  
        > Ainda assim, validar a função antes de remover.

- **Rotina:** `P01_GENERAL > R02_PARADAS`
  - [ ] Rung 0  
  - [ ] Rung 6  
  - [ ] Rung 8  
  - [ ] Rung 9  
  - [ ] Rung 12  
  - [ ] Rung 13  
  - [ ] Rung 15  
  - [ ] Rung 17  

---

#### 3.6.2 T13_RAMPA_S

**Programa:** `T13_RAMPA_S`  
**Rotina:** `P01_RAMPA_S > CONTROL_RAMPA`

- [ ] Rung 1 – condição de rampa bloqueada.

---

#### 3.6.3 T03_REG_APLANADORA2_17_63

**Programa:** `T03_REG_APLANADORA2_17_63`  
**Rotina:** `P01_REG_APLANADORA2_17_63_MOT_9M1 > R02_CONTROL`

- [ ] Rung 10 – bloqueada por bit (M0 ou similar).

---

#### 3.6.4 T01_REG_DESENROLLADOR

**Programa:** `T01_REG_DESENROLLADOR`

- **Rotina:** `P01_REG_DESENROLLADOR_MOT_3M1 > R01_INICIALIZACION`
  - [ ] Rung 2  
  - [ ] Rung 3  
  - [ ] Rung 4  

- **Rotina:** `P01_REG_DESENROLLADOR_MOT_3M1 > R02_CONTROLE`
  - [ ] Rung 14  

---

#### 3.6.5 T11_VARIOS

**Programa:** `T11_VARIOS`  
**Rotina:** `P01_VARIOS > R01_ALARMAS`

- [ ] Rung 02  
- [ ] Rung 09  
- [ ] Rung 10  
- [ ] Rung 15  
- [ ] Rung 19  
- [ ] Rung 20  
- [ ] Rung 25  
- [ ] Rung 97  
- [ ] Rung 109  
- [ ] Rung 135  
- [ ] Rung 137  
- [ ] Rung 138  
- [ ] Rung 139  
- [ ] Rung 140  
- [ ] Rung 141  

**Rotina:** `P01_VARIOS > R02_PV_SUP`

- [ ] Rung 06  

---

#### 3.6.6 T10_AUXILIARES – Carro de carga, Desenrolador, Guiado

**Programa:** `T10_AUXILIARES`

- **Rotina:** `P02_CARRO_CARGA > CARRO_DE_CARGA`
  - [ ] Rung 04  
  - [ ] Rung 07  

- **Rotina:** `P03_DESENROLLADOR > DESENROLLADOR`
  - [ ] Rung 14  

- **Rotina:** `P03_DESENROLLADOR > SUP_PV`
  - [ ] Rung 03  

- **Rotina:** `P05_GUIADO_AUTOMATICO > GUIADO_AUTOMATICO`
  - [ ] Rung 08  

---

#### 3.6.7 T10_AUXILIARES – Enderezador / Mesa Despuntes

- **Rotina:** `P07_ENDEREZADOR_CIZALLA > ENDEREZADOR_CIZALLA`
  - [ ] Rung 11  
  - [ ] Rung 16  
  - [ ] Rung 19  
  - [ ] Rung 31  

- **Rotina:** `P08_MESA_DESPUNTES > MESA_DESPUNTES`
  - [ ] Rung 05 – (TXT: “bit fechado, apenas deletar” – validar antes).  
  - [ ] Rung 08  

---

#### 3.6.8 T10_AUXILIARES – Aplanadora 1

- **Rotina:** `P09_APLANADORA1 > PRESET`
  - [ ] Rung 03  
  - [ ] Rung 04  
  - [ ] Rung 06  
  - [ ] Rung 07  
  - [ ] Rung 09  
  - [ ] Rung 10  
  - [ ] Rung 12  
  - [ ] Rung 13  
  - [ ] Rung 15  
  - [ ] Rung 16  
  - [ ] Rung 25  
  - [ ] Rung 26  
  - [ ] Rung 27  

- **Rotina:** `P09_APLANADORA1 > SUP_PV`
  - [ ] Rung 15  
  - [ ] Rung 30  

---

#### 3.6.9 T10_AUXILIARES – Aplanadora 2

- **Rotina:** `P10_APLANADORA2 > PRESET`
  - [ ] Rung 03  
  - [ ] Rung 04  
  - [ ] Rung 06  
  - [ ] Rung 07  
  - [ ] Rung 09  
  - [ ] Rung 10  
  - [ ] Rung 12  
  - [ ] Rung 13  
  - [ ] Rung 15  
  - [ ] Rung 16  
  - [ ] Rung 18  
  - [ ] Rung 19  
  - [ ] Rung 21  
  - [ ] Rung 22  
  - [ ] Rung 29  
  - [ ] Rung 31  
  - [ ] Rung 32  
  - [ ] Rung 33  

---

#### 3.6.10 T10_AUXILIARES – Tractor Alimentador / Cizalla Rotativa

- **Rotina:** `P12_TRACTOR_ALIMENTADOR > SUP_PV`
  - [ ] Rung 02  

- **Rotina:** `P12_TRACTOR_ALIMENTADOR > TRACTOR_ALIMENTADOR`
  - [ ] Rung 09 – (TXT: “bit fechado, apenas deletar” – validar antes).  

- **Rotina:** `P13_CIZALLA_ROTATIVA > CIZALLA_ROTATIVA`
  - [ ] Rung 04  

---

### 3.7 Lógicas forçadas por M1

**Como revisar:**  
Mesma lógica de M0: entender o motivo, corrigir a causa real, depois liberar.

**Item identificado:**

- **Programa:** `T10_AUXILIARES`  
  **Rotina:** `P06_ABRIDOR_R_APOYO > SUP_PV`
  - [ ] Rung 02 – travada por M1.

---

## 4. Encerramento da revisão

Ao final da revisão, garantir que:

- [ ] Todos os forces foram eliminados ou justificados com plano específico e prazo.  
- [ ] Todos os jumpers tiveram causa raiz tratada e lógica corrigida.  
- [ ] Nenhuma rung essencial permanece bloqueada por M0/M1 sem justificativa formal.  
- [ ] Saídas duplicadas foram centralizadas em uma rung mestre.  
- [ ] Programas/rotinas não utilizadas foram classificadas como LEGADO ou integradas.  
- [ ] Toda mudança foi testada e documentada.  
- [ ] Nova versão do projeto foi salva, versionada e arquivada corretamente.

> Este guia pode ser usado como padrão para futuras revisões de software na LCT08 e em outras linhas, sempre respeitando o fluxo:
> **entender → corrigir → testar → documentar**, nunca apenas “apagar gambiarra”.
