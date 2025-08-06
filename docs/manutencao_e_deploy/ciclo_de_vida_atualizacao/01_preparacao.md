# Atualizando a Aplicação

Este documento descreve o ciclo de vida completo para adicionar uma nova funcionalidade ou correção ao projeto.

## Fase 1: Preparação e Início de uma Nova Tarefa

### 1.1: Sincronizando a Branch `main` Local

**Objetivo:** Garantir que seu ambiente de desenvolvimento local esteja com a versão mais recente e estável do projeto antes de iniciar qualquer nova alteração.

**Procedimento:**

1 - Abra o terminal na pasta do seu projeto.  
2 - Verifique o estado atual do seu repositório:  
```bash
git status
```
3 -  Mude para a sua branch principal:  
```bash
git checkout main
```
4 - Puxe as últimas atualizações do GitHub:  
```bash
git pull origin main
```
**Resultado Esperado:** O terminal deve exibir a mensagem "Already up to date." se não houver nenhuma mudança, ou listará os arquivos que foram atualizados.

### 1.1.1: Lidando com Cenários Inesperados
**Procedimento (Use com CUIDADO - apaga alterações não salvas):**

1 - Certifique-se de que está na branch `main`:
```bash
git checkout main
```
2 - Force sua branch local a ser idêntica à do repositório remoto, descartando alterações locais:
```bash
git reset --hard origin/main
```
3 - Limpe quaisquer arquivos e diretórios extras que não são rastreados pelo Git:
```bash
git clean -fd
```
**Resultado Esperado:** Seu terminal estará limpo e a pasta do seu projeto será uma cópia exata da branch `main` do GitHub.

### 1.2: Criando uma Nova Branch de Trabalho

**Procedimento:**

1 - Certifique-se de que você está na branch `main` e que ela está atualizada.  
2 - Crie e mude para a nova branch com um único comando:  
```bash
git checkout -b <tipo>/<descricao-curta>
```

#### Solução de Problemas: Erro `fatal: a branch named '...' already exists`

Este erro ocorre se você tentar criar uma branch com um nome que já existe.

*   **Opção A: Deletar a branch antiga e criar uma nova (Recomendado).**
```bash
# O -D (maiúsculo) força a exclusão.
git branch -D <nome-da-branch-antiga>
# Agora, crie a branch novamente.
git checkout -b <nome-da-branch-antiga>
```

*   **Opção B: Reutilizar a branch existente.**
```bash
git checkout <nome-da-branch-existente>
```

#### Padrões de Nomenclatura para `<tipo>`:

*   `feat`: Para uma nova funcionalidade (feature).
*   `fix`: Para uma correção de bug.
*   `infra`: Para mudanças na infraestrutura (Docker, deploy).
*   `docs`: Para mudanças na documentação.

**Resultado Esperado:** O terminal exibirá a mensagem "Switched to a new branch `<nome-da-sua-branch>`". A partir deste momento, todas as suas alterações serão salvas nesta nova branch, deixando a `main` intacta.
