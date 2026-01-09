# Procedimento de Comunicação entre IHM e PLC (Rockwell)  
**Plataforma:** FactoryTalk View ME (PanelView / PC Runtime)  
**Foco:** Entender e configurar **Shortcut**, **caminho de comunicação**, **Design x Runtime**, **Create Runtime**, **Transfer Utility** e a opção **Replace Communications**.  
**Formato:** Treinamento técnico (base para aulas futuras)

---

## 0) Objetivo do treinamento  
Padronizar o entendimento e a configuração da comunicação entre **IHM (FactoryTalk View ME)** e **PLCs Rockwell (Logix/Micro/Compact/Control)**, evitando os erros clássicos de:  

- “No meu computador funciona, na IHM não”  
- Runtime (.MER) com comunicação diferente do projeto  
- **Atualização de IHM em campo com decisão errada sobre sobrescrever comunicação (quando sobrescrever e quando preservar):**  
  - **Sobrescrever (Replace Communications = ON)** quando você **quer aplicar** no PanelView a comunicação que está “dentro do .MER” (ex.: comissionamento de IHM nova, troca de hardware/CPU, mudança planejada de IP/rota/topologia, correção deliberada de shortcuts, padronização intencional).  
  - **Preservar (Replace Communications = OFF)** quando a comunicação **já está correta na máquina** e você está apenas **atualizando telas/recursos**, ou quando o seu ambiente de desenvolvimento (Design) usa **rede/PLC diferente** e você não quer correr o risco de “levar” esse caminho para a produção.  


---

## 1) Conceitos básicos: o que é o *Shortcut* (a analogia do porteiro e dos prédios)  

### 1.1 A analogia completa  
Pense assim:

- **PLC = um prédio**
- **Programa/Rotina/Tags = apartamentos e moradores do prédio**
- **IHM = visitantes (público, entregadores, prestadores)**
- **Shortcut = o porteiro do prédio**
- **Caminho (driver/IP/rota CIP) = o endereço e o caminho até a portaria**
- **Tags internas (HMI Tags) = fichas de cadastro / lista interna que a recepção usa para não ter que “decorar” tudo**

A IHM (visitantes) **não entra no prédio sozinha**. Ela fala com o **porteiro (Shortcut)**:  
> “Quero falar com o morador ‘Motor_Run’ no apartamento X.”  
O porteiro olha o nome (Shortcut), sabe **qual prédio** é e **para onde mandar** a mensagem (caminho).  

### 1.2 “Se eu tenho outro prédio, preciso de outro porteiro?”  
**Sim.** Se você tem **um segundo PLC**, isso é **um segundo prédio**.  
Você precisa de **outro Shortcut (outro porteiro)** para esse segundo prédio.

Exemplos práticos:  

- **1 PLC** → 1 prédio → **1 shortcut** (ex.: `CLX_TesteLCT16`)  
- **2 PLCs** → 2 prédios → **2 shortcuts** (ex.: `CLX_TesteLCT16` e `CLX_TesteLCT08`)  
- **Mesmas pessoas (mesmos nomes de tags)** podem existir em prédios diferentes, mas o **porteiro precisa saber qual prédio é qual**.  

> Se você tentar usar “um porteiro só” para dois prédios, você corre o risco de “entregar a encomenda no prédio errado”.

### 1.3 O que o Shortcut realmente “guarda”  
O Shortcut **não é a tag**. O Shortcut é a **ponte** (mapeamento) entre:  

- o **nome simbólico** que a aplicação usa (`CLX_TesteLCT16`)  
- e o **caminho real** até o controlador (driver + rota + CPU)  

Ele é o “nome do prédio” para o mundo da IHM.  

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_01.png)


---

## 2) Caminho da comunicação: como o dado sai do objeto e chega no PLC  

### 2.1 Caminho direto (objeto aponta para tag do PLC via shortcut)  
**Fluxo:**  

- **Objeto na Tela** → **Shortcut** → **CPU/Controlador** → **Programa/Tags**

Exemplo mental:  

- Objeto: “Lâmpada do Motor”  
- Tag: `{::[CLX_TesteLCT16]Program:Main.Motor_Run}`    
O objeto pergunta ao porteiro `CLX_TesteLCT16` e ele encaminha para a CPU certa.  

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_02.png)

