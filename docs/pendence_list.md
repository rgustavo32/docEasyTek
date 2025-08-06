# Pendências Técnicas e de Processo - EasyTek-Data

Esta é uma lista consolidada de todas as tarefas e melhorias técnicas que foram discutidas e estão pendentes de implementação ou validação final.

| Tarefa | Estimativa (Horas) | Pendências Detalhadas (Sub-tarefas) |
| :--- | :--- | :--- |
| **1. Validação do Fluxo de Deploy** | **CONCLUÍDO** | <ul><li><span style="text-decoration:line-through">1.1: Teste de ponta a ponta.</span></li><li><span style="text-decoration:line-through">1.2: Atualizar documentação com scripts.</span></li><li><span style="text-decoration:line-through">1.3: Adicionar seção de setup inicial de servidor.</span></li></ul> |
| **2. Estratégia de Imagens e Registro** | 10 horas | <ul><li>2.1: Configurar GHCR.</li><li>2.2: Gerar PAT.</li><li>2.3: Nomear imagens no compose.</li><li>2.4: Ajustar scripts para push/pull.</li><li>2.5: Remover `build:` dos compose de deploy.</li></ul> |
| **3. Integração com API do WhatsApp** | 21 horas | <ul><li>3.1: Setup da plataforma Meta.</li><li>3.2: Criação de templates.</li><li>3.3: Módulo de notificação no `event-gateway`.</li><li>3.4: Lógica de agendamento no `Node-RED`.</li></ul> |
| **4. Otimização de DEV (Bind Mounts)** | 8 horas | <ul><li>4.1: Modificar `override` de dev.</li><li>4.2: Ajustar `Dockerfile` para hot-reloading.</li><li>4.3: Documentar fluxo alternativo.</li><li>4.4: Criar checklist de PR.</li></ul> |
| **5. Robustez do Node-RED** | 1 - 2 horas | <ul><li>5.1: Adicionar nó de `delay` ou `trigger` para resolver condição de corrida na inicialização dos fluxos.</li></ul> |
| **6. Automação de Inicialização (Serviço)** | 6 - 10 horas | <ul><li>6.1: Desinstalar Docker Desktop.</li><li>6.2: Habilitar `systemd` no WSL.</li><li>6.3: Instalar Docker Engine nativo no WSL.</li><li>6.4: Criar serviço `systemd` para a aplicação.</li><li>6.5: Configurar Agendador de Tarefas do Windows para iniciar o WSL no boot.</li></ul> |

### 1. Finalização do Fluxo de Deploy Padrão
*   **Descrição:** O fluxo de trabalho principal, usando scripts para deploy em `dev` e `prod`, foi implementado, mas ainda não foi formalmente validado de ponta a ponta nem documentado em sua versão final.
*   **Status:** **CONCLUÍDO.** O ciclo completo foi executado e a documentação principal foi atualizada com todos os aprendizados, incluindo o uso dos scripts, a configuração do arquivo `hosts` local e os ajustes de permissão e autenticação nos servidores.
*   **Tarefas:**
    *   **1.1:** <span style="text-decoration:line-through">Executar um ciclo completo (local -> dev -> prod) usando os scripts `up.sh`/`down.sh` para validar a robustez do processo.</span>
    *   **1.2:** <span style="text-decoration:line-through">Atualizar a documentação principal (`Atualizando a Aplicação.md`) para refletir o uso dos scripts, simplificando os passos para o desenvolvedor.</span>
    *   **1.3:** <span style="text-decoration:line-through">Adicionar uma seção na documentação sobre a "Configuração Inicial de um Novo Servidor", mencionando a necessidade de executar `chmod +x` nos scripts e copiar os arquivos `.env.example`.</span>

### 2. Otimização do Processo de Build (Estratégia de Imagens)
*   **Descrição:** O processo atual exige que o código-fonte esteja no servidor de produção para construir as imagens. O objetivo é mover o `build` para o ambiente local (ou CI/CD) e fazer o deploy de imagens prontas, aumentando a segurança e a velocidade.
*   **Tarefas:**
    *   **2.1:** Configurar um registro de contêineres (GitHub Container Registry é o recomendado).
    *   **2.2:** Gerar um Personal Access Token (PAT) no GitHub com escopo `write:packages` para autenticação.
    *   **2.3:** Modificar o `docker-compose.yml` para adicionar a diretiva `image: ghcr.io/seu-usuario/seu-repo:tag` a cada serviço customizado (`webapp`, `event-gateway`, `node-red`).
    *   **2.4:** Ajustar os scripts `up.sh`/`up.ps1` para usar `docker compose push` (no ambiente de build) e `docker compose pull` (nos ambientes de deploy).
    *   **2.5:** Remover a diretiva `build:` dos arquivos `docker-compose.yml` que serão usados nos servidores, deixando apenas a `image:`.

