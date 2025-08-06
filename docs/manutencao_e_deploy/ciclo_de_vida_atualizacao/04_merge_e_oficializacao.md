## Fase 4: Merge e Oficialização do Código

Após validar suas alterações no ambiente de desenvolvimento, o próximo passo é integrá-las à branch principal (`main`) através de um Pull Request (PR) no GitHub.

### 4.1: Criando o Pull Request (PR)

O PR é a maneira formal de solicitar a incorporação do seu trabalho. Ele cria um ambiente para revisão e discussão antes do merge final.

**Procedimento:**

1.  No seu repositório no GitHub, clique na aba **"Pull requests"**.
2.  Clique no botão verde **"New pull request"**.
3.  Selecione as branches para o merge:
    *   **`base`**: `main`
    *   **`compare`**: `<sua-branch-de-trabalho>`
4.  Preencha o **Título** e a **Descrição** do PR.
    *   **Título:** Dê um título claro que siga o padrão do seu último commit.
    *   **Descrição:** Detalhe o que foi feito, por que foi feito e como foi testado. Mencione que a validação foi concluída no ambiente de DEV.
5.  Clique em **"Create pull request"**.

### 4.2: Revisando e Executando o Merge

Com o PR criado, você ou sua equipe devem revisar o código antes de oficializá-lo.

**Procedimento:**

1.  Na página do PR, revise as alterações na aba "Files changed".
2.  Se aprovado, volte para a aba "Conversation".
3.  Clique em **"Merge pull request"** e confirme.

**Resultado:** O código da sua branch foi incorporado com sucesso à `main`.

### 4.3: Limpando o Repositório

Após o merge, a branch de trabalho já cumpriu seu propósito e deve ser removida para manter o repositório organizado.

**Procedimento:**

1.  **Delete a Branch Remota (GitHub):** Na página do PR mesclado, o GitHub exibirá um botão **"Delete branch"**. Clique nele para remover a branch do servidor.
2.  **Delete a Branch Local (seu PC):**
```bash
git checkout main
git pull origin main
git branch -d <nome-da-sua-branch>
```

**Resultado Esperado:** Sua branch de trabalho foi removida do GitHub e da sua máquina. Seu repositório está limpo e pronto para a próxima tarefa.
