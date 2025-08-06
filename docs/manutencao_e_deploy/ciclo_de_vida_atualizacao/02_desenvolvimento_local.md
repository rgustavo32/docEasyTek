## Fase 2: Desenvolvimento e Teste Local

### 2.1: Configuração do Ambiente Local (Passo Único)

**Objetivo:** Garantir que seu computador consiga acessar os serviços locais (webapp, Node-RED, etc.) de forma estável através dos domínios definidos.

> **Importante:** Este procedimento só precisa ser feito **uma vez** na sua máquina.

**Procedimento (Windows):**

1 - **Abra o Bloco de Notas como Administrador:**
    *   No Menu Iniciar, digite `Bloco de Notas`.
    *   Clique com o botão direito e selecione **"Executar como administrador"**.

2 - **Abra o arquivo `hosts`:**  

*   No Bloco de Notas, vá em `Arquivo` > `Abrir`.  
*   Navegue até `C:\Windows\System32\drivers\etc`.  
*   Mude o filtro de "Documentos de Texto (\*.txt)" para **"Todos os arquivos (\*.\*)"**.  
*   Selecione o arquivo `hosts` e clique em `Abrir`.  

3 - **Adicione os Mapeamentos:**
    *   No final do arquivo, adicione as seguintes linhas:
```
# Mapeamento local para o projeto EasyTek-Data
127.0.0.1 subdominio.dominio.duckdns.org
127.0.0.1 subdominio.dominio.duckdns.org
127.0.0.1 subdominio.dominio.duckdns.org
127.0.0.1 subdominio.dominio.duckdns.org
```

4 - **Salve e Feche** o arquivo.

### 2.2: Codificação e Testes na Máquina Local

**Objetivo:** Desenvolver a funcionalidade ou correção e garantir que ela funcione corretamente no ambiente local.

**Procedimento:**

1 - **Desenvolva o Código:** Utilize seu editor para fazer as alterações necessárias.

2 - **Inicie a Aplicação Localmente:**
*   Abra o terminal na pasta raiz do projeto.
*   Se estiver no Windows, execute:
```powershell
./scripts/up.ps1 -env local
```
*   Se estiver no Linux/macOS, execute:
```bash
./scripts/up.sh local
```

3 - **Acesse a aplicação** no seu navegador (ex: `http://ldashtek.loc-easytek.duckdns.org` ) e realize os testes.
> **Nota:** Após iniciar os serviços pela primeira vez, pode haver um pequeno atraso (até 2 minutos) para que os domínios fiquem acessíveis, devido à emissão inicial dos certificados SSL.

### 2.3: Salvando o Progresso com Commits

**Objetivo:** Criar "pontos de salvamento" (commits) lógicos e bem descritos do seu trabalho.

**Procedimento:**

1.  `git status` para ver os arquivos modificados.
2.  `git add .` para adicionar todos os arquivos ao "palco".
3.  `git commit -m "<tipo>(<escopo>): <mensagem>"` para salvar as alterações.
    *   **Exemplo:** `git commit -m "feat(webapp): Add Google authentication button"`

### 2.4: Enviando a Nova Branch para o GitHub

**Objetivo:** Publicar sua branch de trabalho e seus commits no repositório remoto (GitHub).

**Procedimento:**

1 - Para o primeiro envio da branch:
```bash
git push --set-upstream origin <nome-da-sua-branch>
```
2 - Para envios futuros na mesma branch:
```bash
git push
```

### 2.5: Finalizando e Limpando o Ambiente Local

**Objetivo:** Liberar recursos do seu computador após a conclusão do trabalho local.

**Procedimento:**

1 - **Pare os contêineres do projeto:**
*   No Windows:
```powershell
./scripts/down.ps1 -env local
```
*   No Linux/macOS:
```bash
./scripts/down.sh local
```
2 - **(Opcional) Execute a limpeza geral do Docker:**
```bash
docker container prune -f
docker image prune -a -f
docker volume prune -f
docker network prune -f
```