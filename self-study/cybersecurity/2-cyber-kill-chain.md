# Cyber Kill Chain — Anotações de estudo para SOC e Blue Team

Material organizado a partir do artigo da CyberDefenders fornecido.
## 1. O que é a Cyber Kill Chain?

A Cyber Kill Chain é um modelo que divide um ataque cibernético direcionado em sete estágios, desde o reconhecimento inicial do alvo até a execução do objetivo do invasor.

O modelo foi apresentado em 2011 por Eric Hutchins, Michael Cloppert e Rohan Amin, da Lockheed Martin, no artigo Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains.

A ideia central é simples:

> O invasor precisa avançar por uma sequência de etapas para atingir seu objetivo. O defensor precisa identificar e interromper pelo menos uma delas.

O modelo ajuda o analista a responder três perguntas:

- Em qual estágio do ataque estamos?
    
- O que o invasor pode ter realizado antes desse estágio?
    
- Em qual ponto podemos interromper a sequência?
    

A Cyber Kill Chain não é uma ferramenta que você instala. É um modelo mental para organizar investigações, planejar defesas e entender a progressão de um ataque.

### 1.1. Por que ela é importante para o SOC?

Imagine que o SIEM gere um alerta de phishing.

O analista pode usar a Kill Chain para identificar que a atividade está relacionada ao estágio de entrega. Se o e-mail foi bloqueado antes de o usuário abrir o anexo, talvez o ataque tenha sido interrompido precocemente.

Agora imagine que o alerta indique comunicação recorrente entre um endpoint e um servidor externo suspeito. Isso pode indicar atividade de comando e controle, sugerindo que o invasor talvez já tenha executado código no ambiente.

A segunda situação exige uma investigação mais ampla, pois pode haver comprometimento anterior ao alerta.

Atenção: o estágio ajuda a orientar a investigação, mas não determina sozinho a gravidade. Um único evento de alto impacto pode ser crítico mesmo quando detectado cedo.

# 2. Os sete estágios da Cyber Kill Chain

O modelo original possui sete estágios sequenciais.

## 01

Reconhecimento

Reconnaissance

O invasor pesquisa o alvo e coleta informações.

## 02

Armamento

Weaponization

Prepara a ferramenta, o arquivo ou o mecanismo de ataque.

## 03

Entrega

Delivery

Transmite o conteúdo ou mecanismo até o alvo.

## 04

Exploração

Exploitation

Explora uma vulnerabilidade ou induz uma ação que permite executar código.

## 05

Instalação

Installation

Estabelece um ponto de apoio ou persistência, quando necessário.

## 06

Comando e controle

Command and Control (C2)

Estabelece comunicação para controlar ou operar o sistema comprometido.

## 07

Ações sobre os objetivos

Actions on Objectives

Executa a finalidade do ataque, como roubo de dados ou ransomware.

O artigo utiliza como exemplo um ataque de spear-phishing contra um funcionário do setor financeiro. Vamos acompanhar essa situação ao longo dos sete estágios.

## 2.1. Estágio 1 — Reconhecimento (Reconnaissance)

Objetivo: coletar informações sobre o alvo antes de iniciar o ataque.

O invasor busca entender a organização, identificar pessoas relevantes e descobrir possíveis pontos de entrada.

### Reconhecimento passivo

Consiste em obter informações sem interagir diretamente com os sistemas-alvo de maneira perceptível.

Exemplos citados no artigo:

- Perfis de funcionários em redes sociais.
    
- Formatos de endereços de e-mail corporativos.
    
- Publicações sobre tecnologias utilizadas pela organização.
    
- Registros DNS.
    
- Informações disponíveis publicamente sobre possíveis vazamentos.
    

### Reconhecimento ativo

Envolve interações diretas com a infraestrutura do alvo, como verificações de portas e serviços expostos.

Esse tipo de atividade pode deixar registros em firewalls, sistemas de detecção de intrusão, aplicações e serviços de rede.

### Exemplo prático

O invasor descobre que:

- Um gerente financeiro publica informações profissionais nas redes sociais.
    
- A empresa utiliza um padrão previsível de endereços de e-mail.
    
