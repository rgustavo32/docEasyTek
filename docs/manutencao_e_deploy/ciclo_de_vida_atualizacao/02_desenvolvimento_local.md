

## Fase 2: Desenvolvimento e Teste Local
### 2.1: Codificação e Testes na Máquina Local

**Objetivo:** Desenvolver a funcionalidade ou correção de bug e garantir que ela funcione corretamente em um ambiente controlado antes de compartilhá-la.

**Procedimento:**

**1 - Desenvolva o Código:** Utilize seu editor de código (VS Code, etc.) para fazer todas as alterações necessárias nos arquivos do projeto. Crie novos arquivos, modifique os existentes, e implemente a lógica da sua tarefa.

**2 -Teste a Aplicação Localmente:** É crucial validar se suas alterações não quebraram nada e se a nova funcionalidade se comporta como o esperado.  

*  Abra o terminal na pasta do projeto.  
*  Suba a aplicação usando o Docker Compose. O comando `--build` é importante caso você tenha alterado arquivos que compõem a imagem Docker (como um `Dockerfile` ou o código-fonte da `webapp`).  

```bash
docker compose up -d --build
```
* Acesse a aplicação no seu navegador (geralmente em `http://localhost:<porta>` ) e realize testes manuais para validar o resultado.  

**3 - Repita o Ciclo:** Caso encontre problemas, pare a aplicação (`docker compose down`), corrija o código e repita o passo 2 até que a funcionalidade esteja estável e completa no seu ambiente local.

### 2.2: Salvando o Progresso com Commits

**Objetivo:** Criar "pontos de salvamento" (commits) lógicos e bem descritos do seu trabalho. Isso cria um histórico claro das alterações e facilita a colaboração e a depuração de problemas.

**Procedimento:**

1.  **Verifique o Status das Alterações:** Antes de fazer um commit, é uma boa prática ver quais arquivos foram modificados.
    ```bash
    git status
    ```
    *(Este comando listará os arquivos modificados em vermelho).*

2.  **Adicione os Arquivos ao "Palco":** Informe ao Git quais arquivos você deseja incluir neste ponto de salvamento. Para adicionar todos os arquivos modificados, use o ponto.
    ```bash
    git add .
    ```

3.  **Crie o Commit:** Salve as alterações com uma mensagem que siga o padrão de "Commits Convencionais".
    ```bash
    git commit -m "<tipo>(<escopo>): <mensagem>"
    ```
    *   **`<tipo>`:** `feat`, `fix`, `docs`, `infra`, etc.
    *   **`<escopo>`:** A parte do projeto que foi alterada (ex: `webapp`, `node-red`, `database`).
    *   **`<mensagem>`:** Uma descrição curta e imperativa do que foi feito.

    **Exemplos Práticos:**

    *   `git commit -m "feat(webapp): Add Google authentication button"`
    *   `git commit -m "fix(node-red): Correct payload parsing on MQTT input"`

**Boas Práticas:**
*   Faça commits pequenos e focados. Em vez de um commit gigante no final, salve cada pequena parte funcional da sua tarefa.
*   Escreva mensagens claras. Elas devem explicar *o que* a mudança faz e, se necessário, *por que* ela foi feita.

### 2.3: Enviando a Nova Branch para o GitHub

**Objetivo:** Publicar a sua branch de trabalho e seus commits no repositório remoto (GitHub). Isso cria um backup do seu trabalho na nuvem e o prepara para ser compartilhado e testado em outros ambientes.

**Procedimento:**

1.  **Execute o Comando `push`:** Na primeira vez que você envia uma nova branch, o Git pode pedir para você especificar o destino. O comando a seguir envia a branch atual para o repositório remoto (chamado `origin` por padrão) e a configura para rastrear a versão remota.
```bash
git push --set-upstream origin <nome-da-sua-branch>
```
*Substitua `<nome-da-sua-branch>` pelo nome exato da sua branch de trabalho (ex: `feat/login-com-google`).*

2.  **Para Envios Futuros:** Depois do primeiro `push`, para enviar commits subsequentes na mesma branch, o comando é mais simples:
```bash
git push
```

**Resultado Esperado:** O terminal mostrará o progresso do upload e confirmará que a branch foi enviada com sucesso. Ao acessar a página do seu repositório no GitHub, você verá a nova branch listada e poderá visualizar todos os commits que você enviou.


---
