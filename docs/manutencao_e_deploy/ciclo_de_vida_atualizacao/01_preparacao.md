# Atualizando a Aplicação

Este documento descreve o ciclo de vida completo para adicionar uma nova funcionalidade ou correção ao projeto.


## Fase 1: Preparação e Início de uma Nova Tarefa

### 1.1: Sincronizando a Branch `main` Local
**Objetivo:** Garantir que seu ambiente de desenvolvimento local (no seu PC Windows) esteja com a versão mais recente e estável do projeto antes de iniciar qualquer nova alteração. Isso evita conflitos futuros.

**Procedimento:**

**1 - Abra o terminal** (Git Bash, PowerShell, etc.) na pasta do seu projeto no Windows.

**2 - Mude para a sua branch principal.** O nome padrão é `main`, mas pode ser `master` em projetos mais antigos.
```bash
git checkout main
```

**3 - Puxe as últimas atualizações do GitHub.** Este comando baixa quaisquer alterações que possam ter sido mescladas na branch `main` desde a última vez que você a atualizou.
```bash
git pull origin main
```

**Resultado Esperado:** O terminal deve exibir a mensagem "Already up to date." se não houver nenhuma mudança, ou listará os arquivos que foram atualizados. Agora, sua máquina local está perfeitamente alinhada com a versão oficial do projeto.

### 1.2: Criando uma Nova Branch de Trabalho
**Objetivo:** Isolar suas novas alterações em um espaço de trabalho separado. Isso permite que você trabalhe em uma nova funcionalidade ou correção sem afetar a estabilidade da branch `main`.

**Procedimento:**

**1 - Certifique-se de que você está na branch `main`** e que ela está atualizada (conforme o passo 1.1).

**2 - Crie e mude para a nova branch com um único comando.** O nome da branch deve seguir um padrão para manter a organização.
```bash
git checkout -b <tipo>/<descricao-curta>
```

**Padrões de Nomenclatura para `<tipo>`:**

*   `feat`: Para uma nova funcionalidade (feature).
    *   *Exemplo:* `git checkout -b feat/login-com-google`
*   `fix`: Para uma correção de bug.
    *   *Exemplo:* `git checkout -b fix/ajuste-botao-salvar`
*   `infra`: Para mudanças na infraestrutura (Docker, configurações de deploy, etc.).
    *   *Exemplo:* `git checkout -b infra/atualizar-versao-docker`
*   `docs`: Para mudanças na documentação.
    *   *Exemplo:* `git checkout -b docs/adicionar-guia-de-instalacao`

**Resultado Esperado:** O terminal exibirá a mensagem "Switched to a new branch `<nome-da-sua-branch>`. A partir deste momento, todas as suas alterações serão salvas nesta nova branch, deixando a `main` intacta.


---