- Existe uma aplicação acessível pela internet.
    

Essas informações podem ajudar a preparar um ataque direcionado.

### O que o analista SOC deve observar?

- Varreduras incomuns contra serviços expostos.
    
- Picos de requisições em aplicações web.
    
- Consultas e padrões de DNS suspeitos.
    
- Acesso repetitivo a endpoints públicos.
    
- Tentativas de enumeração de recursos.
    

Nem todo reconhecimento será detectável internamente. Parte da coleta pode ocorrer em fontes públicas, sem gerar eventos na infraestrutura da vítima.

## 2.2. Estágio 2 — Armamento (Weaponization)

Objetivo: preparar o mecanismo que será utilizado para comprometer o alvo.

Nesse estágio, o invasor combina um método de entrega com um arquivo, código ou mecanismo de exploração.

O artigo cita exemplos como:

- Planilha maliciosa.
    
- PDF manipulado.
    
- Carregador de ransomware escondido em um instalador aparentemente legítimo.
    

### Exemplo prático

O invasor prepara uma planilha que contém um mecanismo malicioso, planejado para executar código quando determinadas condições forem atendidas.

O arquivo é preparado antes de ser enviado à vítima.

### Por que esse estágio é difícil de observar?

A preparação pode ocorrer inteiramente na infraestrutura do invasor, fora da rede da organização.

Por isso, a equipe defensiva pode não ter visibilidade direta sobre o armamento.

Ainda assim, informações de inteligência de ameaças, hashes de arquivos conhecidos e indicadores associados a campanhas podem ajudar a reconhecer ferramentas e artefatos relacionados.

### O que o analista SOC deve compreender?

Nem toda etapa do ataque terá evidências disponíveis no ambiente monitorado.

O analista precisa distinguir entre:

- Comportamentos efetivamente observados.
    
- Informações obtidas por inteligência de ameaças.
    
- Hipóteses sobre o que pode ter ocorrido fora da infraestrutura.
    

## 2.3. Estágio 3 — Entrega (Delivery)

Objetivo: transmitir o arquivo, link ou mecanismo de ataque até o alvo.

O artigo cita os seguintes vetores:

- E-mail.
    
- Links maliciosos.
    
- Sites comprometidos que atraem visitantes (watering hole).
    
- Dispositivos USB.
    
- Cadeias de suprimentos comprometidas.
    

### Exemplo prático

O invasor envia ao gerente financeiro uma planilha com o nome de uma suposta fatura.

A mensagem foi preparada para parecer relevante para o destinatário.

### O que monitorar?

|Fonte de dados|O que investigar|
|---|---|
|Gateway de e-mail|Remetente, anexos, URLs e resultados de análise|
|Logs de proxy|Acessos a links suspeitos|
|Sandbox|Comportamento de arquivos analisados em ambiente isolado|
|EDR|Abertura de arquivos e processos iniciados posteriormente|
|SIEM|Correlação entre entrega, acesso ao link e atividade no endpoint|

### Como interromper esse estágio?

Algumas medidas incluem:

- Filtragem de e-mails maliciosos.
    
- Bloqueio de URLs conhecidas por serem perigosas.
    
- Análise de anexos em sandbox.
    
- Quarentena de mensagens suspeitas.
    
- Treinamento dos usuários.
    

Conceito importante: a entrega de um arquivo não significa necessariamente que o sistema foi comprometido. É necessário investigar se houve abertura, execução ou exploração posterior.

## 2.4. Estágio 4 — Exploração (Exploitation)

Objetivo: aproveitar uma vulnerabilidade técnica ou uma condição que permita executar o mecanismo de ataque.

No exemplo do artigo, o funcionário abre a planilha e habilita conteúdo que permite a execução do código malicioso.

Outros cenários podem envolver a exploração de uma vulnerabilidade em uma aplicação ou serviço exposto.

### Exemplos de evidências

- Criação de processos suspeitos.
    
- Alertas de exploração no EDR.
    
- Execução inesperada de scripts.
    
- Comportamentos anormais de aplicações.
    
- Erros ou requisições suspeitas em serviços web.
    
