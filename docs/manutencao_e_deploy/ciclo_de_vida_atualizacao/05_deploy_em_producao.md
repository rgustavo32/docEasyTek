
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


### 5.2: Sincronizando a Branch `main` na Produção

**Objetivo:** Atualizar o código no servidor de produção para que ele corresponda exatamente à versão mais recente da branch `main` no GitHub, que agora inclui as alterações recém-mescladas.

**Procedimento:**

1.  **Mude para a Branch `main`:** É crucial garantir que você está na branch principal antes de puxar as atualizações. O servidor de produção deve sempre rodar o código da `main`.
```bash
git checkout main
```

2.  **Puxe as Atualizações da `main`:** Baixe a versão mais recente do código do GitHub. Como o merge foi feito na Fase 4, este comando trará todas as novas funcionalidades e correções para o servidor.
```bash
git pull origin main
```

**Resultado Esperado:** O terminal mostrará os arquivos que foram alterados, confirmando que o código da nova versão foi baixado com sucesso para a VPS de Produção. A pasta `~/EasyTek-Data` agora contém o código pronto para ser implantado.

### 5.3: Implantando a Versão Final

**Objetivo:** Parar a versão antiga da aplicação e iniciar a nova versão que acabamos de baixar, disponibilizando as atualizações para todos os usuários finais.

**Procedimento:**

1.  **Verifique as Configurações do Ambiente:** Garanta que os arquivos `.env` e `docker-compose.yml` (principalmente a seção `networks`) estão corretos para o ambiente de Produção. Normalmente, nenhuma alteração é necessária nesta etapa, pois a `main` já deve conter a configuração correta para produção.

2.  **Construa as Novas Imagens e Suba os Contêineres:** Use o Docker Compose para implantar a aplicação. O argumento `--build` é essencial para que o Docker utilize o novo código para recriar as imagens.
```bash
docker compose up -d --build
```
*O Docker é inteligente e só reconstruirá as imagens dos serviços cujo código-fonte foi alterado (ex: `webapp`), tornando o processo eficiente.*

3.  **Verificação Final:** Após a implantação, faça uma verificação rápida para garantir que a aplicação está funcionando como esperado no ambiente de produção.
    *   Acesse o domínio principal da sua aplicação (ex: `https://app.meuprojeto.com` ) no navegador.
    *   Teste a nova funcionalidade e verifique se as principais partes do site continuam operacionais.

**Resultado Esperado:** A nova versão da aplicação está no ar, em produção, e acessível a todos os usuários. O ciclo de desenvolvimento e deploy está completo.
