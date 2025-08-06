

## Fase 2: Desenvolvimento e Teste Local
## Fase 2: Desenvolvimento e Teste Local

### 2.1: Codificação e Testes na Máquina Local

**Objetivo:** Desenvolver a funcionalidade ou correção e garantir que ela funcione corretamente no ambiente local antes de compartilhá-la.

**Procedimento:**

**1 - Desenvolva o Código:** Utilize seu editor de código para fazer todas as alterações necessárias.

**2 - Teste a Aplicação Localmente:** Para garantir que a aplicação inicie corretamente com a configuração do ambiente **local**, siga a ordem de inicialização.

*   **Abra o terminal** na pasta raiz do projeto.

*   **Passo 2.1: Inicie a Infraestrutura de Rede e Proxy.** Este comando utiliza a configuração definida em `environments/local/.env` para iniciar o Nginx Proxy Manager e criar a rede compartilhada.
```bash
docker compose --env-file ./environments/local/.env -f proxy-compose.yml up -d
```

*   **Passo 2.2: Inicie a Aplicação Principal.** Este comando utiliza o arquivo de configuração base (`docker-compose.yml`) e o sobrepõe com as definições específicas do ambiente local (como o Portainer). O `--build` reconstrói as imagens se você alterou o código.
```bash
docker compose --env-file ./environments/local/.env -f docker-compose.yml -f ./environments/local/docker-compose.override.yml up -d --build
```

*   **Acesse a aplicação** no seu navegador e realize testes manuais para validar o resultado.

### 2.2: Salvando o Progresso com Commits

**Objetivo:** Criar "pontos de salvamento" (commits) lógicos e bem descritos do seu trabalho. Isso cria um histórico claro das alterações e facilita a colaboração e a depuração de problemas.

**Procedimento:**

**1 - Verifique o Status das Alterações:** Antes de fazer um commit, é uma boa prática ver quais arquivos foram modificados.
```bash
git status
```
*(Este comando listará os arquivos modificados em vermelho).*

**2 - Adicione os Arquivos ao "Palco":** Informe ao Git quais arquivos você deseja incluir neste ponto de salvamento. Para adicionar todos os arquivos modificados, use o ponto.
```bash
git add .
```

**3 - Crie o Commit:** Salve as alterações com uma mensagem que siga o padrão de "Commits Convencionais".
```bash
git commit -m "<tipo>(<escopo>): <mensagem>"
```

*   **`<tipo>`:** `feat`, `fix`, `docs`, `infra`, etc.
*   **`<escopo>`:** A parte do projeto que foi alterada (ex: `webapp`, `node-red`, `database`).
*   **`<mensagem>`:** Uma descrição curta e imperativa do que foi feito.

**Exemplos Práticos:**

*   `git commit -m "feat(webapp): Add Google authentication button"`
*   `git commit -m "fix(node-red): Correct payload parsing on MQTT input"`

**Boas Práticas:**

*   Faça commits pequenos e focados. Em vez de um commit gigante no final, salve cada pequena parte funcional da sua tarefa.
*   Escreva mensagens claras. Elas devem explicar *o que* a mudança faz e, se necessário, *por que* ela foi feita.

### 2.3: Enviando a Nova Branch para o GitHub

**Objetivo:** Publicar a sua branch de trabalho e seus commits no repositório remoto (GitHub). Isso cria um backup do seu trabalho na nuvem e o prepara para ser compartilhado e testado em outros ambientes.

**Procedimento:**

1.  **Execute o Comando `push`:** Na primeira vez que você envia uma nova branch, o Git pode pedir para você especificar o destino. O comando a seguir envia a branch atual para o repositório remoto (chamado `origin` por padrão) e a configura para rastrear a versão remota.
```bash
git push --set-upstream origin <nome-da-sua-branch>
```
*Substitua `<nome-da-sua-branch>` pelo nome exato da sua branch de trabalho (ex: `feat/login-com-google`).*

2.  **Para Envios Futuros:** Depois do primeiro `push`, para enviar commits subsequentes na mesma branch, o comando é mais simples:
```bash
git push
```

**Resultado Esperado:** O terminal mostrará o progresso do upload e confirmará que a branch foi enviada com sucesso. Ao acessar a página do seu repositório no GitHub, você verá a nova branch listada e poderá visualizar todos os commits que você enviou.

#### **Solução de Problemas: Erro `[rejected] (non-fast-forward)`**

Este erro ocorre quando você tenta enviar (`push`) uma branch local que tem o mesmo nome de uma branch no GitHub, mas históricos de commits diferentes. Isso é comum se você deletou e recriou uma branch localmente para "refazer" uma tarefa.

O Git rejeita o push para proteger o histórico remoto. Como neste cenário de "refazer" a intenção é justamente substituir a branch remota pela sua nova versão local, você deve forçar o envio.

**Procedimento de Correção:**

*   Use o comando `push` com a flag `--force` ou `-f`.

```bash
git push --force --set-upstream origin <nome-da-sua-branch>
```
*Exemplo:* `git push --force --set-upstream origin feat/simpleUpdate`

> **Atenção:** Use o `--force` com extremo cuidado. Ele sobrescreve a branch no repositório remoto. Só o utilize quando tiver certeza absoluta de que sua versão local é a correta e que a versão remota pode ser descartada, como no caso de refazer uma tarefa do zero.

### 2.4: Finalizando e Limpando o Ambiente Local

**Objetivo:** Liberar recursos e evitar o acúmulo de dados obsoletos após a conclusão do trabalho local.

**Procedimento:**

1.  **Pare e remova os contêineres do projeto:** Use os mesmos arquivos de configuração para garantir que todos os serviços (base e locais) sejam encerrados.
```bash
# Parando a aplicação principal e serviços locais
docker compose --env-file ./environments/local/.env -f docker-compose.yml -f ./environments/local/docker-compose.override.yml down

# Parando o proxy
docker compose --env-file ./environments/local/.env -f proxy-compose.yml down
```

2.  **Execute a limpeza geral do Docker:**
```bash
docker container prune -f
docker image prune -a -f
docker volume prune -f
docker network prune -f
```

*   `container prune`: Limpa contêineres parados.
*   `image prune -a`: Limpa imagens "penduradas" (dangling) e também as que não estão associadas a nenhum contêiner. O `-a` é importante para uma limpeza mais completa.
*   `volume prune`: Limpa volumes órfãos.
*   `network prune`: Limpa redes que não estão em uso.

**Resultado Esperado:** Seu ambiente de desenvolvimento local estará inativo e completamente limpo, pronto para quando você começar a próxima tarefa.


---
