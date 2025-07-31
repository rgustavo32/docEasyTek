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

