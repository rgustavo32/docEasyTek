# Procedimento de Calibração de Folga de Faca (Knife Gap) - Cizalha Rotativa LCT08

**Autor:** Rodolfo Silva  
**Data:** 30 de Outubro de 2025  
**Máquina:** LCT08

---

## 1. Objetivo

Estabelecer um procedimento padronizado para a calibração da folga de faca (Knife Gap) da cizalha rotativa LCT08, utilizando o princípio de **interpolação linear** para correlacionar o valor lido pelo sensor potenciométrico (X) com a folga física real (Y) em milímetros (mm).

---

## 2. Princípio da Interpolação Linear

A calibração é baseada na determinação da equação de uma linha reta que passa por dois pontos de calibração conhecidos, obtidos através de medições mecânicas e leituras elétricas.

### 2.1. Fórmula de Calibração

A folga calculada (Y) é determinada pela seguinte equação:

$$
Y = Y_1 + \left(X - X_1  \right)*(\frac{Y_2 - Y_1}{X_2 - X_1})
$$

Onde:

- **Y:** Folga da faca calculada (mm)  
- **X:** Valor atual do sensor lido pelo CLP (%)  
- **(X₁  ,  Y₁):** Ponto de calibração 1 (Sensor %, Folga mm)  
- **(X₂  ,  Y₂):** Ponto de calibração 2 (Sensor %, Folga mm)

### 2.2. TAGs de Calibração no Studio 5000

As TAGs a seguir devem ser preenchidas pelo técnico e são utilizadas pelo programa do CLP para realizar o cálculo:

| TAG | Tipo de Dado | Descrição | Unidade | Requisito |
|-----|--------------|-----------|----------|-----------|
| `LCT08.Calib.X1_Sensor` | REAL | Valor do sensor no Ponto 1 | % (0 a 100) | **4 casas decimais** |
| `LCT08.Calib.Y1_Folga` | REAL | Folga da faca medida no Ponto 1 | mm | 0.05 a 0.8 |
| `LCT08.Calib.X2_Sensor` | REAL | Valor do sensor no Ponto 2 | % (0 a 100) | **4 casas decimais** |
| `LCT08.Calib.Y2_Folga` | REAL | Folga da faca medida no Ponto 2 | mm | 0.05 a 0.8 |
| `LCT08.Sensor.ValorAtual` | REAL | Valor lido em tempo real | % (0 a 100) | Leitura do CLP |
| `LCT08.Folga.Calculada` | REAL | Folga calculada | mm | Resultado do cálculo |

---

## 3. Procedimento de Calibração Passo a Passo

O procedimento requer colaboração entre a equipe de **Manutenção Mecânica** (medição da folga) e de **Manutenção Elétrica/Eletrônica** (leitura e inserção dos dados no CLP).

### 3.1. Segurança e Preparação

1 - **Bloqueio e Etiquetagem (LOTO):** desligar e bloquear a energia principal da máquina LCT08.

2 - **Acesso:** garantir acesso seguro à área da cizalha rotativa e ao painel do CLP.  

3 - **Ferramentas necessárias:**  

* Jogo de calibres de folga (pentes);
* Acesso ao Studio 5000 e ao CLP.

---

### 3.2. Obtenção do Ponto de Calibração 1 (X₁ , Y₁)

#### 1. Medição Mecânica (Y₁)

- Ajustar a folga para um valor próximo ao limite inferior do range (ex: **0.10 mm**).  
- Confirmar usando o calibre de folga.  
- **Exemplo:** folga medida = **0.100 mm**.

#### 2. Leitura Elétrica (X₁)

- No Studio 5000, ler a TAG `LCT08.Sensor.ValorAtual`.  
- **Exemplo:** leitura = **92.5000 %**.

#### 3. Inserção dos Valores do Ponto 1

- `LCT08.Calib.Y1_Folga` = **0.100**  
- `LCT08.Calib.X1_Sensor` = **92.5000**

---

### 3.3. Obtenção do Ponto de Calibração 2 (X₂, Y₂)

#### 1. Medição Mecânica (Y₂)

- Ajustar a folga para um valor próximo ao limite superior do range (ex: **0.70 mm**).  
- Confirmar com calibre de folga.  
- **Exemplo:** folga medida = **0.700 mm**.

#### 2. Leitura Elétrica (X₂)

- No Studio 5000, ler `LCT08.Sensor.ValorAtual`.  
- **Exemplo:** leitura = **15.2500 %**.

#### 3. Inserção dos Valores do Ponto 2

- `LCT08.Calib.Y2_Folga` = **0.700**  
- `LCT08.Calib.X2_Sensor` = **15.2500**

---

### 3.4. Exemplo de Cálculo e Verificação

| Ponto | Folga (Y) | Sensor (X) | TAG Folga | TAG Sensor |
|-------|-----------|------------|-----------|------------|
| Ponto 1 | 0.100 mm | 92.5000 % | `Y1_Folga` | `X1_Sensor` |
| Ponto 2 | 0.700 mm | 15.2500 % | `Y2_Folga` | `X2_Sensor` |

#### Cálculo do Coeficiente Angular (m)

$$
m = \frac{0.700 - 0.100}{15.2500 - 92.5000}
= \frac{0.600}{-77.2500}
\approx -0.0077669
$$

#### Exemplo de Verificação

Se o sensor ler **50.0000 %**, então:

$$
Y = 0.100 + (-0.0077669)(50.0000 - 92.5000)
$$

$$
Y = 0.100 + (-0.0077669)(-42.5000)
$$

$$
Y = 0.100 + 0.33009325 \approx 0.430\ \text{mm}
$$

O técnico deve confirmar se o valor apresentado em `LCT08.Folga.Calculada` coincide com o esperado.

---

## 4. Alerta Crítico: Zona Morta da Régua Potenciométrica

**ATENÇÃO:** a régua potenciométrica possui **Zona Morta** (Dead Zone) nas extremidades, onde a leitura pode ser não linear ou saturada (0% ou 100%).

### 4.1. Recomendações

- **Evitar extremos:** não utilizar valores muito próximos de 0% ou 100% nos pontos de calibração.  
- **Faixa segura:** realizar a calibração entre **10% e 90%** do range do sensor.  
- **Sintoma:** se a folga continuar mudando mesmo com leitura fixa em 0% ou 100%, a faca entrou na Zona Morta.  
- **Ação corretiva:** reposicionar mecanicamente a régua para que o range de 0.05 mm a 0.8 mm fique dentro da faixa útil do sensor.

---

_Fim do Procedimento_
