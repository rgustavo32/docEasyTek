## Fase 3: Validação no Ambiente de Desenvolvimento

### 3.1: Acessando a VPS de Desenvolvimento

**Objetivo:** Preparar o ambiente de testes real, que é um clone do ambiente de produção. É aqui que validamos se o código funciona em um servidor Linux real, e não apenas na máquina local.

**Procedimento:**

**1 - Conecte-se à sua VPS de Desenvolvimento** usando o protocolo SSH (Secure Shell).
    *   Abra seu terminal (Git Bash, PowerShell, Prompt de Comando do Windows, etc.).
    *   Utilize o comando `ssh` com seu nome de usuário e o endereço IP ou domínio da sua VPS.
```bash
ssh <seu-usuario>@<ip-ou-dominio-da-vps-dev>
```
*Exemplo* `ssh admin@198.51.100.10` ou `ssh admin@dev.meuprojeto.com`

**2 - Navegue até a Pasta do Projeto:** Uma vez conectado, vá para o diretório onde o código da aplicação está localizado.
```bash
cd ~/EasyTek-Data
```

**Resultado Esperado:** Você estará logado no terminal da sua VPS de Desenvolvimento e dentro da pasta correta, pronto para baixar e implantar a nova versão da aplicação para testes.

### 3.2: Trazendo a Nova Branch para a VPS (Método Robusto)

**Objetivo:** Baixar a branch com suas novas alterações para o servidor de desenvolvimento, garantindo que a versão na VPS seja um clone exato da versão no GitHub e evitando conflitos com arquivos modificados de testes anteriores.

**Procedimento:**

**1 -  Force a Limpeza do Repositório:** Este comando descarta quaisquer alterações locais (como as feitas pelo Node-RED) na branch em que você estiver.
```bash
git reset --hard
sudo git clean -fd
```

**2 - Mude para a `main` e Sincronize:** Agora que o repositório está limpo, mude para a `main` e garanta que ela esteja alinhada com a versão do GitHub.
```bash
git checkout main
git pull origin main
```

**3 - Busque as Atualizações do GitHub:** Atualize o "mapa" de todas as branches e commits remotos.
```bash
git fetch origin
```

**4 - Mude para a Sua Branch de Trabalho.**
```bash
git checkout <nome-da-sua-branch>
```


*Exemplo:* `git checkout fix/Arquivos_NodeRed_Update`

**Resultado Esperado:** O terminal confirmará que você mudou para a sua branch de trabalho. A pasta ~/EasyTek-Data na VPS agora contém o código exato que você enviou do seu PC, pronto para o deploy de teste.


### 3.3: Implantando e Testando a Nova Versão

**Objetivo:** Executar a nova versão do código no servidor de desenvolvimento na ordem correta para garantir que a rede e a aplicação se conectem perfeitamente.

**Procedimento:**

**1 - Pare e Remova Completamente os Contêineres Antigos (se houver):** Para garantir um teste limpo, paramos tanto a aplicação quanto a infraestrutura.
```bash
docker compose down
docker compose -f proxy-compose.yml down
```

**2 - Inicie a Infraestrutura de Rede (Proxy):** Este é o primeiro passo crucial. O comando abaixo utiliza o proxy-compose.yml para iniciar o Nginx Proxy Manager e, mais importante, criar a rede compartilhada easytek-net.
```bash
docker compose -f proxy-compose.yml up -d
```
**3 - Construa e Inicie a Aplicação Principal:** Agora que a rede easytek-net existe, o comando a seguir irá construir as novas imagens da sua branch e conectar os contêineres da aplicação à rede que acabamos de criar.

```bash
docker compose up --build -d
```

**4 - Monitore a Inicialização (Opcional, mas recomendado):** Verifique se todos os contêineres subiram sem erros.

*   Liste os contêineres em execução: `docker ps`
*   Se necessário, verifique os logs: `docker logs <nome-do-container>`

**5 - Teste a Aplicação:**

*   Acesse o subdomínio de desenvolvimento (ex: `https://dev-easytek.duckdns.org` ) e valide sua nova funcionalidade.

**Resultado Esperado:** A nova versão da sua aplicação está no ar e funcionando corretamente no ambiente de desenvolvimento.

### 3.4: Finalizando e Limpando o Ambiente de Desenvolvimento

**Objetivo:** Após a validação bem-sucedida, realizar uma limpeza completa do ambiente Docker na VPS de DEV para liberar recursos e evitar o acúmulo de dados obsoletos.

**Procedimento:**

Este deve ser o último passo executado na VPS de DEV, logo após a conclusão dos testes e antes de prosseguir para o merge do código no GitHub.

**1 - **Pare e remova os contêineres do projeto:**
```bash
docker compose down
docker compose -f proxy-compose.yml down
```

**2 - Execute a limpeza geral do Docker:**
```bash
# Remove todos os contêineres parados
docker container prune -f

# Remove todas as imagens que não estão sendo usadas por nenhum contêiner
docker image prune -a -f

# Remove todos os volumes não utilizados
docker volume prune -f

# Remove todas as redes não utilizadas
docker network prune -f
```

**Resultado Esperado:** O ambiente de DEV estará inativo e completamente limpo, concluindo a fase de validação e pronto para o próximo ciclo de testes.


---

