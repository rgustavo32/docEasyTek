Com certeza! Aqui está a documentação completa e aprimorada, pronta para você copiar e colar no seu arquivo Markdown.
markdown
# Atualizando a Aplicação

Este documento descreve o ciclo de vida completo para adicionar uma nova funcionalidade ou correção ao projeto.

## Fase 1: Preparação e Início de uma Nova Tarefa

### 1.1: Sincronizando a Branch `main` Local

**Objetivo:** Garantir que seu ambiente de desenvolvimento local (no seu PC Windows) esteja com a versão mais recente e estável do projeto antes de iniciar qualquer nova alteração. Isso evita conflitos futuros.

**Procedimento:**

**1 - Abra o terminal** (Git Bash, PowerShell, etc.) na pasta do seu projeto no Windows.

**2 - Verifique o estado atual do seu repositório.** Antes de qualquer coisa, execute o comando:
```bash
git status
```
Este comando é seu melhor amigo. Ele dirá em qual branch você está e se existem alterações não salvas.  
**3 - Mude para a sua branch principal.** O nome padrão é main, mas pode ser master em projetos mais antigos.
```bash
git checkout main
```
**4 -  Puxe as últimas atualizações do GitHub.** Este comando baixa quaisquer alterações que possam ter sido mescladas na branch main desde a última vez que você a atualizou.
```bash
git pull origin main
```
Resultado Esperado: O terminal deve exibir a mensagem "Already up to date." se não houver nenhuma mudança, ou listará os arquivos que foram atualizados. Agora, sua máquina local está perfeitamente alinhada com a versão oficial do projeto.

### 1.1.1: Lidando com Cenários Inesperados
**Procedimento (Use com CUIDADO - apaga alterações não salvas):**

**1 - Certifique-se de que está na branch `main`:**
```bash
git checkout main
```
**2 - Force a sua branch local a ser idêntica à do repositório remoto.** Este comando descarta todas as alterações nos arquivos que o Git já conhece.
```bash
git reset --hard origin/main
```

--hard: Este é o parâmetro que descarta todas as alterações nos arquivos que o Git conhece.

**3 - Limpe quaisquer arquivos extras que não são rastreados pelo Git.** Este é o passo de "limpeza pesada" final.

```bash
git clean -fd
```
-f: Força a remoção de arquivos.  
-d: Remove também diretórios.  
Resultado Esperado: Seu terminal estará limpo e a pasta do seu projeto será uma cópia exata e sem modificações da branch main do GitHub. Agora você pode prosseguir para o passo 1.2 com segurança.

### 1.2: Criando uma Nova Branch de Trabalho

**Procedimento:**

1.  Certifique-se de que você está na branch `main` e que ela está atualizada.
2.  Crie e mude para a nova branch com um único comando.
```bash
git checkout -b <tipo>/<descricao-curta>
```

#### **Solução de Problemas caso ocorrer o Erro `fatal: a branch named '<nome-da-branch>' already exists`**

Este erro ocorre se você tentar criar uma branch com um nome que já existe no seu repositório local, geralmente de uma tentativa anterior que foi abandonada. Você tem duas opções:

*   **Opção A: Deletar a branch antiga e criar uma nova (Recomendado para recomeçar do zero).**

**1 - Primeiro, delete a branch antiga.** O `-D` (maiúsculo) força a exclusão.
```bash
git branch -D <nome-da-branch-antiga>
```
*Exemplo:* `git branch -D feat/simpleUpdate`

**2 - Agora, execute o comando de criação de branch novamente.**
```bash
git checkout -b <nome-da-branch-antiga>
```

*   **Opção B: Reutilizar a branch existente.**
    Se você apenas quer continuar o trabalho na branch que já existe, em vez de criar uma nova, simplesmente mude para ela:
```bash
git checkout <nome-da-branch-existente>
```

Padrões de Nomenclatura para <tipo>:

* feat: Para uma nova funcionalidade (feature).
    * Exemplo: git checkout -b feat/login-com-google
* fix: Para uma correção de bug.
    * Exemplo: git checkout -b fix/ajuste-botao-salvar
* infra: Para mudanças na infraestrutura (Docker, configurações de deploy, etc.).
    * Exemplo: git checkout -b infra/atualizar-versao-docker
* docs: Para mudanças na documentação.
    * Exemplo: git checkout -b docs/adicionar-guia-de-instalacao
    
Resultado Esperado: O terminal exibirá a mensagem "Switched to a new branch <nome-da-sua-branch>. A partir deste momento, todas as suas alterações serão salvas nesta nova branch, deixando a main intacta.
