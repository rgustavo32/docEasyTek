# 📘 Procedimento Simples para Ajuste de Relação entre Motores  
*(Explicação clara, simples e para técnicos leigos)*

## 🧩 1. O que é a **relação**?
A “relação” é apenas um **fator multiplicativo** usado para ajustar a velocidade de cada motor para que *todos fiquem sincronizados*.  
- Se a relação = **1.0**, significa que o motor está rodando na **velocidade de referência**.  
- Se a relação é **maior que 1.0**, o motor está **mais rápido**.  
- Se a relação é **menor que 1.0**, o motor está **mais lento**.

---

## 🎯 2. Quando devo ajustar?
Quando **um dos motores** estiver rodando com velocidade diferente dos demais.  
O objetivo é calcular **quantos % essa diferença representa** e ajustar os outros motores para acompanhar.

---

## 🔍 3. Como descobrir **qual motor está diferente**?
Observe na Watch List (como no exemplo):

| Tag                      | Valor |
|-------------------------|--------|
| APL_RELACION_REDUCTORA  | 1.00   |
| DES_RELACION_REDUCTORA  | 0.90   |
| END_RELACION_REDUCTORA  | 0.52   |
| TRA_RELACION_REDUCTORA  | 0.46   |

- O **motor com relação = 1.00** está servindo como **referência**.  
- Os outros estão **mais lentos**, porque o valor é menor que 1.

---

## 📐 4. Como calcular a diferença (%)?

A fórmula é SIMPLES:

