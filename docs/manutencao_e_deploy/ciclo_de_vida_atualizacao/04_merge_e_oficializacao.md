## Fase 4: Merge e Oficialização do Código

Após validar suas alterações no ambiente de desenvolvimento, o próximo passo é integrá-las à branch principal (`main`) através de um Pull Request (PR) no GitHub.

### 4.1: Criando o Pull Request (PR)

O PR é a maneira formal de solicitar a incorporação do seu trabalho. Ele cria um ambiente para revisão e discussão antes do merge final.

**Procedimento:**

1.  **Acesse a Aba "Pull Requests":** No seu repositório no GitHub, clique na aba "Pull requests".

2.  **Inicie um Novo Pull Request:** Clique no botão verde "New pull request".

3.  **Selecione as Branches para o Merge:** Esta é a etapa mais importante. Você verá duas caixas de seleção:
    *   **`base`**: Esta é a branch que receberá as alterações. Selecione `main`.
    *   **`compare`**: Esta é a branch que contém as suas alterações. Selecione a sua branch de trabalho (ex: `fix/loading-ux`).

    > **Atenção:** O GitHub irá comparar as duas branches e mostrar um resumo das alterações. Se estiver tudo certo, você verá uma mensagem "Able to merge".

4.  **Preencha os Detalhes do PR:**
    *   **Título:** Dê um título claro que siga o padrão do seu último commit (ex: `fix(webapp): implement loading overlay`).
    *   **Descrição:** Detalhe o que foi feito, por que foi feito e como foi testado. Mencione que a validação foi concluída no ambiente de DEV.

5.  **Crie o Pull Request:** Clique em "Create pull request".

### 4.2: Revisando e Executando o Merge

Com o PR criado, você ou sua equipe devem revisar o código antes de oficializá-lo.

**Procedimento:**

1.  **Revise os Arquivos:** Na página do PR, clique na aba "Files changed" para ver todas as linhas de código que foram adicionadas ou removidas.

2.  **Execute o Merge:** Se a revisão estiver aprovada, volte para a aba "Conversation".
    *   Clique no botão verde **"Merge pull request"**.
    *   Clique em **"Confirm merge"**.

**Resultado:** O código da sua branch foi incorporado com sucesso à `main`.

### 4.3: Limpando o Repositório

Após o merge, a branch de trabalho já cumpriu seu propósito e deve ser removida para manter o repositório organizado.

**Procedimento:**

**1 - Delete a Branch Remota (no GitHub):**

*   Na página do PR recém-mesclado, o GitHub exibirá um botão **"Delete branch"**. Clique nele para remover a branch do servidor.

**2 - Delete a Branch Local (no seu PC):**

*   No seu terminal, volte para a branch `main` e sincronize-a para baixar o merge que você acabou de fazer.
```bash
git checkout main
git pull origin main
```

*   Agora, delete a cópia local da sua branch de trabalho.
```bash
# Use -d (minúsculo) para uma exclusão segura
git branch -d <nome-da-sua-branch>
```
*Exemplo:* `git branch -d fix/loading-ux`

**Resultado Esperado:** Sua branch de trabalho foi removida do GitHub e da sua máquina. Seu repositório está limpo e pronto para a próxima tarefa.