### 2.2 Caminho com Gerenciador de Tags Internas (HMI Tags como “cadastro interno”)  
Às vezes você não quer que o objeto “conheça” diretamente a tag do PLC.  
Você cria uma **Tag Interna** (HMI Tag) que referencia (ou representa) a tag real.

**Fluxo:**

- **Objeto na Tela** → **Gerenciador de Tags Internas (HMI Tags)** → **Shortcut** → **CPU** → **Programa/Tags**

Por que isso existe (na prática):  

- Padronização: telas usam `HMI.MotorRun` e você troca a origem por trás  
- Reaproveitamento: telas iguais para máquinas diferentes  
- Facilidade de manutenção: altera o mapeamento sem caçar 200 objetos  

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_14.png)

> **Observação importante:** independente de você usar o caminho **direto para o controlador** (Objeto → Shortcut → CPU → Tag) ou via **banco de tags interno da IHM** (Objeto → Tags Internas → Shortcut → CPU → Tag), em algum ponto **sempre será obrigatório passar pelo Shortcut** *(o “porteiro”)* para chegar ao PLC correto.

---

## 3) Onde configurar o Shortcut (o “porteiro”) no FactoryTalk View ME  

### 3.1 Caminho no projeto  
No FTView Studio ME (com FactoryTalk Linx):  

- Abra **Communication Setup**  
- Você verá duas abas principais:
    - **Design (Local)**: seu ambiente de desenvolvimento (seu PC)  
    - **Runtime (Target)**: o ambiente de execução (PanelView / máquina)  

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_12.png)

### 3.1.1 Import / Export Configuration (aba Runtime – Target)
Na aba **Runtime (Target)** existe a opção **Import and Export Configuration**, que serve para **exportar** a configuração de comunicação (drivers, dispositivos, rotas e associações) para um arquivo e depois **importar** novamente quando necessário.

**Por que isso é útil (na prática):**  

- **Backup rápido da comunicação**: antes de alterar o Runtime (Target), você exporta e guarda um “snapshot” da configuração.  
- **Reaplicar comunicação em outro terminal**: quando você tem vários PanelViews parecidos, pode exportar de um padrão e importar em outro (ajustando apenas IP/slot quando necessário).  
- **Rollback de comunicação**: se uma mudança de comunicação deu errado, importar o arquivo anterior pode ser mais rápido do que reconfigurar tudo manualmente.  
- **Separar “conteúdo” de “infra”**: você pode atualizar telas/funcionalidades e manter um controle paralelo da configuração de comunicação.  

**Como ele se relaciona com Copy from Design to Runtime:**  

- **Copy from Design to Runtime** replica a topologia do **Design (Local)** para o **Runtime (Target)** quando os ambientes são equivalentes.  
- **Import/Export** é uma abordagem alternativa/ complementar para **preservar, transportar ou restaurar** a configuração do **Runtime (Target)** sem depender do Design (Local) — especialmente útil em cenário de **atualização em campo**.  

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_13.png)

### 3.2 Criando / associando um Shortcut (passo a passo)  
1) **Add** (criar shortcut)  
![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_03.png)  
2) Nomear o shortcut (padrão da planta, ex.: `CLX_LCT08`)  
3) Navegar na árvore de rede/driver até a **CPU**  
4) Selecionar a CPU e clicar **Apply** (associar o shortcut ao dispositivo)  
![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_04.png)
![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_04.png)
![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_05.png)

> **Dica:** depois de associar o shortcut com **Apply** e validar com **Verify**, finalize com **OK** para gravar a configuração. Se sair sem OK (ou cancelar), você pode perder o que configurou.





> **Disclaimer (máquina virtual / descoberta de rede):** quando estiver usando **VM**, é comum que a busca automática de dispositivos **não encontre** a CPU (dependendo de NAT/Bridge, firewall, driver de rede virtual, rota CIP, permissões, etc.).  
> Se você **não enxergar** o controlador na árvore do **Communication Setup**, isso **não significa** necessariamente que “não existe comunicação”: pode ser limitação de descoberta.  
> **Solução prática:** adicione o dispositivo **manualmente** no Runtime/Design (Add Device / informar IP / slot / caminho) e depois associe ao Shortcut com **Apply** e valide com **Verify**.  
>
> **Testes rápidos para separar “descoberta falhou” de “não tem comunicação”:**  

