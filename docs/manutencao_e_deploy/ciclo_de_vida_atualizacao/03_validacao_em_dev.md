## Fase 3: Validação no Ambiente de Desenvolvimento (VPS)

### 3.1: Acessando a VPS de Desenvolvimento

**Procedimento:**

1 - Conecte-se à VPS de Desenvolvimento via SSH:
```bash
ssh -i <seu-usuario>@<ip-ou-dominio-da-vps-dev>
```
2 - Navegue até a pasta do projeto:
```bash
cd ~/EasyTek-Data
```

### 3.2: Trazendo a Nova Branch para a VPS (Método Robusto)

**Objetivo:** Sincronizar o código na VPS com a versão exata do GitHub, limpando quaisquer resíduos de testes anteriores.

**Procedimento:**

1 - **Limpe o Repositório:**
```bash
git reset --hard
sudo git clean -fd
```
2 - **Sincronize a Branch `main`:**
```bash
git checkout main
git pull origin main
```
> **Nota sobre Autenticação:** O GitHub não suporta mais autenticação por senha via terminal. É altamente recomendado configurar a autenticação na VPS usando um **Personal Access Token (PAT)** ou uma **chave SSH** para evitar falhas.

3 - **Busque Todas as Atualizações Remotas:**
```bash
git fetch origin
```
4 - **Dê Permissão de Execução aos Scripts (Crítico):**
Este passo é necessário após um `clone` ou `pull` para garantir que os scripts possam ser executados no Linux.
```bash
chmod +x ./scripts/*.sh
```
5 - **Mude para a Sua Branch de Trabalho:**
```bash
git checkout <nome-da-sua-branch>
```

### 3.3: Implantando e Testando a Nova Versão

**Procedimento:**

1 - **Inicie a Aplicação no Ambiente de `dev`:**
```bash
./scripts/up.sh dev
```
2 - **Monitore a Inicialização (Opcional):**  

*   `docker ps` (listar contêineres)  
*   `docker logs <nome-do-container>` (verificar logs)

3 - **Teste a Aplicação:** Acesse o subdomínio de desenvolvimento (ex: `https://dev.meuprojeto.com` ) e valide sua funcionalidade.

### 3.4: Finalizando e Limpando o Ambiente de Desenvolvimento

**Procedimento:**

1 - **Pare os contêineres do projeto:**
```bash
./scripts/down.sh dev
```
2 - **Execute a limpeza geral do Docker:**
```bash
docker container prune -f
docker image prune -a -f
docker volume prune -f
docker network prune -f
```
