
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


### 5.2: Sincronizando a Branch `main` na Produção (Método Robusto)

**Objetivo:** Forçar o código no servidor de produção a ser um espelho exato da branch `main` no GitHub.

**Procedimento:**

1.  **Atualize o "mapa" local do Git:** Este comando baixa as informações mais recentes do GitHub, garantindo que o próximo passo use a versão correta.
```bash
git fetch origin
```

2.  **Garanta que está na branch `main`:**
```bash
git checkout main
```

3.  **Force a sincronização com o repositório remoto:**
```bash
git reset --hard origin/main
```

4.  **(Verificação) Confirme o Commit Ativo:**
```bash
git log -1 --pretty=format:"%h - %s"
```

### 5.3: Implantando a Versão Final (Método Robusto)

**Objetivo:** Realizar uma implantação limpa e ordenada no ambiente de produção, removendo todos os artefatos antigos antes de iniciar a nova versão.

**Procedimento:**

**1 -  Pare e remova os contêineres do projeto:**
```bash
docker compose down
docker compose -f proxy-compose.yml down
#=======================================
# Use os arquivos de configuração de PROD para garantir a parada correta
docker compose --env-file ./environments/prod/.env -f docker-compose.yml -f ./environments/prod/docker-compose.override.yml down
docker compose --env-file ./environments/prod/.env -f proxy-compose.yml down

```

**2 - Execute a limpeza geral do Docker:** Este passo remove contêineres, imagens, volumes e redes obsoletas, garantindo que a implantação ocorra em um ambiente limpo.
```bash
docker container prune -f
docker image prune -a -f
docker volume prune -f
docker network prune -f
```

**3 - Inicie a Infraestrutura de Rede (Proxy):** Recrie a rede `easytek-net` e inicie o Nginx Proxy Manager.
```bash
docker compose --env-file ./environments/prod/.env -f proxy-compose.yml up -d
```

**4 - Construa e Inicie a Aplicação Principal:** Construa as novas imagens a partir do código atualizado e inicie os serviços da aplicação.
```bash
docker compose --env-file ./environments/prod/.env -f docker-compose.yml -f ./environments/prod/docker-compose.override.yml up -d --build

```

**5 - Verificação Final:** Acesse o domínio de produção para confirmar que a nova versão está no ar e funcionando corretamente.