- Atividade incomum após a abertura de um arquivo.
    

### Exemplo de análise de processos

O analista observa esta relação:

```
EXCEL.EXE
    └── powershell.exe
```

Isso significa que o Excel iniciou o PowerShell.

Essa relação pode ser suspeita em determinados contextos, mas não prova automaticamente uma exploração. O analista deve verificar a linha de comando, a origem do arquivo, o usuário, o horário e os eventos subsequentes.

### Como defender?

- Aplicar atualizações de segurança.
    
- Corrigir vulnerabilidades conhecidas.
    
- Restringir macros e scripts quando apropriado.
    
- Utilizar EDR.
    
- Aplicar políticas de execução de aplicações.
    
- Reduzir privilégios desnecessários.
    

O objetivo é impedir que um arquivo ou uma requisição inicial se transforme em execução maliciosa.

## 2.5. Estágio 5 — Instalação (Installation)

Objetivo: estabelecer um ponto de apoio no sistema, frequentemente por meio de mecanismos de persistência.

No exemplo do artigo, o trojan modifica uma chave de inicialização do Registro para continuar sendo executado depois de uma reinicialização ou de um novo logon.

Outros mecanismos mencionados incluem:

- Criação de serviços.
    
- Tarefas agendadas.
    
- Backdoors.
    
- Modificações em mecanismos de inicialização.
    

### Exemplo de investigação

O EDR identifica a criação de uma tarefa agendada por um processo suspeito.

O analista precisa descobrir:

1. Qual tarefa foi criada?
    
2. Qual comando será executado?
    
3. Qual conta executará a tarefa?
    
4. Qual processo criou a tarefa?
    
5. Quando a alteração ocorreu?
    
6. A atividade corresponde a uma instalação legítima?
    

### Como interromper esse estágio?

- Monitorar mecanismos de inicialização automática.
    
- Utilizar allowlisting de aplicações.
    
- Aplicar o princípio do menor privilégio.
    
- Verificar a integridade de arquivos e configurações.
    
- Isolar o endpoint quando necessário.
    
- Remover mecanismos de persistência após confirmar o incidente e preservar as evidências relevantes.
    

Atenção: remover apenas o arquivo malicioso pode não ser suficiente se houver outras formas de persistência ou credenciais comprometidas.

## 2.6. Estágio 6 — Comando e controle (Command and Control — C2)

Objetivo: permitir que o invasor se comunique com o sistema comprometido e envie instruções.

Depois de obter execução e estabelecer um ponto de apoio, o invasor pode precisar de um canal para controlar a máquina.

O artigo menciona que essa comunicação pode utilizar protocolos ou serviços comuns, como:

- HTTPS.
    
- DNS.
    
- Serviços de nuvem.
    

O uso de protocolos legítimos pode dificultar a diferenciação entre tráfego normal e comunicação maliciosa.

### Exemplo prático

O endpoint comprometido estabelece conexões recorrentes com um domínio suspeito a cada 60 segundos.

Esse padrão pode ser compatível com um mecanismo de beaconing, no qual o sistema verifica periodicamente se existem novas instruções.

Entretanto, conexões periódicas também são comuns em aplicações legítimas.

### O que investigar?

- Domínio e endereço IP de destino.
    
- Frequência e duração das conexões.
    
- Processos responsáveis pelo tráfego.
    
- Volume de dados transferidos.
    
- Reputação e histórico do destino.
    
- Consultas DNS relacionadas.
    
- Relação temporal com alertas de execução e persistência.
    

### Como interromper esse estágio?

- Bloquear domínios e IPs maliciosos confirmados.
    
- Utilizar monitoramento de DNS e proxy.
    
- Correlacionar conexões com eventos de endpoint.
    
- Aplicar segmentação de rede.
    
- Isolar o host quando houver evidências suficientes.
    
- Monitorar tentativas de comunicação após a contenção.
    

O bloqueio de um domínio conhecido pode interromper uma comunicação específica, mas não garante que todas as formas de controle do invasor tenham sido eliminadas.

## 2.7. Estágio 7 — Ações sobre os objetivos (Actions on Objectives)

