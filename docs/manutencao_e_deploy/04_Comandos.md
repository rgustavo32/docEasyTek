# Guia de Operações

## O Estado Atual: Arquitetura Final

Na arquitetura atual, temos dois ambientes idênticos, robustos e independentes.

| Característica      | VPS de Produção (`easytek.duckdns.org`)                      | VPS de Desenvolvimento (`dev-easytek.duckdns.org`)           |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **Função**          | Servir a aplicação principal para os usuários.               | Servir para testes, novas funcionalidades e como clone de segurança. |
| **Proxy**           | Nginx Proxy Manager **próprio e isolado**.                   | Nginx Proxy Manager **próprio e isolado**.                   |
| **Dados do Proxy**  | Salvos em `~/nginx-proxy-manager/`                           | Salvos em `~/nginx-proxy-manager/`                           |
| **Domínios**        | Gerencia `*.easytek.duckdns.org`                             | Gerencia `*.dev-easytek.duckdns.org`                         |
| **Independência**   | **Total.** Pode ser reiniciada ou desligada sem afetar a outra. | **Total.** Pode ser reiniciada ou desligada sem afetar a outra. |

**Vantagens:** Se a VPS de Desenvolvimento quebrar, a de Produção continua funcionando perfeitamente, e vice-versa. A complexidade da VPN e do proxy central foi eliminada.

---

## Guia de Operações: Como Gerenciar seu Novo Setup

Este é o seu manual para o dia a dia. Todos os comandos devem ser executados dentro da pasta `~/EasyTek-Data` na VPS correspondente.

### 1. Como Parar e Iniciar a Aplicação (sem tocar no Proxy)

Use este procedimento para atualizar o código da sua aplicação (webapp, node-red, etc.).

*   **Para parar a aplicação:**
```bash
docker compose down
```
*   **Para iniciar a aplicação:**
```bash
docker compose up -d
```
*   **Para forçar a recriação dos contêineres (após mudar o `docker-compose.yml`):**
```bash
docker compose up -d --force-recreate
```

### 2. Como Parar e Iniciar o Proxy (sem tocar na Aplicação)

Você raramente precisará fazer isso.

*   **Para parar o proxy:**
```bash
docker compose -f proxy-compose.yml down
```
*   **Para iniciar o proxy:**
```bash
docker compose -f proxy-compose.yml up -d
```

### 3. Como Reiniciar Tudo (Limpeza Geral)

Se algo der muito errado e você quiser reiniciar todo o ambiente do zero (sem perder os dados do proxy).

```bash
docker compose -f proxy-compose.yml down
docker compose down
```
### 4. Subir na ordem correta
```bash
docker compose -f proxy-compose.yml up -d
docker compose up -d
```