> - **Ping** no IP do PLC (e no gateway/switch da rede): se não responde, pode ser rota/IP/rede/firewall.   
> - Verificar se o IP do seu PC/VM está na **mesma sub-rede** (ou se existe **rota** entre as redes).  
> - Confirmar modo de rede da VM (**Bridge x NAT**) e se a placa correta está selecionada.  
> - Checar **LEDs/link** (porta/switch) e se há tráfego na rede (quando aplicável).  
> - Tentar acessar o PLC por outra ferramenta (ex.: **RSLinx Classic / Studio 5000** ou quem já estiver funcionando no seu ambiente): se conecta lá, mas não aparece no browse do FT, é forte indício de **problema de descoberta** e não de rede.  
> - Se o dispositivo aparece em outras ferramentas mas não no FT, tente **Add Device manual** (IP/slot) e siga com **Apply/Verify**.  

---

## 4) Design (Local) vs Runtime (Target): por que existem dois “mundos”

### 4.1 O motivo real

A Rockwell separa o mundo de **desenvolvimento** do mundo de **produção** porque:
- No **PC** você pode testar com uma topologia diferente (rede de bancada, IPs temporários, emulador)
- Na **máquina** a topologia pode ser outra (rede real, VLAN, switch industrial, rota CIP diferente)
- Em **atualizações**, a comunicação pode já estar pronta e não deve ser “quebrada” sem querer

Ou seja: **o prédio de testes pode ser diferente do prédio real**, e você precisa ter controle disso.

### 4.2 Regra de ouro

- **Design (Local)**: “onde eu testo e desenvolvo”
- **Runtime (Target)**: “o que será empacotado no .MER e executado na IHM”

---

## 5) Copy from Design to Runtime: o que é, por que existe, e quando usar (ou NÃO usar)

### 5.1 O que ele faz (sem mistério)
O botão **Copy from Design to Runtime** copia:

- drivers
- topologia visível
- caminhos de dispositivos  
da aba **Design (Local)** para a aba **Runtime (Target)**.

Tradução: ele diz “porteiro do prédio real, copie a lista e os endereços do porteiro do prédio de testes”.

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_06.png)

### 5.2 Por que ele existe (a razão “boa”, não só o erro)
Ele existe para acelerar o trabalho quando:

- seu ambiente de desenvolvimento **é igual** ao ambiente real  
ou  
- você quer **partir de uma base** e depois ajustar o runtime

Exemplo comum:

- Você montou a comunicação no seu PC (Design) e quer rapidamente “replicar” para o Runtime sem reconfigurar tudo manualmente.

### 5.3 Quando usar
Use o **Copy from Design to Runtime** quando:

- está criando uma aplicação nova do zero
- ou a topologia de rede e CPU do **Design** são iguais às do **Runtime**
- ou você quer “copiar e depois limpar/ajustar” (com cuidado)

### 5.4 Quando NÃO usar (caso mais comum em fábrica: atualização)
**Cenário típico:**

- A máquina já está em produção
- A comunicação no PanelView já está funcionando (já existe um “porteiro” correto lá)
- Você quer atualizar telas/funcionalidades sem destruir a comunicação configurada em campo

Nesse caso:

- Você pode usar o Design (Local) para testar com sua rede / seu PLC / seu ambiente de bancada
- E **não copiar isso para Runtime**, para evitar que o .MER “leve junto” uma comunicação diferente

> **Em atualização (5.4): o que pode acontecer se você usar Copy from Design to Runtime / Replace Communications sem critério**

