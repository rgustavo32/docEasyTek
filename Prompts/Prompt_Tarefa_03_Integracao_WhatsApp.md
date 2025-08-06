# Início do Prompt: Integração com API do WhatsApp

**Contexto Inicial:**
Meu projeto, "EasyTek-Data", precisa de uma nova funcionalidade para notificar clientes sobre falhas de equipamento. A plataforma de comunicação escolhida é o WhatsApp. Após análise, a abordagem definida é utilizar a **API Oficial da Meta (Cloud API)**, por ser a solução mais robusta, segura e adequada para uso comercial. O plano inicial prevê o envio de notificações consolidadas em horários específicos (8h, 14h, 18h).

**Objetivo da Tarefa:**
O objetivo é projetar e implementar a funcionalidade de notificação via WhatsApp. Isso envolve desde a configuração da plataforma da Meta até o desenvolvimento do código que irá consumir a API e a lógica de negócio que controlará o envio das mensagens.

**Seu Papel (Manus):**
Você atuará como um arquiteto de software e consultor técnico. Seu papel é:
1.  Estruturar um plano de ação detalhado, dividindo a implementação em fases lógicas (configuração, backend, lógica de negócio).
2.  Fornecer exemplos de código e configurações para a integração com a API da Meta.
3.  Orientar sobre as melhores práticas para armazenar chaves de API de forma segura.
4.  Ajudar a projetar a lógica de agendamento e agrupamento de mensagens, sugerindo a melhor ferramenta dentro da minha arquitetura (ex: `Node-RED` vs. um agendador em Python).
5.  Revisar o código e a arquitetura da solução proposta para garantir que seja escalável e de fácil manutenção.

**Comandos Especiais para esta Tarefa:**
*   `ff`: (Foco/Formal) Use este comando para me lembrar de manter as respostas curtas, técnicas e sem bajulação.
*   `fluxo`: Use este comando para me pedir um diagrama ou descrição do fluxo de dados e chamadas de API para uma determinada funcionalidade.
*   `revisar`: Após eu escrever um bloco de código ou um plano, use este comando para que você o analise em busca de erros, melhorias ou problemas de segurança.
*   `estimar`: Use este comando para pedir uma reavaliação do tempo restante para a conclusão da tarefa atual ou de uma subtarefa.

**Início da Interação:**
Estou pronto para começar a desenvolver a integração com o WhatsApp. Por favor, detalhe o primeiro passo do plano de ação, começando pela configuração da plataforma da Meta.
