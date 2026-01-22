# 📄 Relatório de Análise de Causa Raiz (RCA)  
## Trepidação dos Servomotores do Empilhador – Linha LCT 2,5

**Equipamento:** Empilhador – 4 Esteiras Imantadas (A, B, C, D)  
**Acionamento:** 2 Servomotores (Mestre por posição / Escravo por torque)  
**Drive:** SEW – sem falhas registradas  
**Fabricante da Máquina:** Biele  
**Período de Análise:** 01/10/2025 a 16/01/2026  
**Responsável pela Análise:** Rodolfo Silva – Automação / Manutenção

---

## 1. Descrição do Problema

O problema consiste em um **descontrole dos dois servomotores (mestre e escravo)** responsáveis pelo movimento do eixo que atua sobre as **quatro esteiras de empilhamento de chapas**.

O comportamento observado é uma **trepidação severa**, caracterizada por rápidas inversões de sentido (avanço e reverso várias vezes por segundo), gerando:

- Risco de dano mecânico
- Instabilidade de posicionamento
- Queda de qualidade no empilhamento
- Paradas de produção

### Características principais:

- ❌ Drive **não apresenta falhas elétricas**
- ✅ Ocorre durante correções de posicionamento
- ⚠️ Sistema entra em **ressonância mecânico-dinâmica**
- ❗ Não atinge posição final programada

---

## 2. Hipótese Inicial

Suspeita inicial de:

- Falha de drive
- Problemas elétricos
- Problemas de debounce ou lógica de transição Manual/Automático
- Parâmetros incorretos de controle

Essas hipóteses foram investigadas ao longo da análise.

---

## 3. Linha do Tempo da Investigação

### 📅 01/10/2025 – Análise com representante SEW

- Troca do drive do eixo escravo → problema permaneceu
- Troca do drive do eixo mestre → problema permaneceu
- Conclusão: **não se trata de defeito físico de drive**

Relato dos operadores:
- ~90% das ocorrências no momento de rearme (Manual → Automático)

Ação:
- Implementada lógica de tempo mínimo de transição para evitar comutação rápida
- Resultado: problema **continuou ocorrendo**

---

### 📅 16/10/2025 – Avaliação de parâmetros via receita

- Aceleração, velocidade e posicionamento vêm da receita
- Nenhuma trepidação observada nesse acompanhamento
- Não foi possível correlacionar diretamente com parâmetros nesse momento

---

### 📅 22/10/2025 – Análise por SKU / Configuração de Empilhamento

Observações:

- Materiais com mesmas dimensões apresentam:
  - Quantidades diferentes de chapas por passe
  - Padrões distintos de empilhamento

Diferença de passe:

- Mesa 1 a ~3 m da origem
- Mesa 2 a ~6 m da origem
- Algumas receitas usam:
  - Passes longos
  - Outras usam vários passes curtos

Hipótese levantada:
- Possível sobrecarga térmica ou mecânica dependendo da receita

Ação:
- Criado apontamento de operador para registrar receita e carga na falha

Resultado:
- **Nenhuma correlação direta entre receita e falha no rearme**

---

### 📅 02/12/2025 – Testes controlados de rearme

Cenário:
- Trepidação ocorre quando eixo é rearmado em automático

Testes:

1. Manual ↔ Automático (40 vezes)  
   → Nenhuma trepidação

2. Manual + JOG + Automático (5 vezes)  
   → 4 falhas

Conclusões:

- Se **não há correção de posição**, não ocorre trepidação
- Se há **correção pequena de posição**, ocorre ressonância

Início de análise das FCs de controle:
- FC54
- FC116

---

### 📅 03/12/2025 – Análise de Controle

Conclusões técnicas:

- O software Siemens **não gerencia janelas de tolerância**
- O drive tenta corrigir **cada pulso de erro**

Observado:

| Erro de posição | Comportamento |
|--------|----------------|
| ~0–10 pulsos | Corrige normalmente |
| >500 pulsos | Corrige normalmente |
| 50–200 pulsos | Entra em ressonância |

Motivo:
- Correção mínima gera **overcorrection**
- Oscilação crescente (ressonância)

Configuração do sistema:

- Servo Mestre → Controle por posição
- Servo Escravo → Controle por torque

Decisão:
- Desenvolver lógica para gerenciamento do erro no PLC

---

### 📅 04/12/2025 – Implementação de Solução para Rearme

Criada lógica de proteção no rearme automático:

Fluxo:

1. Operador muda para Manual
2. Servos desmagnetizados
3. Operações manuais
4. Retorno para Automático
5. Movimento de referência
6. Forçar erro grande artificial (Destino = Atual + 5000 pulsos)
7. Depois permitir posicionamento normal

Resultado:

- Trepidação no rearme **eliminada em 100%**
- Falhas restantes ocorrem **somente durante processo automático**

---

### 📅 10/12/2025 – Atendimento Corretivo em Produção

Condições:

- Chapa: ~1000 mm largura
- Velocidade empilhador: **150 m/min**

Observações:

- Correia visivelmente frouxa
- Confirmada hipótese de afrouxamento por excesso de velocidade

Outros achados:

- Ressonância inicia quando empurradores atuam
- Pequeno movimento desloca eixo da posição alvo
- Servo tenta corrigir erro pequeno → entra em ressonância

Teste:

- Velocidade reduzida para ~75 m/min
- Operação estável

Conclusão:
- **Velocidade excessiva não é necessária para produtividade**

---

### 📅 15/01/2026 – Análise de Receitas x Velocidade

Constatação via documentação do fabricante Biele:

- Velocidade máxima recomendada: **90 m/min**
- Muitas receitas configuradas em:
  - 120 m/min
  - 150 m/min

Observação importante:

- Quanto menor o deslocamento, menor necessidade de velocidade alta
- Receitas não consideram distância real de passe

---

### 📅 16/01/2026 – Ensaio Mecânico-Dinâmico

Resultados:

- Acima de 60 m/min:
  - Ao acionar empurradores ocorre pequeno movimento da esteira
  - Indício de correia tensionada de um lado e frouxa do outro

Efeitos:

1. **Na dinâmica do servo:**
   - Pequeno tranco → erro pequeno
   - Servo tenta corrigir → entra em ressonância

2. **Na qualidade do empilhamento:**
   - Chapas variam posição
   - Acúmulo de erro entre ciclos

Conclusão:
- Movimento induzido por **alívio de tensão da correia** no momento da desconexão

---

## 4. Causa Raiz

A falha de trepidação é causada por **interação entre três fatores principais**:

### 🔧 Causa Mecânica

- Afrouxamento de correias por operação acima da velocidade nominal
- Redistribuição de tensão no momento da atuação dos empurradores
- Pequenos deslocamentos não previstos no eixo

### ⚙️ Causa Dinâmica de Controle

- Correções de posição muito pequenas
- Drive tenta corrigir cada pulso
- Overcorrection gera ressonância

### ⚠️ Causa de Processo

- Receitas com velocidades acima do necessário
- Falta de relação direta entre distância de passe e velocidade configurada

---

## 5. Solução Implementada

### ✔ Solução para Rearme Automático

- Implementada lógica de gerenciamento de erro
- Força condição de correção longa antes do posicionamento fino
- Eliminou 100% das falhas de rearme

---

## 6. Situação Atual

- Falhas remanescentes ocorrem **somente durante processo automático**
- Ocorrem apenas em determinadas receitas
- Ajustes de tensão das esteiras reduziram incidência significamente
- Qualidade de empilhamento melhora com redução de velocidade

---

## 7. Recomendações Técnicas

### 🔩 Mecânicas

- Revisar sistema de tensionamento de correias
- Avaliar desgaste prematuro
- Criar procedimento de inspeção periódica

### ⚙️ Processo

- Criar limites máximos de velocidade por tipo de passe
- Relacionar velocidade com distância real de deslocamento
- Revisar todas as receitas atuais

### 🧠 Controle

- Avaliar implementação de:
  - Janelas de tolerância de posição
  - Perfis de desaceleração diferenciados
- Avaliar ajuste de torque do servo escravo

### 🤝 Suporte Fabricantes

- Agendar análise conjunta com:
  - SEW (drive e servo)
  - Time Manutenção (Mecânica)


Objetivo:
- Validar limites dinâmicos do conjunto mecânico
- Confirmar estratégia de controle mais adequada

---

## 8. Conclusão Final

A trepidação não é causada por falha elétrica ou defeito de drive, e sim por **condição de processo e dinâmica mecânico-eletrônica** operando fora do envelope ideal do sistema.

O problema deixou de ser crônico após correção de lógica de rearme, restando apenas ajustes de:

- Receita
- Velocidade
- Tensão mecânica

Portanto, trata-se atualmente de um **problema de ajuste de processo e dimensionamento operacional**, não de defeito de hardware.

---