Objetivo: executar a finalidade principal do ataque.

Essa etapa pode incluir:

- Roubo de informações.
    
- Exfiltração de dados.
    
- Criptografia de arquivos para extorsão.
    
- Destruição de informações.
    
- Fraude.
    
- Preparação para ataques posteriores.
    

### Exemplo prático

Após comprometer um endpoint, o invasor investiga compartilhamentos de arquivos, acessa documentos financeiros e transfere informações para fora da organização.

Em outro cenário, pode iniciar a criptografia de arquivos para executar um ataque de ransomware.

### O que monitorar?

|Área|Exemplos de evidências|
|---|---|
|Acesso a arquivos|Leituras incomuns de grandes quantidades de documentos|
|Identidade|Uso anômalo de contas privilegiadas|
|Rede|Transferências atípicas para destinos externos|
|DLP|Movimentação de informações sensíveis|
|EDR|Criação ou modificação massiva de arquivos|
|Sistemas críticos|Exclusão, alteração ou criptografia de dados|

### Como defender?

- Aplicar controles de saída de dados.
    
- Segmentar a rede.
    
- Restringir permissões de acesso.
    
- Monitorar operações com dados sensíveis.
    
- Manter backups protegidos.
    
- Preparar procedimentos de resposta a incidentes.
    

Quando o ataque chega às ações sobre os objetivos, pode ser necessário realizar contenção urgente, preservar evidências, avaliar o impacto e coordenar a recuperação dos sistemas.

# 3. Como interromper a Kill Chain

O artigo apresenta seis ações defensivas utilizadas para pensar em diferentes formas de interromper uma intrusão:

- Detectar.
    
- Negar.
    
- Interromper.
    
- Degradar.
    
- Enganar.
    
- Destruir.
    

O princípio é utilizar controles complementares em diferentes estágios, de modo que a falha de uma defesa não permita que o ataque avance sem resistência.

## 3.1. Controles por estágio

|Estágio|Exemplos de controles defensivos|
|---|---|
|Reconhecimento|Monitoramento web e DNS, redução da exposição pública|
|Armamento|Inteligência de ameaças e rastreamento de ferramentas e infraestrutura|
|Entrega|Segurança de e-mail, sandbox e quarentena|
|Exploração|Patching, EDR e mitigação de exploração|
|Instalação|Monitoramento de persistência, allowlisting e menor privilégio|
|C2|Análise de DNS e proxy, SIEM e bloqueio de destinos maliciosos|
|Ações sobre objetivos|DLP, segmentação, controles de saída e monitoramento de acesso a dados|

A tabela representa exemplos de medidas, não uma lista obrigatória de controles para todas as organizações.

## 3.2. Por que interromper cedo é vantajoso?

Compare duas situações.

Interrupção precoce

O e-mail malicioso é bloqueado

O usuário não recebe o arquivo e não há evidências de execução. A resposta pode se concentrar na análise da mensagem, no bloqueio de indicadores e na busca por outros destinatários.

Interrupção tardia

O invasor já estabeleceu comunicação C2

É necessário investigar o endpoint, a persistência, as credenciais utilizadas, os possíveis acessos a outros sistemas e eventuais transferências de dados.

Quanto mais tarde o ataque é identificado, maior pode ser o esforço necessário para determinar o escopo e o impacto.

Isso não significa que toda detecção tardia terá consequências graves. Significa que o SOC precisa estar preparado para investigar o que ocorreu antes do alerta.

## 3.3. Defesa em profundidade

O artigo enfatiza que não é necessário detectar todas as etapas para impedir o ataque.

Se um controle falhar na entrega, outro pode identificar a exploração. Se a exploração não for detectada, uma regra de persistência ou C2 ainda poderá interromper a progressão.

Esse princípio é conhecido como defesa em profundidade.

O aprendizado prático é não depender de uma única barreira de segurança.

# 4. Estudo de caso: violação da Equifax em 2017

O artigo utiliza o incidente da Equifax para demonstrar como a Kill Chain ajuda a entender uma intrusão real.

A violação expôs dados pessoais de aproximadamente 147 milhões de pessoas.

## 4.1. Como a sequência se desenvolveu?

