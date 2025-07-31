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

### 3.2: Trazendo a Nova Branch para a VPS

**Objetivo:** Baixar a branch com suas novas alterações, que até agora só existia no seu PC e no GitHub, para o servidor de desenvolvimento.

**Procedimento:**

**1 - Busque Todas as Atualizações do GitHub:** Primeiro, atualize a lista de todas as branches e commits que existem no repositório remoto. Este comando não altera nenhum arquivo ainda, apenas atualiza o "mapa" do Git local.
```bash
git fetch origin
```

**2 - Mude para a Sua Branch de Trabalho:** Agora que a VPS conhece a sua nova branch, mude para ela.
```bash
git checkout <nome-da-sua-branch>
```
*Exemplo:* `git checkout feat/login-com-google`

**3 - Garanta que a Branch Local está Sincronizada:** Embora o `checkout` geralmente já traga a versão mais recente, é uma boa prática executar um `pull` para garantir que você tem exatamente a última versão que foi enviada ao GitHub.
```bash
git pull
```

**Resultado Esperado:** O terminal confirmará que você mudou para a sua branch de trabalho e que ela está atualizada. A pasta `~/EasyTek-Data` na VPS de Desenvolvimento agora contém exatamente o mesmo código que você finalizou no seu PC.

### 3.3: Implantando e Testando a Nova Versão

**Objetivo:** Executar a nova versão do código no servidor de desenvolvimento e realizar os testes finais para garantir que tudo funciona perfeitamente no ambiente real.

**Procedimento:**

**1 - Verifique as Configurações do Ambiente:** Lembre-se de que a VPS de Desenvolvimento pode ter configurações específicas. Garanta que os arquivos `.env` e `docker-compose.yml` (principalmente a seção `networks`) estão corretos para este ambiente.

**2 - Construa as Novas Imagens e Suba os Contêineres:** Use o Docker Compose para implantar a aplicação. O argumento `--build` é essencial para garantir que o Docker use seu novo código para criar as imagens, em vez de usar versões antigas em cache.
```bash
docker compose up -d --build
```

**3 - Monitore a Inicialização (Opcional, mas recomendado):** Verifique se todos os contêineres subiram sem erros.

*   Liste os contêineres em execução:
```bash
docker ps
```
*   Se algum contêiner estiver reiniciando ou ausente, verifique seus logs para encontrar a causa do problema:
```bash
docker logs <nome-do-container>
```

**4 - Teste a Aplicação no Domínio de Desenvolvimento:**

*   Abra seu navegador e acesse o subdomínio correspondente à sua funcionalidade no ambiente de desenvolvimento (ex: `https://app.dev.meuprojeto.com` ).
*   Realize todos os testes necessários para validar a nova funcionalidade e também para garantir que nenhuma funcionalidade antiga foi quebrada (testes de regressão).

**Resultado Esperado:** A nova versão da sua aplicação está no ar e funcionando corretamente no ambiente de desenvolvimento. Se você encontrar algum problema, volte para a Fase 2 (Desenvolvimento Local), corrija, envie as alterações para o GitHub e repita os passos da Fase 3.

---

