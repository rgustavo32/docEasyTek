### Resumo: Célula de Carga (PV) para a IHM

1.  O valor "bruto" da célula de carga é lido na entrada analógica (`1004`).
2.  Este valor é movido para um registro auxiliar (`7002`) para ser usado em um cálculo de 32 bits.
3.  Um cálculo complexo de escalonamento e condicionamento é realizado, envolvendo multiplicação (`MULBL`) e divisão (`DVBLL`).
4.  O resultado final deste cálculo é armazenado em um registro (`415C`).
5.  Este valor calculado e condicionado (`415C`) é então movido para o registro do PV do PID (`5030`), que é o valor exibido na IHM.

---

### Ação do Controle PID

1.  O bloco `PID_I` (`Linha 45`) lê o **PV** do registro `5030` e o **Setpoint (SP)** do registro `4100` (que vem da IHM via `5020`).
2.  Ele calcula o **Erro** (SP - PV) e, com base nos seus parâmetros internos (Kp, Ki, Kd), determina a ação de controle necessária.
3.  O resultado desta ação de controle é escrito no registro de **Saída do PID** (`4130`).

---

### Pontos chaves a serem observados:

*   **Observar o registro `1004`:** Monitorar este registro para garantir que o sinal da entrada analógica (PV bruto) está sendo lido corretamente.

*   **Observar o registro `4100` (SP) e `5030` (PV):** Estes são os dois valores mais importantes. Comparar. Eles devem estar na mesma escala. O PID trabalhará para fazer com que o valor de `5030` se iguale ao de `4100`.

*   **Observar o registro `4130`:** Esta é a **saída calculada pelo PID**. Monitorar este valor para ver se o PID está reagindo às mudanças no erro. Se o erro é grande, `4130` deve mudar significativamente. Se o erro é zero, `4130` deve se estabilizar.

### Resumo: Ação de Controle (PID) para a Saída Analógica

1.  **Ação do Controle PID (Reforço):** O bloco `PID_I` (`Linha 45`) compara o Setpoint (`4100`) com o PV (`5030`) e calcula a ação de controle necessária, escrevendo o resultado na **Saída do PID** (`4130`).

2.  **Condicionamento da Saída:** O valor da Saída do PID (`4130`) é convertido (`CONV` na Linha 49) e armazenado em `4140` para ser usado em cálculos.

3.  **Leitura da Velocidade da Máquina:** O valor do taco gerador (`7200`) é lido e convertido (`CONV` na Linha 50), gerando um fator de correção de velocidade em `4142`.

4.  **Cálculo Adaptativo (Feed-Forward):** O sistema multiplica (`MULTB` na Linha 53) a ação de controle do PID (`4140`) pelo fator de velocidade (`4142`). O resultado (`4150`) é então dividido (`DVBLL` na Linha 56) por uma constante, gerando o valor de acionamento final em `415A` e `4158`.

5.  **Escalonamento Final:** O valor de acionamento (`415A` e `415C` após conversão) é escalonado (`SCL` na Linha 59) para a faixa correta da saída analógica, usando os parâmetros da tabela (`4020`). O resultado final é escrito em `4030`.

6.  **Escrita na Saída Analógica:** O valor final e escalonado (`4030`) é movido (`MOV` na Linha 61) para o registro da **saída analógica** (`1006`), controlando diretamente o atuador.

---

### Pontos chaves a serem observados:

*   **Observar o registro `4130`:** Verificar se o PID está gerando uma saída coerente. Se o erro (SP-PV) muda, `4130` deve mudar também.

*   **Observar o registro `7200`:** Confirmar se o sinal de velocidade da encadernadora está sendo lido corretamente.

*   **Observar o registro `4030`:** Este é o valor "pré-saída", o resultado final de todos os cálculos (PID + velocidade + escalonamento). É o valor mais importante para diagnosticar a lógica de controle.

*   **Observar o registro `1006`:** Este é o valor final enviado ao atuador. Ele deve ser idêntico a `4030`, a menos que uma das lógicas de sobrescrita (linhas 62 ou 63) esteja ativa, forçando um valor diferente.