Segundo o artigo, a intrusão envolveu uma aplicação Apache Struts exposta à internet e a exploração da vulnerabilidade CVE-2017-5638.

O patch já havia sido disponibilizado em março de 2017, mas a aplicação afetada não foi corrigida a tempo.

O ataque não começou com um e-mail de phishing. A exploração ocorreu por meio de uma requisição HTTP direcionada à aplicação web.

Após o comprometimento inicial, os invasores:

- Utilizaram comunicação de comando e controle.
    
- Movimentaram-se pelo ambiente.
    
- Encontraram credenciais armazenadas sem proteção adequada em um compartilhamento.
    
- Acessaram bancos de dados.
    
- Consultaram e extraíram dados em pequenos lotes.
    

O artigo relata que as atividades persistiram por mais de dois meses e que a comunicação suspeita só foi identificada posteriormente, quando um certificado expirado em um dispositivo de monitoramento foi renovado.

## 4.2. O que esse caso ensina?

### Lição 1 — A entrega não precisa ser um e-mail

A sequência tradicional de phishing não se aplica a todos os incidentes.

Em ataques contra aplicações web, uma requisição maliciosa pode levar diretamente à exploração.

### Lição 2 — Patching é fundamental

A correção de vulnerabilidades conhecidas reduz oportunidades de exploração.

No entanto, o gerenciamento de patches precisa ser acompanhado de inventário, verificação da aplicação das correções e monitoramento.

### Lição 3 — O comprometimento inicial não representa o fim da investigação

Mesmo depois do primeiro acesso, o invasor pode continuar explorando o ambiente.

A equipe precisa investigar movimento lateral, credenciais, acesso a bancos de dados e comunicação externa.

### Lição 4 — Monitoramento e segmentação podem limitar o impacto

O monitoramento da rede, a proteção de credenciais e a segmentação podem ajudar a detectar ou limitar a progressão de uma intrusão.

### Lição 5 — A Kill Chain ajuda a localizar oportunidades de defesa

O caso demonstra que a organização poderia ter interrompido a sequência em diferentes pontos: correção da vulnerabilidade, detecção da exploração, monitoramento de C2, proteção de credenciais ou detecção da exfiltração.

# 5. Cyber Kill Chain versus MITRE ATT&CK

A Cyber Kill Chain e o MITRE ATT&CK são modelos complementares, mas não são equivalentes.

|Característica|Cyber Kill Chain|MITRE ATT&CK|
|---|---|---|
|Organização|Sete estágios de alto nível|Táticas, técnicas e subtécnicas|
|Granularidade|Mais ampla|Mais detalhada|
|Sequência|Modelo originalmente sequencial|Comportamentos não necessariamente lineares|
|Principal uso|Compreender a progressão do ataque e os pontos de intervenção|Classificar comportamentos, criar detecções e orientar investigações|
|Ênfase|Sequência da intrusão|Comportamentos específicos dos adversários|
|Uso no SOC|Triagem, comunicação e planejamento defensivo|Engenharia de detecção, hunting e análise técnica|

## 5.1. Como os modelos se complementam?

A Cyber Kill Chain ajuda a responder:

> Em que parte da progressão do ataque estamos?

O MITRE ATT&CK ajuda a responder:

> Qual comportamento específico foi observado e como podemos detectá-lo?

Por exemplo, o estágio de ações sobre os objetivos pode envolver diferentes comportamentos descritos no ATT&CK, como coleta, exfiltração ou impacto.

Da mesma forma, o estágio de comando e controle pode ser detalhado por técnicas específicas de comunicação utilizadas pelo adversário.

Regra prática:

- Utilize a Kill Chain para organizar a narrativa do incidente.
    
- Utilize o ATT&CK para classificar os comportamentos específicos e orientar a investigação técnica.
    

# 6. Limitações da Cyber Kill Chain

O artigo ressalta que a Kill Chain original foi desenvolvida em 2011 e deve ser utilizada como uma estrutura útil, não como uma descrição completa de todos os ataques modernos.

## 6.1. Ênfase no perímetro