### 3. Implementação de Notificações via WhatsApp
*   **Descrição:** Adicionar a funcionalidade de enviar notificações de falhas de equipamento para clientes através da API Oficial do WhatsApp.
*   **Tarefas:**
    *   **3.1 (Configuração):** Criar a conta no Gerenciador de Negócios da Meta, verificar a empresa, configurar o App, vincular o número de telefone e obter as credenciais da API.
    *   **3.2 (Templates):** Criar e submeter os templates de mensagem (ex: "Alerta de Falha", "Resumo Diário") para aprovação da Meta.
    *   **3.3 (Backend):** Desenvolver um módulo no `event-gateway` (Python) para encapsular a lógica de chamada à API do WhatsApp.
    *   **3.4 (Lógica de Negócio):** Implementar no `Node-RED` a lógica de agendamento (disparar nos horários 08h, 14h, 18h) e o agrupamento de falhas para enviar um resumo consolidado.

### 4. Otimização do Desenvolvimento em DEV (Bind Mounts)
*   **Descrição:** Criar um fluxo de trabalho alternativo para o ambiente `dev` que permita o desenvolvimento "real-time" sem a necessidade de reconstruir a imagem a cada alteração de código.
*   **Tarefas:**
    *   **4.1:** Modificar o `environments/dev/docker-compose.override.yml` para adicionar um *bind mount* do código-fonte do host para dentro do contêiner (ex: `- ./webapp:/app`).
    *   **4.2:** Ajustar o `Dockerfile` do `webapp` para instalar dependências de desenvolvimento (como `debugpy` ou `watchdog`) que permitam o "hot-reloading".
    *   **4.3:** Criar uma nova documentação ou uma seção separada explicando este fluxo de desenvolvimento avançado e suas implicações.
    *   **4.4:** Adicionar uma checklist de "Pré-Merge" ao template de Pull Request no GitHub, forçando a verificação de que todas as alterações (novos arquivos, novas dependências) foram devidamente adicionadas ao `Dockerfile` principal.

### 5. Melhorias de Robustez (Node-RED)
*   **Descrição:** O Node-RED apresenta erros de "variável não encontrada" na inicialização, que se autocorrigem. Isso indica uma condição de corrida.
*   **Tarefas:**
    *   **5.1:** Implementar um nó de `delay` ou `trigger` no início dos fluxos do Node-RED para garantir que os fluxos de inicialização (que definem variáveis globais) terminem antes que os fluxos de trabalho principais comecem.

### 6. Automação de Inicialização como Serviço de Sistema
*   **Descrição:** O requisito é que a aplicação inicie automaticamente em uma máquina Windows após uma queda de energia, sem a necessidade de login de usuário, para cumprir políticas de TI restritivas. A solução padrão (Docker Desktop + NSSM) não atende a este requisito, pois depende de uma sessão de usuário ativa. A solução robusta é migrar do Docker Desktop para uma instalação nativa do Docker Engine dentro do WSL 2.
*   **Tarefas:**
    *   **6.1:** Desinstalar completamente o Docker Desktop da máquina de produção para evitar conflitos.
    *   **6.2:** Habilitar o `systemd` na distribuição WSL (Ubuntu) editando o arquivo `/etc/wsl.conf`.
    *   **6.3:** Instalar o Docker Engine e o Docker Compose diretamente no ambiente WSL usando `apt-get`.
    *   **6.4:** Criar um arquivo de serviço `systemd` (ex: `/etc/systemd/system/easytek-data.service`) que define a dependência do serviço Docker e executa o script `up.sh` na inicialização.
    *   **6.5:** Configurar uma tarefa no Agendador de Tarefas do Windows para ser executada na inicialização do sistema (`On startup`), com o único propósito de executar o comando `wsl.exe --boot`, que por sua vez ativa o `systemd` e toda a cadeia de inicialização.