> - Você “só queria adicionar uma chave/tela”, mas acaba **sobrescrevendo toda a topologia de comunicação** do PanelView com a topologia do seu PC/lab.
> - A IHM passa a procurar o PLC em **IP/slot/rota CIP diferentes** (ex.: seu ambiente de testes) e “some” a comunicação na máquina.
> - Projetos com **comunicação complexa** (vários PLCs/shortcuts, rotas via ENBT/EN2T, chassis/remotos, NAT, múltiplas sub-redes, caminhos por switch/estratégia da planta) podem ser **simplificados/alterados involuntariamente**, quebrando apenas parte do sistema (o pior tipo de falha).
> - Você leva para o runtime **drivers/dispositivos extras** do Design que não existem na planta, poluindo a configuração e confundindo diagnóstico.
> - Você cria um .MER com Runtime “zerado” ou incompleto (porque esqueceu de ajustar a aba Runtime) e ao transferir com **Replace Communications**, o PanelView fica com shortcuts **sem associação válida**.
> - Resultado típico: **telas abrem**, mas tags ficam **“???” / estrelas / timeouts**, botões não atuam, alarmes não atualizam — e a equipe jura que “antes estava funcionando”.
> - Em casos mais críticos, você derruba comunicação de **mais de um controlador** (porque “um copy” copiou todos os dispositivos/atalhos), e o impacto deixa de ser “uma tecla não funcionou” e vira **parada de linha**.

### 5.5 O que pode dar errado se você desconhecer isso

- Você testa no PC (Design) e tudo funciona
- Mas o Runtime (Target) não está configurado (ou está diferente)
- Você cria o .MER e baixa para o PanelView
- Resultado: **no computador comunica, na IHM não**

Causas clássicas:

- Runtime sem o shortcut associado à CPU correta
- Runtime apontando para IP/rota diferente
- Você “levou” a comunicação do seu PC para a máquina sem querer (sobrescreveu algo existente)
- Você não aplicou/confirmou as mudanças no setup de comunicação

---

## 6) Create Runtime: o que é, o que ele gera e por que isso importa

### 6.1 O que significa “Create Runtime”
O **Create Runtime** é o processo que:

- compila/empacota o projeto
- gera um arquivo **.MER** (Machine Edition Runtime)
- incluindo as configurações necessárias para rodar na IHM (telas, alarmes, parâmetros e — principalmente — o que estiver definido para o **Runtime (Target)**)

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_07.png)
![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_08.png)

### 6.2 Arquivos: edição vs execução (por que isso existe)
- **Projeto editável (Design):** o que você abre no FTView Studio (estrutura do projeto)
- **Runtime (.MER):** pacote “fechado” para execução no PanelView

O runtime existe porque:

- a IHM precisa de algo otimizado para rodar
- e porque nem sempre você quer (ou pode) rodar com um projeto editável em campo

### 6.3 A relação direta com comunicação
O .MER “leva junto” a comunicação do **Runtime (Target)**.  
Logo:

- Se o Runtime (Target) está errado → o .MER vai com comunicação errada  
- Se o Runtime (Target) está vazio → o .MER pode rodar “cego” (sem achar PLC)

### 6.4 Boas práticas — sempre gerar um .MER novo (estratégia de rollback)

- **Opte sempre por criar um arquivo novo de runtime (.MER)** a cada alteração relevante, em vez de sobrescrever o arquivo anterior.
- Nomeie com padrão de versão/data (ex.: `Linha08_Rev005_2025-12-20.mer`) para rastreabilidade.
- **Por quê:** se algo der errado em campo (principalmente comunicação), seu **plano de retorno** fica simples: você volta para o **.MER anterior** e normalmente a IHM retorna ao estado de comunicação que estava funcionando.
- Isso reduz tempo de parada e evita “caça às bruxas” tentando adivinhar qual mudança quebrou a comunicação.
- **Prática recomendada:** mantenha também uma cópia do último .MER “bom” em um local padrão (ex.: pasta de backups da engenharia e/ou pendrive técnico), para rollback rápido.

---

## 7) Transfer Utility: download do .MER e o papel do “Replace Communications”

### 7.1 O que o Transfer Utility faz
Ele transfere o arquivo .MER para a IHM (PanelView), e pode configurar comportamento de inicialização:

- rodar a aplicação ao ligar
- substituir comunicação
- limpar logs, etc.

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_09.png)
![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_10.png)

### 7.2 “Replace Communications”: o que significa na analogia
**Replace Communications** = “substituir o porteiro e as rotas do prédio pelo que veio no pacote (.MER)”

Ou seja, se marcado:

