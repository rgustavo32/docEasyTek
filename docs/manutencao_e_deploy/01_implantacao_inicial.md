# Guia 1: Restabelecimento a Partir do Zero

Este guia reconstrói o projeto do zero, aplicando as correções de rede e permissões.

**Pré-requisito:** A "Destruição Controlada" foi concluída.

**1 - Clonar o Repositório:** Baixe o código-fonte do GitHub.
```bash
git clone https://github.com/rgustavo32/EasyTek.git EasyTek-Data
```

 **2 - Navegar e Mudar de Branch:**
```bash
cd EasyTek-Data
git checkout [Cat]/[Target]
```

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