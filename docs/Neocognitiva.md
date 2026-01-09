# Proposta
Data: 2025-12-09  
Escopo: Avaliar solicitação do cliente mediante envio de documentação (Plataforma de Dashboards para o LMS.pdf) para avaliação de mão de obra e recursos necessários para customização e implemetação de aplicação.

---

## 1) Solicitação do cliente segundo documentação

### 1.1 Objetivo geral
Criar uma **plataforma de dashboards** para o **Line Monitoring System (LMS)**, centralizando dados do chão de fábrica/linha, com foco em **tomada de decisão**, **monitoramento** e **manutenção preditiva**.

### 1.2 Requisitos explícitos 
- **Interface intuitiva e responsiva** 
- **Barra de navegação** para alternar entre dashboards
- **Gráficos interativos**, capazes de lidar com **grandes volumes** (ex.: “milhões de pontos” em séries temporais)
- **Multiusuário** - Arquitetura deve ser capaz de sustentar usuários simultâneos (Capacidade máxima de Usuarios simultâneos dependerá de recursos disponíveis e necessidade de alteração de arquitetura se necessidade de usuários simultâneos for muito alta.)
- **Controle de acesso**: usuários comuns vs **administrador** (ações restritas)
- **Preferencialmente nativa ao Node-RED** ou com **integração simples**, permitindo que stakeholder ajuste/expanda (mudanças complexas viram novos projetos)
- **Compatível com a arquitetura existente**:
  - rodar **local** nos servidores do LMS
  - **MySQL** para configurações/parâmetros
  - **InfluxDB** para séries temporais
  - **Sem nuvem**
- **Robusta, segura e resistente a erros**

### 1.3 Dashboards / telas exemplificadas no PDF (o “escopo funcional”)
- **Página inicial**: “central de métricas” (ex.: VMs offline/alto uso, comportamento anômalo, PLCs offline/jumpers, temperatura por shop acima do limite, etc.)
- **LMS Monitor**: status das Máquinas Virtuais do LMS
- **PLC Monitoring**: status PLCs com seletores (shop/equipamento)
- **Motor / Sensor / Gun Monitoring**: séries temporais massivas + controles de parâmetros + “disparador” de calibração automática de limites
- **Sealer Management**: monitoramento específico do processo de selagem

---

## 2) Arquitetura da Aplicação

Stack organizada  em **serviços dockerizados** e separados por:
- **webapp** (Dash/Flask): Framework com base em Python, desenvolvido para diversas aplicações Web com autenticação, páginas rotas, elementos gráficos etc.
- **node-red**: Caso seja    necessário, uma instância Node-Red rodará em conteiner separado, fazendo parte da stack principal
- **event-gateway**: Recurso para o uso da aplicação em requisições websockets, MQTT, API's, onde lógica orientada a eventos são necessárias


Requisitos Atendidos:
- Aplicação **local/on-premises** (dockers)
- Possui **login/registro** e **controle de nível** 
- **Gráficos + KPIs + tabela de mensagens** disponíveis


Customização ao Cliente:
- **MySQL:** Schema + migrations + conexão (env) + CRUD/configs + ajustes de consultas
- **InfluxDB**: Buckets/retention + client + queries (downsampling/agg) + performance para séries grandes

> Performance em séries com milhões de pontos ou mais dependerá dos recursos do servidor, load balance e controle para requisições, e não apenas da infraestrutura de software:  
Exemplo: Uma requisição dos últimos 10 anos, de um tipo de dado que é coletado a cada  100 ms poderá sobrecarregar recursos de hardware e não deveriam ser possíveis pela UI (User Interface)

---

## 3) Tabela de Recursos da Aplicação


