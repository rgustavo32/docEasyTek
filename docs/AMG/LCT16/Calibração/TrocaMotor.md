# Procedimento Prático: Ajuste e Sincronização de Motores
**Autor:** Rodolfo Silva  
**Data:** 7 de Outubro de 2025  
**Máquina:** LCT16
## 1. Objetivo

Auxiliar no procedimento de troca de motor, cálculo dos fatores de relação e ajuste de sincronismo para relação de velocidade da linha entre motores de tração

---

## 2. Exemplo Prático: 

**Motor Aplanadora:**  
Motor anterior: 1240 RPM
Motor Atual: 676 RPM

### Dados de Relação no Programa
| Nome do Motor              | Fator de Relação Atual |
| -------------------------- | ---------------------- |
| `APL_RELACION_REDUCTORA`   | **1.00**               |
| `DES_RELACION_REDUCTORA`   | **1.66**               |
| `END_RELACION_REDUCTORA`   | **0.96**               |
| `TRA_RELACION_REDUCTORA`   | **1.00**               |

Para sabermos a diferença em porcentagem, devemos dividir a velocidade do motor que irá entrar pela velocidade do motor que saiu:

```
ΔVel = RPM_MotorAtual / RPM_MotorAnterior
     = 676 / 1240
     ≈ 0.545
```

Isso mostra que o motor novo representa aproximadamente 0.545 ou 54.5% da velocidade do motor anterior. Sendo assim, para que os outros motores trabalhem em sincronismo com o motor da Aplanadora


#### Dados da Configuração Original (Motor de Referência)

*   **Motor de Referência:** `APL_RELACION_REDUCTORA`
*   **Fator de Relação Ideal:** `1.0`

Este é o nosso "motor mestre". Todos os outros devem seguir o ritmo dele.

#### Dados da Situação Atual (Motores com Problema)

Ao inspecionar a máquina, encontramos os seguintes valores na tela de parâmetros:

| Nome do Motor              | Fator de Relação Atual |
| -------------------------- | ---------------------- |
| `APL_RELACION_REDUCTORA`   | **1.0** (Correto)      |
| `DES_RELACION_REDUCTORA`   | **0.9** (Incorreto)    |
| `END_RELACION_REDUCTORA`   | **0.52** (Incorreto)   |
| `TRA_RELACION_REDUCTORA`   | **0.46** (Incorreto)   |

**Diagnóstico:** Os motores `DES`, `END` e `TRA` estão fora de sincronia com o motor principal `APL`.

---

## 3. Calculando a Diferença de Velocidade

Para entender *o quanto* um motor está desalinhado, calculamos a diferença em porcentagem. O sinal (positivo ou negativo) nos diz se ele está mais rápido ou mais lento.

**Fórmula:** `Diferença (%) = ( (Valor Atual / Valor de Referência) - 1 ) * 100`

Vamos usar o motor **`DES_RELACION_REDUCTORA`** (valor `0.9`) como exemplo:

1.  **Dividir o valor atual pelo de referência:**
    `0.9 / 1.0 = 0.9`

2.  **Subtrair 1 do resultado:**
    `0.9 - 1 = -0.1`

3.  **Multiplicar por 100 para obter a porcentagem:**
    `-0.1 * 100 = -10%`

**Conclusão do Cálculo:** O motor `DES` está operando **10% mais lento** que o motor de referência. O sinal de menos (`-`) indica que a velocidade é menor.

---

## 4. Determinando Quais Motores Alterar

O objetivo é fazer com que todos os fatores de relação voltem a ser **1.0**.

*   O motor `APL_RELACION_REDUCTORA` já está com o valor `1.0`, então **não mexemos nele**.
*   Os motores `DES`, `END` e `TRA` estão com valores diferentes de `1.0`. **Estes são os motores que precisam de ajuste.**

---

## 5. Alterando as Relações (Passo a Passo)

Agora, vamos corrigir os valores no sistema.

1.  Acesse a tela de parâmetros ou a "Watch List" do controlador da máquina.
2.  Navegue até o parâmetro do primeiro motor a ser corrigido.

    *   **Motor:** `DES_RELACION_REDUCTORA`
    *   Valor Atual: `0.9`
    *   **Ação:** Altere o valor para **`1.0`**.

3.  Repita o processo para os outros motores.

    *   **Motor:** `END_RELACION_REDUCTORA`
    *   Valor Atual: `0.52`
    *   **Ação:** Altere o valor para **`1.0`**.

    *   **Motor:** `TRA_RELACION_REDUCTORA`
    *   Valor Atual: `0.46`
    *   **Ação:** Altere o valor para **`1.0`**.

4.  Após alterar todos os valores, **salve ou confirme as modificações** no sistema.

---

## 6. Testando e Validando a Correção

Após salvar as alterações, é fundamental verificar se o ajuste funcionou.

1.  **Monitore a Tela de Parâmetros:** Verifique se os valores de todos os motores (`APL`, `DES`, `END`, `TRA`) agora exibem **`1.0`**.
2.  **Observação Visual e Sonora:** Ligue a máquina em baixa velocidade e observe se os componentes movidos pelos motores estão se movendo de forma sincronizada. Preste atenção a ruídos anormais, trancos ou vibrações, que podem indicar que o problema não foi totalmente resolvido.
3.  **Teste de Produção:** Se possível, realize um ciclo de operação completo para garantir que a sincronia se mantém sob condições normais de trabalho.

Se todos os testes forem bem-sucedidos, o procedimento está concluído e a máquina está sincronizada.
