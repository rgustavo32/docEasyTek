# Fluxo de Trabalho Completo de Atualização

Este documento resume o ciclo de vida completo para o desenvolvimento, teste e implantação de uma nova funcionalidade ou correção.

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
    *   `docker-compose up --build -d`

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

*   **2.2. Baixar a Nova Branch:** Trazer o código do GitHub para a VPS.
    *   `git fetch origin`
    *   `git checkout <nome-da-branch>`
    *   `git pull`

*   **2.3. Parar Contêineres Antigos:** Garantir um ambiente de teste limpo.
    *   `docker-compose down`

*   **2.4. Implantar para Teste:** Construir e iniciar a nova versão.
    *   `docker-compose up --build -d`

*   **2.5. Testar no Domínio de DEV:** Validar a funcionalidade no navegador.

---

### **Fase 3: Oficialização e Limpeza do Ambiente de DEV**

Após a validação, o código é formalmente incorporado e o ambiente de teste é desligado.

*   **3.1. Criar Pull Request:** No GitHub, abrir um PR de `<sua-branch>` para a `main`.

*   **3.2. Revisar e Fazer o Merge:** Aprovar e mesclar o Pull Request no GitHub.

*   **3.3. Deletar a Branch:** Remover a branch de trabalho no GitHub e localmente.
    *   No GitHub: Botão "Delete branch".
    *   Localmente: `git checkout main`, `git pull`, `git branch -d <nome-da-branch>`.

*   **3.4. Desligar Serviços na DEV:** Acessar a VPS de DEV e liberar os recursos.
    *   `docker-compose down`

---

### **Fase 4: Deploy Final (VPS de Produção)**

A nova versão é finalmente disponibilizada para os usuários finais.

*   **4.1. Acessar a VPS de PROD:** Conectar-se ao servidor de produção.
    *   `ssh <user>@<vps-prod-ip>`

*   **4.2. Sincronizar a `main`:** Atualizar o código do servidor com a versão recém-mesclada.
    *   `git checkout main`
    *   `git pull origin main`

*   **4.3. Parar a Versão Atual:** Garantir uma transição segura e controlada.
    *   `docker-compose down`

*   **4.4. Implantar a Versão Final:** Construir e iniciar a nova versão.
    *   `docker-compose up --build -d`

*   **4.5. Verificação Final:** Testar rapidamente a aplicação no domínio de produção.
