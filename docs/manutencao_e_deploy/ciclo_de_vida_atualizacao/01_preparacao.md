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
Ao tentar executar o git checkout main (passo 3), você pode encontrar uma mensagem de erro ou um comportamento inesperado se tiver alterações locais não salvas.

Cenário: Você tem alterações não salvas e precisa descartá-las para começar do zero.
Para forçar o seu ambiente a ficar exatamente igual ao repositório remoto, descartando todo o trabalho local, siga estes passos:
Objetivo: Resetar completamente o seu ambiente local para uma cópia limpa e idêntica à da branch main no GitHub.
Procedimento (Use com CUIDADO - apaga alterações não salvas):
**1 - Certifique-se de que está na branch main:
```bash
git checkout main
```
**2 - Descarte todas as alterações locais e limpe arquivos não rastreados.** Este é o passo de "limpeza pesada".
```bash
git clean -fd
```
* -f: Força a remoção de arquivos.
* -d: Remove também diretórios.
**3 - Force a sua branch local a ser idêntica à do repositório remoto**.
```bash
git reset --hard origin/main
```
--hard: Este é o parâmetro que descarta todas as alterações nos arquivos que o Git conhece.
Resultado Esperado: Seu terminal estará limpo e a pasta do seu projeto será uma cópia exata e sem modificações da branch main do GitHub. Agora você pode prosseguir para o passo 1.2 com segurança.
**1.2: Criando uma Nova Branch de Trabalho**
Objetivo: Isolar suas novas alterações em um espaço de trabalho separado. Isso permite que você trabalhe em uma nova funcionalidade ou correção sem afetar a estabilidade da branch main.
Procedimento:
1 - Certifique-se de que você está na branch main e que ela está atualizada (conforme o passo 1.1).
2 - Crie e mude para a nova branch com um único comando. O nome da branch deve seguir um padrão para manter a organização.
```bash
git checkout -b <tipo>/<descricao-curta>
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
