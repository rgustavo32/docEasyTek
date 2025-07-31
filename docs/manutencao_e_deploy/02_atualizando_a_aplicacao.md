# Atualizando a Aplicação

Este documento descreve o ciclo de vida completo para adicionar uma nova funcionalidade ou correção ao projeto.

---


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

## Fase 3: Validação no Ambiente de Desenvolvimento

### 3.1: Acessando a VPS de Desenvolvimento

**Objetivo:** Preparar o ambiente de testes real, que é um clone do ambiente de produção. É aqui que validamos se o código funciona em um servidor Linux real, e não apenas na máquina local.

**Procedimento:**

**1 - Conecte-se à sua VPS de Desenvolvimento** usando o protocolo SSH (Secure Shell).
    *   Abra seu terminal (Git Bash, PowerShell, Prompt de Comando do Windows, etc.).
    *   Utilize o comando `ssh` com seu nome de usuário e o endereço IP ou domínio da sua VPS.
```bash
ssh <seu-usuario>@<ip-ou-dominio-da-vps-dev>
```
*Exemplo* `ssh admin@198.51.100.10` ou `ssh admin@dev.meuprojeto.com`

**2 - Navegue até a Pasta do Projeto:** Uma vez conectado, vá para o diretório onde o código da aplicação está localizado.
```bash
cd ~/EasyTek-Data
```

**Resultado Esperado:** Você estará logado no terminal da sua VPS de Desenvolvimento e dentro da pasta correta, pronto para baixar e implantar a nova versão da aplicação para testes.

### 3.2: Trazendo a Nova Branch para a VPS

**Objetivo:** Baixar a branch com suas novas alterações, que até agora só existia no seu PC e no GitHub, para o servidor de desenvolvimento.

**Procedimento:**

**1 - Busque Todas as Atualizações do GitHub:** Primeiro, atualize a lista de todas as branches e commits que existem no repositório remoto. Este comando não altera nenhum arquivo ainda, apenas atualiza o "mapa" do Git local.
```bash
git fetch origin
```

**2 - Mude para a Sua Branch de Trabalho:** Agora que a VPS conhece a sua nova branch, mude para ela.
```bash
git checkout <nome-da-sua-branch>
```
*Exemplo:* `git checkout feat/login-com-google`

**3 - Garanta que a Branch Local está Sincronizada:** Embora o `checkout` geralmente já traga a versão mais recente, é uma boa prática executar um `pull` para garantir que você tem exatamente a última versão que foi enviada ao GitHub.
```bash
git pull
```

**Resultado Esperado:** O terminal confirmará que você mudou para a sua branch de trabalho e que ela está atualizada. A pasta `~/EasyTek-Data` na VPS de Desenvolvimento agora contém exatamente o mesmo código que você finalizou no seu PC.

### 3.3: Implantando e Testando a Nova Versão

**Objetivo:** Executar a nova versão do código no servidor de desenvolvimento e realizar os testes finais para garantir que tudo funciona perfeitamente no ambiente real.

**Procedimento:**

**1 - Verifique as Configurações do Ambiente:** Lembre-se de que a VPS de Desenvolvimento pode ter configurações específicas. Garanta que os arquivos `.env` e `docker-compose.yml` (principalmente a seção `networks`) estão corretos para este ambiente.

**2 - Construa as Novas Imagens e Suba os Contêineres:** Use o Docker Compose para implantar a aplicação. O argumento `--build` é essencial para garantir que o Docker use seu novo código para criar as imagens, em vez de usar versões antigas em cache.
```bash
docker compose up -d --build
```

**3 - Monitore a Inicialização (Opcional, mas recomendado):** Verifique se todos os contêineres subiram sem erros.

*   Liste os contêineres em execução:
```bash
docker ps
```
*   Se algum contêiner estiver reiniciando ou ausente, verifique seus logs para encontrar a causa do problema:
```bash
docker logs <nome-do-container>
```

**4 - Teste a Aplicação no Domínio de Desenvolvimento:**

*   Abra seu navegador e acesse o subdomínio correspondente à sua funcionalidade no ambiente de desenvolvimento (ex: `https://app.dev.meuprojeto.com` ).
*   Realize todos os testes necessários para validar a nova funcionalidade e também para garantir que nenhuma funcionalidade antiga foi quebrada (testes de regressão).

**Resultado Esperado:** A nova versão da sua aplicação está no ar e funcionando corretamente no ambiente de desenvolvimento. Se você encontrar algum problema, volte para a Fase 2 (Desenvolvimento Local), corrija, envie as alterações para o GitHub e repita os passos da Fase 3.

---


## Fase 4: Merge e Oficialização do Código

### 4.1: Criando o Pull Request no GitHub

**Objetivo:** Propor formalmente que as alterações da sua branch de trabalho sejam incorporadas (mescladas) na branch principal (`main`). O Pull Request (PR) é um mecanismo de revisão que permite a você e a outros visualizarem as mudanças antes que elas se tornem parte oficial do projeto.

**Procedimento:**

1.  **Acesse o Repositório no GitHub:** Abra a página do seu projeto no navegador (ex: `https://github.com/seu-usuario/seu-repositorio` ).

2.  **Inicie o Pull Request:**
    *   O GitHub geralmente detecta que você enviou uma nova branch recentemente e exibe um banner amarelo com o nome da sua branch e um botão verde **"Compare & pull request"**. Clique neste botão.
    *   Se o banner não aparecer, vá para a aba **"Pull requests"** e clique no botão verde **"New pull request"**.

3.  **Configure as Branches para o Merge:** Na tela "Open a pull request", certifique-se de que as branches estão configuradas corretamente:
    *   A branch `base` deve ser a `main` (ou `master`).
    *   A branch `compare` deve ser a sua branch de trabalho (ex: `feat/login-com-google`).

4.  **Preencha os Detalhes do Pull Request:**
    *   **Título:** Dê um título claro e conciso que resuma a mudança. Ele geralmente é preenchido automaticamente com a mensagem do seu último commit.
    *   **Descrição:** Escreva um resumo do que foi feito, por que foi feito e como foi testado. Você pode referenciar os testes realizados na Fase 3.

5.  **Crie o Pull Request:** Role a página e clique no botão verde **"Create pull request"**.

**Resultado Esperado:** Uma nova página de Pull Request será criada. Nela, você pode ver um histórico de todos os commits, os arquivos que foram alterados e iniciar discussões. O código ainda **não** foi mesclado na `main`; você apenas criou a solicitação formal para isso.

### 4.2: Revisando e Executando o Merge

**Objetivo:** Realizar uma verificação final das alterações e, se tudo estiver correto, executar a fusão (merge) do código na branch `main`, tornando as mudanças uma parte permanente e oficial do projeto.

**Procedimento:**

1.  **Revise as Alterações:** Na página do Pull Request, navegue pela aba **"Files changed"**. O GitHub mostrará uma visão "diff", destacando em verde as linhas adicionadas e em vermelho as removidas. Esta é sua última oportunidade para pegar qualquer erro ou código indesejado.

2.  **Verifique os Status de CI/CD (se aplicável):** Se o projeto tiver automações (como testes automatizados), aguarde a conclusão delas. Um "check" verde indica que tudo passou.

3.  **Execute o Merge:** Uma vez que a revisão esteja completa e você esteja confiante nas alterações, volte para a aba **"Conversation"**.
    *   Encontre o grande botão verde **"Merge pull request"**.
    *   Clique nele. Uma caixa de confirmação aparecerá.
    *   Clique no botão **"Confirm merge"**.

**Resultado Esperado:** O GitHub executará a fusão. A etiqueta do Pull Request mudará de "Open" (verde) para "Merged" (roxa). Neste momento, todo o código da sua branch de trabalho foi copiado com sucesso para a branch `main`, e a `main` agora contém suas novas funcionalidades ou correções.

### 4.3: Limpando a Branch de Trabalho

**Objetivo:** Manter o repositório organizado, removendo a branch de trabalho que já foi mesclada. Como todo o seu conteúdo agora faz parte da `main`, a branch original não é mais necessária e pode ser deletada com segurança.

**Procedimento:**

**1 - Delete a Branch Remota (no GitHub):**

*   Imediatamente após o merge, na mesma página do Pull Request, o GitHub exibirá uma mensagem informando que o merge foi bem-sucedido e um botão cinza **"Delete branch"** aparecerá.
*   Clique neste botão para remover a branch do repositório no GitHub.

**2 - Delete a Branch Local (no seu PC Windows):**

