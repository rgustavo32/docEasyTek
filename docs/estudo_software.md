# Guia de Diagnóstico Rápido: Sistema de Controle de Tensão

Este documento resume o fluxo de dados e a lógica do sistema, destacando os registros-chave para monitoramento e diagnóstico dos problemas relatados (correção excessiva, nenhuma correção, etc.).

---

## Bloco 1: Geração do Sinal de PV (Célula de Carga)

Esta seção descreve como o sinal bruto da célula de carga é lido e preparado para o cálculo.

### Resumo do Fluxo:

1.  O valor "bruto" da célula de carga é lido na entrada analógica (`1004`).
2.  Este valor é movido para um registro auxiliar (`7002`) para ser usado em um cálculo de 32 bits.
3.  Um cálculo complexo de escalonamento é realizado (`MULBL`, `DVBLL`).
4.  O resultado final (`415C`) é movido para o registro de PV do PID (`5030`), que é o valor exibido na IHM.

### Pontos-Chave e Diagramas:

*   **Verificar a Leitura da Entrada Analógica:**
    *   **O que observar:** O valor no registro `1004` deve variar de forma estável e coerente com a tensão física do papel.
    *   **Ponto de falha:** Se `1004` estiver travado, zerado ou instável, o problema é no hardware (célula de carga, fiação, cartão de entrada).

*   **Verificar a Preparação para o Cálculo:**
    *   **O que observar:** O valor de `1004` deve ser copiado corretamente para `7002`.
    *   **Diagrama Relevante (Linha 32):**
```
|----[ 0F7 ]----( MOV )----|
|             | OP1: 1004 | // Origem: Leitura bruta da célula de carga
|             | OP2: 7002 | // Destino: Parte alta do operando do cálculo
|
```

*   **Verificar o Resultado do Escalonamento:**
    *   **O que observar:** O registro `5030` deve ter o valor final do PV, já em unidade de engenharia. Ele deve ser proporcional a `1004`.
    *   **Ponto de falha:** Se `1004` varia mas `5030` não, o problema está na complexa cadeia de cálculos matemáticos (`MULBL`, `DVBLL`) entre eles.

---

## Bloco 2: Ação do Controle PID

Esta seção descreve como o "cérebro" do sistema funciona, comparando o que ele "quer" (SP) com o que ele "vê" (PV).

### Resumo do Fluxo:

1.  O bloco `PID_I` lê o **PV** (`5030`) e o **Setpoint (SP)** (`4100`).
2.  Ele calcula o **Erro** (SP - PV) e determina a ação de controle.
3.  O resultado é escrito na **Saída do PID** (`4130`).

### Pontos-Chave e Diagramas:

*   **Verificar as Entradas do PID:**
    *   **O que observar:** Os valores em `4100` (SP, vindo da IHM via `5020`) e `5030` (PV) devem estar na mesma escala. É a comparação entre eles que gera a ação.
    *   **Diagrama Relevante (Linha 45):**
```
|----[ H ]----( PID_I )----|
|             | Ent: 5030 | // O que o sistema VÊ (PV)
|             | Dad: 4100 | // O que o sistema QUER (Ponteiro para SP, Kp, etc.)
|             | Saí: 4130 | // O que o sistema DECIDE fazer
|             | Rst: /0F7 | // Reset, atualmente, NUNCA é acionado
|
```

*   **Verificar a Saída do PID:**
    *   **O que observar:** O registro `4130` deve reagir ao erro. Se PV < SP, `4130` deve aumentar. Se PV > SP, `4130` deve diminuir.
    *   **Ponto de falha (Integral Windup):** Se PV > SP mas `4130` continua no máximo (causando a "over correction"), é um sinal clássico de erro integral acumulado, pois o Reset (`Rst`) nunca é ativado. **A ausência de um reset é um forte candidato à causa do problema.**

---

## Bloco 3: Escalonamento e Escrita na Saída Analógica

Esta seção descreve como a decisão do PID é modulada pela velocidade da máquina e enviada para o atuador.

### Resumo do Fluxo:

1.  A **Saída do PID** (`4130`) é combinada com o **Fator de Velocidade** (`7200`).
2.  O sistema multiplica (`MULTB`) a ação do PID pelo fator de velocidade.
3.  O resultado passa por uma divisão (`DVBLL`) e um escalonamento final (`SCL`).
4.  O valor final (`4030`) é movido para a **saída analógica** (`1006`).

### Pontos-Chave e Diagramas:

*   **Verificar o Fator de Velocidade:**
    *   **O que observar:** O registro `7200` deve refletir a velocidade da encadernadora.
    *   **Ponto de falha:** Se `7200` estiver incorreto, a modulação da saída será errada, causando correções excessivas ou insuficientes dependendo da velocidade.

*   **Verificar o Cálculo Adaptativo (A "Mágica" do Sistema):**
    *   **O que observar:** O resultado da multiplicação em `4150` deve ser o produto da ação do PID (`4140`) pela velocidade (`4142`).
    *   **Diagrama Relevante (Linha 53):**
```
|----[ H ]----( MULTB )----|
|             | OP1: 4140 | // Ação do PID
|             | OP2: 4142 | // Fator de Velocidade
|             | OP3: 4150 | // Resultado combinado
|
```

*   **Verificar o Valor Final Antes da Saída:**
    *   **O que observar:** O registro `4030` é o valor final calculado. É o ponto mais importante para diagnosticar a lógica de controle como um todo.

*   **Verificar a Escrita na Saída Física:**
    *   **O que observar:** O registro `1006` deve ser idêntico a `4030`.
    *   **Ponto de falha:** Se `1006` for diferente de `4030`, significa que uma das lógicas de sobrescrita (linhas 62 ou 63) está ativa, forçando um valor manual ou de segurança.
    *   **Diagrama Relevante (Linha 61):**
```
|----[ H ]----( MOV )----|
|             | OP1: 4030 | // Valor final, calculado e escalonado
|             | OP2: 1006 | // Destino: Saída analógica física
|
```
