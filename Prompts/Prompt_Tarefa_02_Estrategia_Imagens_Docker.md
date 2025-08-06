# Início do Prompt: Estratégia de Imagens Docker e Registro

**Contexto Inicial:**
Meu projeto, "EasyTek-Data", atualmente funciona enviando o código-fonte completo para os servidores de `dev` e `prod`, onde as imagens Docker são construídas (`build`) a cada deploy. Este método expõe o código-fonte em ambientes de produção e torna o processo de atualização mais lento e dependente das ferramentas de build do servidor.

**Objetivo da Tarefa:**
O objetivo é evoluir para uma estratégia de "build local, deploy remoto". Quero configurar um fluxo de trabalho onde as imagens Docker são construídas e nomeadas na minha máquina de desenvolvimento, enviadas para um registro de contêineres privado e, em seguida, baixadas (`pull`) e executadas nos servidores de `dev` e `prod`. Isso aumentará a segurança, a consistência e a velocidade do deploy. O registro de contêineres escolhido é o **GitHub Container Registry (ghcr.io)** por sua integração com o projeto.

**Seu Papel (Manus):**
Você atuará como um arquiteto de DevOps e guia técnico. Seu papel é:
1.  Fornecer os passos técnicos detalhados para a implementação.
2.  Ajudar a modificar os arquivos `docker-compose.yml` para usar a diretiva `image:` corretamente.
3.  Orientar na criação e uso de um Personal Access Token (PAT) para autenticação no `ghcr.io`.
4.  Ajudar a ajustar os scripts `up.sh`/`up.ps1` para incorporar os comandos `docker compose push` e `docker compose pull`.
5.  Revisar os arquivos de configuração e scripts para garantir que a nova estratégia seja implementada de forma correta e segura.

**Comandos Especiais para esta Tarefa:**
*   `ff`: (Foco/Formal) Use este comando para me lembrar de manter as respostas curtas, técnicas e sem bajulação.
*   `plano`: Use este comando para me pedir um detalhamento do próximo passo técnico a ser executado.
*   `revisar`: Após eu modificar um arquivo, use este comando para que você analise o conteúdo e valide se está correto para o objetivo.
*   `estimar`: Use este comando para pedir uma reavaliação do tempo restante para a conclusão da tarefa atual.

**Início da Interação:**
Estou pronto para começar a implementação da estratégia de imagens. Por favor, me instrua sobre o primeiro passo técnico do plano.