*   Abra o terminal na pasta do seu projeto.
*   Primeiro, mude de volta para a branch `main`.
```bash
git checkout main
```
*   Sincronize a `main` local para garantir que ela tenha o merge que acabamos de fazer.
```bash
git pull
```
*   Agora, delete a cópia local da sua branch de trabalho. O `-d` (minúsculo) é uma opção segura que só deleta a branch se ela já tiver sido mesclada.
```bash
git branch -d <nome-da-sua-branch>
```
*Exemplo:* `git branch -d feat/login-com-google`

**Resultado Esperado:** Sua branch de trabalho foi removida tanto do GitHub quanto da sua máquina local. Seu repositório fica limpo e pronto para o início de uma nova tarefa, com a branch `main` contendo todas as últimas atualizações.

---


## Fase 5: Deploy Final em Produção

### 5.1: Acessando a VPS de Produção

**Objetivo:** Conectar-se ao servidor de produção, o ambiente real onde os usuários finais acessam a aplicação. Este é o último passo, onde a nova versão será disponibilizada para todos.

**Procedimento:**

**1 - Conecte-se à sua VPS de Produção** usando o protocolo SSH. O processo é idêntico ao da VPS de Desenvolvimento, mas com o endereço do servidor de produção.

*   Abra seu terminal (Git Bash, PowerShell, etc.).
*   Utilize o comando `ssh` com seu nome de usuário e o endereço IP ou domínio da sua VPS de Produção.

```bash
ssh <seu-usuario>@<ip-ou-dominio-da-vps-prod>
```
*Exemplo Fictício:* `ssh admin@203.0.113.25` ou `ssh admin@app.meuprojeto.com`

**2 - Navegue até a Pasta do Projeto:** Uma vez conectado, vá para o diretório onde o código da aplicação está localizado.
```bash
cd ~/EasyTek-Data
```

**Resultado Esperado:** Você estará logado no terminal da sua VPS de Produção e dentro da pasta correta, pronto para baixar e implantar a versão final e testada da aplicação.


### 5.2: Sincronizando a Branch `main` na Produção

**Objetivo:** Atualizar o código no servidor de produção para que ele corresponda exatamente à versão mais recente da branch `main` no GitHub, que agora inclui as alterações recém-mescladas.

**Procedimento:**

1.  **Mude para a Branch `main`:** É crucial garantir que você está na branch principal antes de puxar as atualizações. O servidor de produção deve sempre rodar o código da `main`.
```bash
git checkout main
```

2.  **Puxe as Atualizações da `main`:** Baixe a versão mais recente do código do GitHub. Como o merge foi feito na Fase 4, este comando trará todas as novas funcionalidades e correções para o servidor.
```bash
git pull origin main
```

**Resultado Esperado:** O terminal mostrará os arquivos que foram alterados, confirmando que o código da nova versão foi baixado com sucesso para a VPS de Produção. A pasta `~/EasyTek-Data` agora contém o código pronto para ser implantado.

### 5.3: Implantando a Versão Final

**Objetivo:** Parar a versão antiga da aplicação e iniciar a nova versão que acabamos de baixar, disponibilizando as atualizações para todos os usuários finais.

**Procedimento:**

1.  **Verifique as Configurações do Ambiente:** Garanta que os arquivos `.env` e `docker-compose.yml` (principalmente a seção `networks`) estão corretos para o ambiente de Produção. Normalmente, nenhuma alteração é necessária nesta etapa, pois a `main` já deve conter a configuração correta para produção.

2.  **Construa as Novas Imagens e Suba os Contêineres:** Use o Docker Compose para implantar a aplicação. O argumento `--build` é essencial para que o Docker utilize o novo código para recriar as imagens.
```bash
docker compose up -d --build
```
*O Docker é inteligente e só reconstruirá as imagens dos serviços cujo código-fonte foi alterado (ex: `webapp`), tornando o processo eficiente.*

3.  **Verificação Final:** Após a implantação, faça uma verificação rápida para garantir que a aplicação está funcionando como esperado no ambiente de produção.
    *   Acesse o domínio principal da sua aplicação (ex: `https://app.meuprojeto.com` ) no navegador.
    *   Teste a nova funcionalidade e verifique se as principais partes do site continuam operacionais.

**Resultado Esperado:** A nova versão da aplicação está no ar, em produção, e acessível a todos os usuários. O ciclo de desenvolvimento e deploy está completo.