| Recurso  | Onde está | O que faz hoje | Observações de aderência à solicitação do cliente |
|---|---|---|---|
| Docker Compose / stack multi-serviço | EasyTek-Infra | Orquestra webapp + node-red + event-gateway (+InfluxDB/portainer em dev/local) | Cliente quer rodar local; docker facilita implantação e padroniza ambientes |
| Webapp Dash/Flask | EasyTek-Data | Portal web com páginas, layout, autenticação, dashboards | Interface responsiva, navegação e multiusuário são requisitos |
| Login / Registro / Sessão | webapp | Login/registro, sessão por usuário (flask_login) | Atende controle de acesso e multiusuário |
| Níveis de usuário / páginas restritas | webapp | Ex.: páginas selecionadas restriras a um nível específico de usuários | Mapeia com “admin vs visualizador”, além de ser possível mapear por departamentos |
| Dashboard principal com KPIs | webapp | KPIs (ex.: OEE e afins), cards e indicadores | Cliente pede central de métricas — mas as métricas específicas do LMS ainda precisam ser modeladas |
| Gráficos interativos Plotly | webapp | Gráficos de séries temporais (ex.: energia/temperatura) | Após custumização da aplicação para funcionamento junto ao banco InfluxDB Existente, teremos gráficos interativos para as séries temporais selecionadas. |
| Exportação para Excel | webapp | Botões de export (ex.: temperatura) | Não é pedido explícito, mas agrega valor para manutenção/gestão |
| Tabela de mensagens/alertas | webapp | Lista/visão de eventos/estados | A proposta fala de anomalias e status/alertas; o formato/taxonomia ainda precisa alinhar |
| Página “States” (estados/alertas) | webapp | Visualização e filtros | Similar ao esperado (monitoramento e anomalias) |
| Setpoints | webapp | Inputs de setpoint e/ou comandos remotos | Analisar aderencia: Alterar o banco, alterar equipamento ou alterar os dois? |
| Event-gateway (API de comando) | event-gateway | Endpoint HTTP que publica mensagens em MQTT | Suporta “controles de administrador” (reiniciar VM, desligar sistema etc.). Precisa definir comandos reais do LMS |
| Node-RED instalado e configurado | node-red | Motor de integração, aquisição e automação | Cliente pede “preferencialmente nativo ao Node-RED”, porém para as espectativas mencionadas, apenas node-red pode não ser suficiente quanto customização. Um serviço node-red será disponibilizado para que se, cliente precisar de algum recurso do node-red dentro da aplicação, ja esteja mais fácil. |
| Nós industriais (Modbus) | node-red | Leitura/escrita Modbus | Útil para sensores/medidores |
| Nós industriais (CIP/EtherNet-IP, PCCC etc.) | node-red | Integração com PLCs/ambientes industriais | Direto no escopo de PLC monitoring (dependendo do LMS/arquitetura real) |
| Integração MQTT | node-red + event-gateway | Pub/sub de eventos e comandos | Bom para comandos e telemetria |
| MySQL| webapp | A ser customizado para o cliente | **Parcial**: suporte existe, mas precisa ser aplicado ao modelo do LMS |
| InfluxDB | webapp | A ser customizado para o cliente | **Parcial** (é requisito explícito) |
| Gerenciador de Stacks (Portainer) | dev/local | UI de administração de containers | Não é requisito do Cliente; pode ser “extra” opcional |
| Robustez/segurança (TLS, hardening, auditoria) | transversal | Hoje é o básico do app + docker | **Parcial**: o Cliente enfatiza robustez/segurança; geralmente exige endurecimento, logs, auditoria, backup, etc. |
| Observabilidade (logs padronizados, healthchecks, métricas) | transversal | Não é requisito explícito | Recomendado para operações Industriais |

---

## 4) Estimativa de horas de Desenvolvimento (com subtotais) — **recalculado**

Abaixo, uma estimativa de carga horária com base nas informações. A carga horária pode variar de acordo com alterações e confirmações do escopo.

