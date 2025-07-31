# Guia 3: Destruição Controlada (Reset de Fábrica da VPS)

Este guia apaga o projeto da VPS, mas mantém um backup seguro das configurações do Nginx.

## **Pré-requisito:** Estar logado na VPS que será resetada.

### **1 - Backup Seguro do Proxy:** Renomeie a pasta de dados do Nginx para protegê-la.

```bash
mv ~/nginx-proxy-manager ~/nginx-proxy-manager-BACKUP
```

### **2 - Parar Todos os Contêineres:** Garante que nenhum arquivo esteja em uso. O `|| true` evita erros se não houver contêineres.

```bash
docker stop $(docker ps -a -q) || true
```

### **3 - Remover Todos os Contêineres:**

```bash
docker rm $(docker ps -a -q) || true
```

### **4 - Limpar Volumes Órfãos:** Remove dados persistentes de contêineres (Node-RED, Portainer, etc.).

```bash
docker volume prune -f
```

### **5 - Apagar a Pasta do Projeto:** Use `sudo` para evitar erros de "Permission denied".

```bash
sudo rm -rf ~/EasyTek-Data
```

---


