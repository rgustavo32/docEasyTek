# Guia 1: Restabelecimento a Partir do Zero

Este guia reconstrói o projeto do zero, aplicando as correções de rede e permissões.

**Pré-requisito:** A "Destruição Controlada" foi concluída.

**1 - Clonar o Repositório:** Baixe o código-fonte do GitHub.
```bash
git clone https://github.com/rgustavo32/EasyTek.git EasyTek-Data
```

**2 - Clonando o Repositório do Projeto**

Este passo irá baixar todos os arquivos do projeto do GitHub para a sua VPS.

Cenário 1: Implantar a Versão Principal (Estável)

Este é o cenário padrão para um ambiente de **produção**. Você irá clonar e utilizar a branch `main`, que contém a versão mais estável e testada do código.

**Procedimento:**

*  Execute o comando `clone`. Ele automaticamente selecionará a branch `main`.
```bash
git clone https://github.com/rgustavo32/EasyTek.git
```
*  Entre na pasta do projeto que foi criada.
```bash
cd EasyTek-Data
```

Agora você está pronto para prosseguir, utilizando a versão principal do projeto.

Cenário 2: Implantar uma Funcionalidade Específica para Teste

Este é o cenário para um ambiente de **teste** ou **homologação**. Você precisa testar uma branch específica (ex: `fix/loading-ux` ) que ainda não foi mesclada na `main`.

**Procedimento:**

*  Primeiro, clone o repositório como no cenário padrão. Isso baixa todas as branches, incluindo a que você quer testar.
```bash
git clone https://github.com/rgustavo32/EasyTek.git
```
*  Entre na pasta do projeto.
```bash
cd EasyTek-Data
```
*  **Agora, o passo mais importante:** Mude para a branch que você deseja testar. Use o comando `checkout`.
```bash
git checkout <nome-da-sua-branch>
```
*  (Opcional, mas recomendado ) Verifique se você está na branch correta.
```bash
git branch
```
    O terminal deve mostrar um asterisco (`*`) ao lado do nome da sua branch.

Agora a sua VPS está com a versão exata do código que você precisa testar, e você pode prosseguir com os próximos passos da implantação.


**3 - Criar Arquivo de Ambiente:**
```bash
nano .env
```
*Cole suas senhas e chaves aqui. Salve e feche o arquivo.*

**4 - Corrigir Permissões do Node-RED:** Dê permissão de escrita na pasta de configuração *antes* de subir o contêiner.
```bash
sudo chmod -R 777 ./node-red-config
```

**5 - Ajustar Rede para o Ambiente:** Abra o `docker-compose.yml` para edição.
```bash   
nano docker-compose.yml
```
Vá até o final e **garanta que a seção `networks` esteja correta para o ambiente em questão.**

*   **Para a VPS de Desenvolvimento:** 
```yaml
networks:
    easytek-net:
        driver: bridge
```

*   **Para a VPS de Produção:**  
```yaml
networks:
    easytek-net:
        external: true
        name: easytek-net
```
*   **Salve e feche o arquivo.**

**6 - Subir a Aplicação Principal:** Use `--build` para garantir que as imagens sejam criadas corretamente.
```bash
docker compose up -d --build
```

**7 - Restaurar e Iniciar o Proxy:**
```bash
sudo mv ~/nginx-proxy-manager-BACKUP ~/nginx-proxy-manager
docker compose -f proxy-compose.yml up -d
```