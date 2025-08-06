# Atualizando a Aplicação

Este documento descreve o ciclo de vida completo para desenvolver, testar e implantar uma nova funcionalidade ou correção no projeto, utilizando a estrutura de scripts e ambientes.

## Fase 1: Preparação e Início de uma Nova Tarefa

*(Esta fase permanece conceitualmente a mesma, focada em preparar o repositório local. As seções 1.1 e 1.2 do documento anterior continuam válidas.)*

### 1.1: Sincronizando a Branch `main` Local
... (conteúdo original sem alterações) ...

### 1.2: Criando uma Nova Branch de Trabalho
... (conteúdo original sem alterações) ...

---
## Fase 2: Desenvolvimento e Teste Local

### 2.1: Codificação e Testes na Máquina Local

**Objetivo:** Desenvolver a funcionalidade e validá-la usando o script de automação no seu ambiente Windows.

**Procedimento:**

1.  **Desenvolva o Código:** Faça as alterações necessárias nos arquivos do projeto.

2.  **Inicie o Ambiente Local:** Abra o terminal PowerShell na raiz do projeto e execute o script `up.ps1`, especificando o ambiente `local`.
    ```powershell
    .\scripts\up.ps1 -env local
    ```
    *O script irá automaticamente iniciar a infraestrutura de proxy e, em seguida, construir e iniciar os serviços da aplicação na ordem correta.*

3.  **Valide a Aplicação:** Acesse a aplicação no seu navegador e realize os testes para validar a nova funcionalidade.

### 2.2: Salvando e Enviando o Progresso

*(Esta fase, que inclui `git status`, `git add`, `git commit` e `git push`, permanece exatamente a mesma do documento anterior.)*
... (conteúdo original sem alterações) ...

### 2.3: Finalizando o Ambiente Local

**Objetivo:** Parar todos os contêineres e liberar os recursos da sua máquina.

**Procedimento:**

1.  Execute o script `down.ps1` especificando o ambiente `local`.
    ```powershell
    .\scripts\down.ps1 -env local
    ```
2.  **(Opcional) Execute a limpeza geral do Docker** para remover redes, volumes e imagens não utilizadas.
    ```bash
    docker system prune -a -f --volumes
    ```

---
## Fase 3: Validação no Ambiente de Desenvolvimento (DEV)

### 3.1: Acessando e Preparando a VPS de DEV

**Objetivo:** Conectar-se ao servidor de desenvolvimento e baixar a versão mais recente do código.

**Procedimento:**

1.  **Conecte-se à sua VPS de DEV** usando SSH.
    ```bash
    ssh <seu-usuario>@<ip-ou-dominio-da-vps-dev>
    ```
2.  **Navegue até a pasta do projeto.**
    ```bash
    cd ~/EasyTek-Data
    ```
3.  **Limpe o repositório e baixe sua branch de trabalho.** (Os comandos de `git reset`, `git clean`, `checkout` e `pull` da documentação anterior continuam válidos aqui).

### 3.2: Configuração Inicial do Ambiente (Apenas na Primeira Vez)

**Objetivo:** Se esta for a primeira vez que você implanta o projeto neste servidor, alguns passos de configuração única são necessários.

**Procedimento:**

1.  **Dar Permissão de Execução aos Scripts:** O Git não armazena permissões de execução. Conceda-as manualmente.
    ```bash
    chmod +x ./scripts/*.sh
    ```
2.  **Criar os Arquivos de Ambiente:** Copie os arquivos de exemplo para criar as configurações do ambiente de DEV.
    ```bash
    # Copia o arquivo de configuração comum
    cp .env.common.example .env.common

    # Copia o arquivo de configuração específico do ambiente DEV
    cp environments/dev/.env.example environments/dev/.env
    ```
3.  **(Se necessário) Edite os arquivos `.env`** com um editor como `nano` para ajustar qualquer variável específica para o ambiente de DEV.

### 3.3: Implantando e Testando a Nova Versão

**Objetivo:** Executar a nova versão do código no servidor de desenvolvimento usando o script de automação.

**Procedimento:**

1.  **Pare qualquer versão antiga** que possa estar rodando, usando o script `down.sh`.
    ```bash
    ./scripts/down.sh dev
    ```
2.  **Inicie a nova versão.** O script cuidará de todo o processo.
    ```bash
    ./scripts/up.sh dev
    ```
3.  **Monitore e Teste:** Acesse o subdomínio de desenvolvimento (ex: `dev.sua-app.com`) e valide se a funcionalidade está operando como esperado.

---
## Fase 4: Merge e Oficialização do Código

*(Esta fase permanece inalterada. O processo de criar um Pull Request no GitHub, revisar, fazer o merge e deletar a branch de trabalho continua o mesmo.)*
... (conteúdo original sem alterações) ...

---
## Fase 5: Deploy Final em Produção

### 5.1: Acessando e Preparando a VPS de Produção

**Objetivo:** Conectar-se ao servidor final e prepará-lo para receber a versão oficial da aplicação.

**Procedimento:**

1.  **Conecte-se à sua VPS de Produção** via SSH.
2.  **Navegue até a pasta do projeto.**
3.  **Sincronize a branch `main`**, que agora contém suas alterações recém-mescladas.
    ```bash
    git checkout main
    git pull origin main
    ```

### 5.2: Configuração Inicial do Ambiente (Apenas na Primeira Vez)

**Objetivo:** Assim como em DEV, se for a primeira implantação neste servidor, execute a configuração única.

**Procedimento:**

1.  **Dar Permissão de Execução aos Scripts:**
    ```bash
    chmod +x ./scripts/*.sh
    ```
2.  **Criar os Arquivos de Ambiente:**
    ```bash
    cp .env.common.example .env.common
    cp environments/prod/.env.example environments/prod/.env
    ```
3.  **(Se necessário) Edite os arquivos `.env`** com `nano` para garantir que todas as variáveis (como domínios e chaves) estão corretas para o ambiente de produção.

### 5.3: Implantando a Versão Final

**Objetivo:** Colocar a nova versão em produção de forma segura usando os scripts.

**Procedimento:**

1.  **Pare a versão antiga:**
    ```bash
    ./scripts/down.sh prod
    ```
2.  **Execute a limpeza completa do Docker** para garantir uma implantação limpa.
    ```bash
    docker system prune -a -f --volumes
    ```
3.  **Inicie a nova versão:**
    ```bash
    ./scripts/up.sh prod
    ```
4.  **Verificação Final:** Acesse o domínio de produção para confirmar que a nova versão está no ar e funcionando perfeitamente.