O modelo original se ajusta bem a intrusões em que um adversário externo entrega um mecanismo malicioso e compromete um sistema.

Entretanto, ataques modernos podem começar com credenciais válidas, abuso de serviços SaaS ou comprometimento de identidades na nuvem.

Esses cenários podem não apresentar claramente todos os estágios iniciais.

## 6.2. Ataques podem pular estágios

Nem toda intrusão precisa de um arquivo malicioso, instalação tradicional ou exploração de uma vulnerabilidade.

Por exemplo, uma conta comprometida pode ser utilizada diretamente para acessar aplicações corporativas.

Isso dificulta o enquadramento do incidente em uma sequência fixa.

## 6.3. Ataques reais não são perfeitamente lineares

Um invasor pode:

1. Comprometer um sistema.
    
2. Fazer reconhecimento interno.
    
3. Acessar outro servidor.
    
4. Buscar novas credenciais.
    
5. Retornar a um sistema anterior.
    
6. Repetir essas ações em outras partes da rede.
    

Esse comportamento forma uma sequência com ciclos e ramificações, em vez de uma linha reta.

## 6.4. Pouco detalhamento das atividades pós-comprometimento

A Kill Chain original agrupa comportamentos complexos em estágios mais amplos, especialmente comando e controle e ações sobre os objetivos.

O ATT&CK ajuda a investigar esses comportamentos com maior granularidade.

# 7. Modelos que ampliam a Kill Chain

O artigo menciona três extensões ou adaptações.

## 7.1. Unified Kill Chain

Desenvolvida por Paul Pols em 2017, a Unified Kill Chain combina conceitos da Kill Chain original com conhecimentos sobre técnicas adversárias.

O modelo possui 18 fases agrupadas em três grandes etapas:

- Obter um ponto de apoio inicial.
    
- Propagar-se pela rede.
    
- Atuar sobre os objetivos.
    

A proposta é representar melhor o movimento lateral e a progressão complexa de ataques modernos.

## 7.2. Monetização

Alguns fornecedores acrescentam um estágio relacionado à monetização dos resultados do ataque.

Exemplos:

- Pagamento de resgate.
    
- Revenda de dados roubados.
    
- Fraude financeira.
    

Importante: segundo o artigo, essa etapa adicional não faz parte do modelo original de sete estágios. É uma extensão utilizada em algumas representações.

## 7.3. Cyber Kill Chain para ICS

O artigo também menciona uma adaptação voltada a sistemas de controle industrial.

Nesse contexto, o ataque pode exigir o entendimento prévio de processos físicos e industriais antes de causar efeitos operacionais.

O modelo citado organiza a atividade em duas grandes etapas: espionagem e ação com efeitos físicos.

A principal lição é que o modelo escolhido deve corresponder ao ambiente que a organização precisa defender.

# 8. Como o Blue Team utiliza a Kill Chain no dia a dia

O modelo pode apoiar quatro atividades importantes do SOC.

## 8.1. Triagem e priorização de alertas

O estágio estimado do ataque ajuda a compreender a possível progressão da intrusão.

Exemplos:

- Um e-mail malicioso bloqueado pode representar uma tentativa interrompida na entrega.
    
- Um alerta confirmado de comunicação C2 pode indicar que um sistema já está comprometido.
    
- Uma detecção de exfiltração pode exigir investigação imediata sobre o impacto e os dados envolvidos.
    

O estágio não substitui a classificação de severidade. Um alerta de entrega pode ser crítico se houver muitos destinatários ou se o arquivo tiver sido executado.

## 8.2. Engenharia de detecção

A equipe pode organizar suas detecções pelos sete estágios e identificar onde há menos visibilidade.

Exemplo:

Uma organização tem várias regras para e-mails maliciosos, mas poucas detecções de comunicação externa suspeita e transferência de dados.

Essa diferença pode indicar a necessidade de melhorar o monitoramento de rede, endpoint e saída de dados.

## 8.3. Resposta a incidentes

Durante uma investigação, a Kill Chain ajuda a organizar a pergunta:

> Até onde o invasor conseguiu avançar?

Se houver apenas uma tentativa de entrega bloqueada, o escopo pode ser relativamente restrito.