### 4.1 Serviços principais (arquitetura)
| Serviço / Módulo | Entregáveis do “do zero” | Horas (estimativa) |
|---|---|---:|
| Infra Docker (compose + envs + volumes + redes) | stack dev/local/prod, padronização, scripts de deploy, documentação | 24–40 |
| Webapp (portal + layout base) | estrutura Dash/Flask, roteamento, navbar/sidebar, páginas base | 90 |
| Autenticação + RBAC (admin/usuário) | login, registro, hash senha, níveis, proteção por rota/página | 30–50 |
| Dashboards (framework) | componentes reutilizáveis (cards, tabelas, filtros, selects, datas) | 60–120 |
| Node-RED (instalação + setup) | container, persistência, credenciais, pacotes/nós, organização por flows | 10–20 |
| Event-gateway (API comandos) | API REST, validação, autenticação, publish MQTT, logs | 24–40 |
| **Subtotal (Arquitetura)** |  | **238–360** |

### 4.2 Camada de dados (o ponto crítico do PDF)
| Serviço / Módulo | Entregáveis do “do zero” | Horas (estimativa) |
|---|---|---:|
| MySQL (configurações/parâmetros) | schema, tabelas, migrações, CRUD básico, permissões | 30–60 |
| InfluxDB (séries temporais) | buckets/retention, modelagem de tags/fields, políticas, testes de escrita/leitura | 40–80 |
| Conectores de leitura (Influx → Webapp) | queries, downsampling/aggregate, paginação, performance | 50–120 |
| Conectores de escrita (Node-RED → Influx/MySQL) | flows, retry, buffer, tratamento de falhas, validação | 60–140 |
| **Subtotal (Dados)** |  | **180–400** |

### 4.3 Funcionalidades do escopo do LMS (dashboards do PDF)
| Dashboard / Feature (do PDF) | O que precisa existir | Horas (estimativa) |
|---|---|---:|
| Página Inicial (central de métricas) | definir KPIs do LMS, consultas, cards, alertas, filtros | 120 |
| Versão Mobile | Implementação de Responsividade também para dispositivos móveis | 160 |
| LMS Monitor (VMs) | integração com fonte de status das VMs (API/DB/log), telas e alertas | 65–120 |
| PLC Monitoring | fonte de status PLC + shop/equipamento + alarmes + telas | 60–160 |
| Motor/Sensor/Gun Monitoring (milhões de pontos) | séries temporais, filtros avançados, performance, agregações, UX | 80–220 |
| Controle de parâmetros + “calibração automática” | UI admin, validações, trilha de auditoria, comando/execução | 80–200 |
| Sealer Management | modelagem das variáveis + telas específicas + séries temporais | 60–160 |
| **Subtotal (Dashboards LMS)** |  | **625–1.140** |

### 4.4 Robustez, segurança e operação (normalmente obrigatório em indústria)
| Item | Entregáveis | Horas (estimativa) |
|---|---|---:|
| Hardening e segurança (TLS, secrets, policies) | TLS interno/externo, secrets, validação, rate-limit (se aplicável), revisão | 30–80 |
| Logs e auditoria (ações admin) | logs estruturados, trilha de ações, correlação, export | 20–60 |
| Healthchecks + monitoramento | endpoints, checks de containers, alertas básicos | 16–40 |
| Backup/restore (MySQL/Influx) | política, scripts e testes | 16–40 |
| **Subtotal (Operação/Security)** |  | **82–220** |

### 4.5 Tabela de totais (soma dos subtotais)
| Grupo | Subtotal (horas) |
|---|---:|
| Arquitetura (4.1) | 238–360 |
| Dados (4.2) | 180–400 |
| Dashboards LMS (4.3) | 625–1.140 |
| Operação/Security (4.4) | 82–220 |
| **TOTAL GERAL (soma dos subtotais)** | **1.125–2.120** |


> Observações: A duração do projeto pode alterar com base no investimento em UX (Experiência do Usuário). Uma extratégia de desenvolvimento de um MVP, é recomendada. 




