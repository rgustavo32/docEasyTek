## Fase 5: Deploy Final em Produção

### 5.1: Acessando a VPS de Produção

**Objetivo:** Conectar-se ao servidor de produção, o ambiente real onde os usuários finais acessam a aplicação.

**Procedimento:**

1 - Conecte-se à VPS de Produção via SSH:
```bash
ssh -i <seu-usuario>@<ip-ou-dominio-da-vps-prod>
```
2 - Navegue até a pasta do projeto:
```bash
cd ~/EasyTek-Data
```

### 5.2: Sincronizando a Branch `main` na Produção

**Objetivo:** Forçar o código no servidor de produção a ser um espelho exato da branch `main` no GitHub.

**Procedimento:**

1 - **Atualize o "mapa" local do Git:**
```bash
git fetch origin
```
> **Nota sobre Autenticação:** Assim como no ambiente de DEV, configure a autenticação via **Personal Access Token (PAT)** ou **chave SSH**.

2 - **Garanta que está na branch `main`:**
```bash
git checkout main
```
3 - **Force a Sincronização e Limpeza:**
```bash
git reset --hard origin/main
sudo git clean -fd
```
4 - **Dê Permissão de Execução aos Scripts (Crítico):**
```bash
chmod +x ./scripts/*.sh
```
5 - **(Verificação) Confirme o Commit Ativo:**
```bash
git log -1 --pretty=format:"%h - %s"
```

### 5.3: Implantando a Versão Final (Método Robusto)

**Objetivo:** Realizar uma implantação limpa e ordenada no ambiente de produção.

**Procedimento:**

1 - **Pare e Remova Completamente os Serviços Antigos:**
```bash
./scripts/down.sh prod
```
2 - **Execute a Limpeza Geral do Docker:**
```bash
docker container prune -f
docker image prune -a -f
docker volume prune -f
docker network prune -f
```
3 - **Construa e Inicie a Nova Versão:**
```bash
./scripts/up.sh prod
```
4 - **Verificação Final:** Acesse o domínio de produção para confirmar que a nova versão está no ar e funcionando corretamente.
