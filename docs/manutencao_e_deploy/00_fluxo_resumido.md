# Fluxo de Trabalho Completo de Atualização

Este documento resume o ciclo de vida completo para o desenvolvimento, teste e implantação de uma nova funcionalidade ou correção, utilizando os scripts de automação do projeto.

---

### **Fase 1: Preparação e Desenvolvimento (Máquina Local)**

O trabalho começa no seu computador de desenvolvimento.

*   **1.1. Sincronizar com a `main`:** Garante que você está começando a partir da versão mais recente do código.
    *   `git checkout main`
    *   `git pull origin main`

*   **1.2. Criar Nova Branch:** Isola suas alterações em um ambiente de trabalho seguro.
    *   `git checkout -b <tipo>/<descricao>`

*   **1.3. Codificar a Alteração:** Implementar a nova funcionalidade ou correção.

*   **1.4. Testar Localmente:** Validar se a alteração funciona no seu ambiente Docker local.
    *   **Primeira vez?** Garanta que seu arquivo `hosts` está configurado (conforme documentação detalhada).
    *   `./scripts/up.ps1 -env local` (no Windows) ou `./scripts/up.sh local` (no Linux).

*   **1.5. Salvar o Trabalho (Commit):** Criar um ponto de salvamento com uma mensagem descritiva.
    *   `git add .`
    *   `git commit -m "tipo(escopo): mensagem"`

*   **1.6. Enviar para o GitHub (Push):** Publicar sua branch e seus commits.
    *   `git push --set-upstream origin <nome-da-branch>`

---

### **Fase 2: Validação (VPS de Desenvolvimento)**

As alterações são testadas em um ambiente idêntico ao de produção.

*   **2.1. Acessar a VPS de DEV:** Conectar-se ao servidor de testes.
    *   `ssh <user>@<vps-dev-ip>`

*   **2.2. Baixar e Preparar a Nova Branch (Método Robusto):** Traz o código do GitHub para a VPS, limpando e preparando o ambiente.
    *   `git checkout main`
    *   `git pull origin main`
    *   `git fetch origin`
    *   `git checkout <nome-da-branch>`
    *   `git reset --hard origin/<nome-da-branch>`
    *   `sudo git clean -fd`
    *   `chmod +x ./scripts/*.sh` (Crítico para dar permissão de execução)

*   **2.3. Implantar para Teste:** Construir e iniciar a nova versão usando o script.
    *   `./scripts/up.sh dev`

*   **2.4. Testar no Domínio de DEV:** Validar a funcionalidade no navegador.

---

### **Fase 3: Oficialização e Limpeza do Ambiente de DEV**

Após a validação, o código é formalmente incorporado e o ambiente de teste é desligado.

*   **3.1. Criar Pull Request:** No GitHub, abrir um PR de `<sua-branch>` para a `main` (usando a aba "Pull requests").

*   **3.2. Revisar e Fazer o Merge:** Aprovar e mesclar o Pull Request no GitHub.

*   **3.3. Deletar a Branch:** Remover a branch de trabalho no GitHub e localmente.
    *   No GitHub: Botão "Delete branch".
    *   Localmente: `git checkout main`, `git pull`, `git branch -d <nome-da-branch>`.

*   **3.4. Desligar Serviços na DEV:** Acessar a VPS de DEV e liberar os recursos.
    *   `./scripts/down.sh dev`
    *   `docker container prune -f && docker image prune -a -f` (Limpeza opcional)

---

### **Fase 4: Deploy Final (VPS de Produção)**

A nova versão é finalmente disponibilizada para os usuários finais.

*   **4.1. Acessar a VPS de PROD:** Conectar-se ao servidor de produção.
    *   `ssh <user>@<vps-prod-ip>`

*   **4.2. Sincronizar a `main` (Método Robusto):** Atualizar o código do servidor com a versão recém-mesclada.
    *   `git checkout main`
    *   `git pull origin main`
    *   `git reset --hard origin/main`
    *   `sudo git clean -fd`
    *   `chmod +x ./scripts/*.sh`

*   **4.3. Implantar a Versão Final:** Parar a versão antiga, limpar o ambiente e iniciar a nova.
    *   `./scripts/down.sh prod`
    *   `docker image prune -a -f` (Recomendado para limpar imagens antigas)
    *   `./scripts/up.sh prod`

*   **4.4. Verificação Final:** Testar rapidamente a aplicação no domínio de produção.