- a IHM pega as configurações de comunicação que estão dentro do .MER
- e **aplica por cima** do que já existia na IHM

Se desmarcado:

- a IHM mantém a comunicação que já estava configurada
- e você atualiza “só o conteúdo” (telas/lógica da IHM)

![Transfer Utility — visão geral](../../../Assets/Images/Comunicação%20IHM%20Rockwell/IHM_11.png)

### 7.3 Quando marcar (criação do zero / comissionamento)
Marque **Replace Communications** quando:

- você está colocando a IHM nova em operação
- ou você quer garantir que a comunicação definida no runtime do projeto seja aplicada na IHM
- ou você alterou intencionalmente shortcuts/rotas e quer que a IHM receba isso

### 7.4 Quando NÃO marcar (atualização sem destruir comunicação existente)
Não marque **Replace Communications** quando:

- você está atualizando uma IHM existente em campo
- e a comunicação atual já está correta e você não quer arriscar sobrescrever
- você testou no seu PC com rede diferente (Design), mas quer preservar a rede real

> **Atenção:** preservar comunicação (**Replace Communications = OFF**) só funciona bem quando o terminal **já possui** os shortcuts necessários **com os mesmos nomes** usados no projeto.  

> Se você **criar/renomear shortcuts** no projeto e baixar com Replace OFF, o terminal pode continuar com a comunicação antiga e os novos atalhos podem **não existir**, resultando em falha de comunicação para os objetos que dependem deles.


**Esse é o “pulo do gato”** em atualizações:  
Você consegue modernizar telas e recursos sem “derrubar o prédio” (comunicação) que já estava funcionando.

### 7.5 O erro clássico “no PC comunica, na IHM não” e a ligação com Replace Communications
Se você:

- gerou .MER com Runtime diferente do necessário  
e ainda por cima:
- marcou **Replace Communications**  
então você pode “apagar” a comunicação funcional da máquina e aplicar uma comunicação errada.

A frase “no PC comunica, na IHM não” muitas vezes é:

- Runtime sem caminho correto + Replace Communications aplicado
- ou Runtime não atualizado (faltou Copy/ajuste) + IHM ficou com a comunicação antiga e incompatível

---

## 8) Checklists práticos (para treinamento e para campo)

## 8.1) Checklist ANTES de criar o .MER — perguntas de decisão (guia prático)

> **Objetivo deste 8.1:** antes de clicar em *Create Runtime*, responder perguntas que definem **o que vai no Runtime (Target)** e se você deve ou não **substituir a comunicação** no PanelView.

### A) Qual é o tipo de trabalho?
- **Estou criando uma IHM NOVA do zero (comissionamento / IHM nova / terminal novo)?**
  - ✅ Ação recomendada:
    - Configure **Design (Local)** e **Runtime (Target)** de forma consistente para a planta.
    - Use *Copy from Design to Runtime* **somente** se o Design refletir exatamente a topologia final.
    - Planeje baixar com **Replace Communications = ON**.
- **Estou ALTERANDO uma IHM existente em produção (update/patch)?**
  - ✅ Ação recomendada:
    - Trate a comunicação da máquina como “infra crítica”.
    - Evite *Copy from Design to Runtime* se o seu Design estiver em bancada/VM/lab.
    - Em geral, planeje baixar com **Replace Communications = OFF** (preservar comunicação existente), a menos que você tenha motivo explícito para mudar a comunicação.

### B) Onde essa alteração foi testada?
- **Testei na própria topologia da máquina (rede real / mesmo PLC / mesmo caminho)?**
  - ✅ Você pode alinhar Runtime com mais segurança.
- **Testei em bancada/laboratório/VM (rede diferente / PLC diferente / IP diferente)?**
  - ⚠️ Risco alto de “levar” comunicação errada para o runtime.
  - ✅ Ação recomendada:
    - Mantenha o Runtime (Target) apontando para a planta (ou nem altere o Runtime).
    - Prefira **Replace Communications = OFF** na atualização.
    - Se precisar alterar comunicação, faça isso de forma controlada e documentada.