Se houver evidências de persistência, C2 e acesso a outros sistemas, a equipe precisa avaliar o comprometimento mais amplamente.

## 8.4. Inteligência de ameaças e threat hunting

Os analistas podem utilizar informações sobre ameaças conhecidas para formular hipóteses de caça.

Por exemplo, se um relatório indicar que determinada campanha costuma estabelecer persistência por tarefas agendadas, a equipe pode procurar esse comportamento nos endpoints relevantes.

A hipótese deve ser testada contra os dados disponíveis. A ausência de eventos não comprova que o comportamento não ocorreu, especialmente se a telemetria estiver incompleta.

# 9. Exemplo de investigação com a Cyber Kill Chain

Considere um cenário hipotético em que o SOC recebe um alerta de comunicação suspeita de um endpoint.

O objetivo é utilizar a Kill Chain para organizar as próximas etapas da investigação.

## Passo 1 — Validar o alerta

Identifique:

- Host envolvido.
    
- Usuário.
    
- Processo responsável pela conexão.
    
- Domínio e IP de destino.
    
- Horário.
    
- Frequência das conexões.
    

Pergunta: existe evidência de comunicação maliciosa ou a atividade pode ser legítima?

## Passo 2 — Investigar a execução inicial

Consulte os eventos anteriores à conexão:

- Quais processos foram iniciados?
    
- Houve abertura de arquivo suspeito?
    
- O usuário recebeu algum e-mail incomum?
    
- Existe uma relação suspeita entre processos?
    
- Há sinais de exploração?
    

Objetivo: compreender como o endpoint pode ter sido comprometido.

## Passo 3 — Procurar persistência

Examine eventos relacionados a:

- Tarefas agendadas.
    
- Serviços recém-criados.
    
- Alterações em chaves de inicialização.
    
- Outros mecanismos de execução automática.
    

Objetivo: determinar se o invasor tentou manter o acesso.

## Passo 4 — Avaliar movimento lateral

Investigue autenticações, conexões remotas e atividades em outros hosts.

Objetivo: determinar se o comprometimento se estendeu além do endpoint inicial.

## Passo 5 — Investigar as ações sobre os objetivos

Procure evidências de:

- Acesso a dados sensíveis.
    
- Transferências externas incomuns.
    
- Alterações ou criptografia de arquivos.
    
- Uso de credenciais em sistemas adicionais.
    

Objetivo: avaliar o impacto real do incidente.

## Passo 6 — Conter e documentar

Com base nas evidências, a equipe pode isolar sistemas, bloquear indicadores, proteger contas e preservar os registros necessários.

A investigação deve registrar o que foi observado, quais hipóteses foram levantadas, quais foram confirmadas e quais ações foram tomadas.

# 10. Resumo final

A Cyber Kill Chain organiza uma intrusão direcionada em sete estágios: reconhecimento, armamento, entrega, exploração, instalação, comando e controle e ações sobre os objetivos.

Seu principal valor para o SOC é oferecer uma visão da progressão do ataque e ajudar a identificar onde aplicar controles defensivos.

Os pontos mais importantes para memorizar são:

1. O modelo descreve uma sequência de alto nível, não uma regra rígida para todos os ataques.
    
2. Interromper o ataque cedo geralmente reduz o impacto e o custo da resposta.
    
3. A defesa em profundidade é fundamental: controles em diferentes estágios ajudam a compensar falhas individuais.
    
4. O MITRE ATT&CK complementa a Kill Chain, detalhando técnicas e comportamentos específicos.
    
5. A investigação deve procurar evidências anteriores e posteriores ao alerta, para determinar a extensão do comprometimento.
    
6. O estágio não comprova sozinho a gravidade: a severidade depende do contexto, dos ativos envolvidos e do impacto potencial.
    
7. A Kill Chain deve orientar ações concretas, como melhorar detecções, validar controles e organizar a resposta a incidentes.
    

## Fonte

- Artigo-base: [CyberDefenders — Cyber Kill Chain](https://cyberdefenders.org/cybersecurity-glossary/cyber-kill-chain/?utm_source=chatgpt.com)
