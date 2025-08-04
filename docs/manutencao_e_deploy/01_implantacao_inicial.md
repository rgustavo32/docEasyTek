# Guia 1: Restabelecimento a Partir do Zero

Este guia reconstrói o projeto do zero, aplicando as correções de rede e permissões.

**Pré-requisito:** A "Destruição Controlada" foi concluída.


**1 - Clonando o Repositório do Projeto**

Este passo irá baixar todos os arquivos do projeto do GitHub para a sua VPS.

**Cenário 1:** Implantar a Versão Principal (Estável)

Este é o cenário padrão para um ambiente de **produção**. Você irá clonar e utilizar a branch `main`, que contém a versão mais estável e testada do código.

**Procedimento:**

*  Execute o comando `clone`. Ele automaticamente selecionará a branch `main`.
```bash
git clone https://github.com/rgustavo32/EasyTek.git EasyTek-Data
```
*  Entre na pasta do projeto que foi criada.
```bash
cd EasyTek-Data
```

Agora você está pronto para prosseguir, utilizando a versão principal do projeto.

**Cenário 2:** Implantar uma Funcionalidade Específica para Teste

Este é o cenário para um ambiente de **teste** ou **homologação**. Você precisa testar uma branch específica (ex: `fix/loading-ux` ) que ainda não foi mesclada na `main`.

**Procedimento:**

*  Primeiro, clone o repositório como no cenário padrão. Isso baixa todas as branches, incluindo a que você quer testar.
```bash
git clone https://github.com/rgustavo32/EasyTek.git EasyTek-Data
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


**2. Configurando as Variáveis de Ambiente**

Sua aplicação precisa de chaves e senhas para se conectar a serviços como o banco de dados e o broker MQTT. Essas informações são armazenadas em um arquivo chamado `.env`, que não é salvo no Git por razões de segurança. Você precisará criá-lo manualmente.

**Procedimento:**

1.  Na pasta raiz do projeto (`EasyTek-Data`), crie um novo arquivo chamado `.env`.
    ```bash
    # No Linux (VPS)
    nano .env
    ```

2.  Copie o conteúdo do template abaixo, cole no arquivo `.env` e **substitua os valores** com suas credenciais reais.

```env
# Variáveis de Ambiente para o Projeto EasyTek-Data

# --- Configurações do Banco de Dados MongoDB ---
MONGO_URI=mongodb+srv://<seu_usuario>:<sua_senha>@seu_cluster_aqui
DB_NAME=Cluster-EasyTek
COLLECTION_NAME=DecapadoPerformance
OTHER_COLLECTION_NAME=DecapadoFalhas
ENERGY_COLLECTION=DecapadoEnergia
TEMP_COLLECTION=DecapadoTemp

# --- Configurações do Broker MQTT ---
MQTT_BROKER_ADDRESS=seu_broker_aqui.s1.eu.hivemq.cloud
MQTT_BROKER_PORT=8883
MQTT_USERNAME=<seu_usuario_mqtt>
MQTT_PASSWORD=<sua_senha_mqtt>

# --- Chave Secreta da Aplicação Web ---
# Gere uma chave forte com: python3 -c 'import secrets; print(secrets.token_hex(32))'
SECRET_KEY=<sua_chave_secreta_aqui>

# --- Configurações de Rede Interna ---
GATEWAY_URL=http://event-gateway:5001
```

3.  Salve e feche o arquivo (no `nano`, `Ctrl+X`, depois `Y`, e `Enter` ).

**Resultado:** O Docker Compose agora lerá automaticamente este arquivo e injetará as variáveis nos contêineres corretos, permitindo que a aplicação se conecte aos serviços.


**3 - Corrigir Permissões do Node-RED:** Dê permissão de escrita na pasta de configuração *antes* de subir o contêiner.
```bash
sudo chmod -R 777 ./node-red-config
```

**4 - Inicializando os Serviços (Ordem Correta)**

Nossa arquitetura requer que a rede seja criada pelo proxy antes que a aplicação principal seja iniciada. A configuração do projeto já está preparada para isso, basta seguir a ordem correta.

**4.1 - Inicie a Infraestrutura de Rede (Proxy):**
Este comando utiliza o arquivo `proxy-compose.yml` para iniciar o Nginx Proxy Manager e criar a rede compartilhada `easytek-net`.

> **Nota sobre Restauração:** Se você está restaurando um backup do Nginx Proxy Manager, execute o comando `sudo mv ~/nginx-proxy-manager-BACKUP ~/nginx-proxy-manager` **antes** de executar o comando abaixo.

```bash
docker-compose -f proxy-compose.yml up -d
```

**4.2 - Inicie a Aplicação Principal:**
Este comando utiliza o arquivo docker-compose.yml principal. Graças à configuração external: true (que deve estar no seu docker-compose.yml versionado no Git), os serviços irão se conectar automaticamente à rede easytek-net criada no passo anterior.
```bash
docker-compose up --build -d
```
---

**Ação Necessária no seu Código:**

Para que este novo passo da documentação funcione, você precisa garantir que o arquivo `docker-compose.yml` no seu repositório Git tenha a seção `networks` da seguinte forma (e apenas desta forma):

```yaml
networks:
  easytek-net:
    external: true
```

```bash
docker-compose -f proxy-compose.yml up -d
```