### C) Eu PRECISO mudar comunicação na máquina?
- **A comunicação atual da máquina já atende e eu só estou mudando telas/teclas/funcionalidade?**
  - ✅ Não mude comunicação.
  - ✅ Runtime pode ficar como está (ou espelhar o que já existe).
  - ✅ Transfer: **Replace Communications = OFF**.
- **Estou incluindo algo novo que EXIGE mudança de comunicação? (ex.: novo PLC, nova CPU, novo caminho)**
  - ✅ Você provavelmente precisa atualizar comunicação.
  - Perguntas de confirmação:
    - Vou adicionar **um novo prédio (novo PLC)**? → então preciso de **novo shortcut (novo porteiro)**.
    - Vou mudar IP/slot/rota CIP do PLC existente? → então preciso atualizar o caminho do shortcut.
  - ✅ Transfer: **Replace Communications = ON** (apenas quando você quer aplicar essas mudanças no terminal).

### D) O Runtime (Target) está coerente com o cenário real?
- **Eu consigo apontar o Runtime (Target) para as CPUs reais da planta (IP/slot/rota corretos)?**
  - ✅ Ótimo: o .MER sairá coerente para produção.
- **Eu NÃO consigo validar Runtime porque estou fora da rede (VPN bloqueada, VM, sem rota)?**
  - ✅ Ação recomendada:
    - Não “chute” caminhos no Runtime.
    - Prefira **não alterar comunicação** e manter **Replace OFF**.
    - Se for obrigatório mudar comunicação, faça isso presencialmente ou com acesso validado e documente.
- **Mesmo assim, eu ainda estou com dúvida do impacto (principalmente em comunicação complexa)?**
  - ✅ Ações seguras (na ordem de preferência):
    - **Simular/validar em bancada** (ambiente controlado) para prever o efeito de *Copy/Replace* e entender o que o terminal vai sobrescrever.
      - Se no teste de bancada “quebra” ou muda algo inesperado, **assuma que pode quebrar na máquina** e revise o plano.
    - **Chamar alguém mais experiente** naquele padrão de comunicação (rota CIP, múltiplos PLCs/shortcuts, NAT, chassi/remotos) para revisar antes de aplicar em produção.
    - **Validar direto na máquina (situação real)** apenas quando você tiver janela, backup e plano de retorno — e com o mínimo de alterações por vez.


### E) Se eu aplicar Replace Communications, o que eu estou prestes a sobrescrever?
- **A máquina tem comunicação simples (1 PLC, 1 shortcut, rota direta)?**
  - ✅ Risco menor, mas ainda assim confirme Runtime.
- **A máquina tem comunicação complexa (vários PLCs/shortcuts, rotas, redes, NAT, chassis, remotos)?**
  - ⚠️ Risco alto de derrubar parte do sistema.
  - ✅ Ação recomendada:
    - Só use Replace ON com plano claro (backup + validação).
    - Garanta que o Runtime inclui **apenas** os dispositivos necessários (evite copiar “tudo” do Design).
    - Documente exatamente o que mudou.

---

### Checklist completo (marcar antes do Create Runtime)
- [ ] **Decisão:** isso é **IHM nova** ou **atualização**?
- [ ] **Ambiente de teste:** testei em **rede real** ou **bancada/VM**?
- [ ] **Mudança de comunicação é necessária?** (sim/não) — por quê?
- [ ] Existe **novo PLC**? Se sim, já defini **novo shortcut** (novo porteiro) e nome padrão.
- [ ] Existe mudança de **IP/slot/rota CIP**? Se sim, está refletida no **Runtime (Target)**.
- [ ] **Design (Local)** está configurado apenas para testes ou representa a planta?
- [ ] **Runtime (Target)** está configurado para a planta (ou conscientemente preservado).
- [ ] Eu sei qual será minha decisão no download:
  - [ ] **Replace Communications = ON** (vou aplicar a comunicação do .MER no terminal)
  - [ ] **Replace Communications = OFF** (vou preservar a comunicação existente)
- [ ] Tenho **backup do .MER atual** do terminal (e/ou arquivo fonte do projeto).
- [ ] Tenho um **plano de retorno** (rollback) se a comunicação falhar.
- [ ] Tenho um teste mínimo pós-download definido (uma tela com leitura + uma escrita crítica controlada).

