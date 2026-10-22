# MITRE ATT&CK — Anotações de estudo para SOC e Blue Team

Material organizado a partir do artigo da CyberDefenders fornecido.
## 1. O que é o MITRE ATT&CK?

O MITRE ATT&CK é uma base de conhecimento que documenta comportamentos, técnicas e procedimentos utilizados por adversários durante ataques cibernéticos.

Seu objetivo é fornecer uma linguagem comum para que analistas de segurança consigam compreender, classificar, investigar e detectar atividades maliciosas.

Em um ambiente de SOC, ferramentas como SIEM e EDR podem gerar alertas técnicos. O ATT&CK ajuda o analista a interpretar esses alertas dentro do contexto de um possível ataque.

### 1.1. Por que ele é importante?

Imagine que seu SIEM gere o seguinte alerta:

> Acesso suspeito à memória do processo `lsass.exe`.

O alerta informa que um comportamento potencialmente perigoso foi observado, mas ainda é necessário entender o que isso significa.

O analista pode relacionar o comportamento à técnica:

- Tática: roubo de credenciais ou outro objetivo relacionado, conforme o contexto.
    
- Técnica: T1003 — OS Credential Dumping.
    
- Subtécnica: T1003.001 — LSASS Memory.
    

Isso ajuda a formular hipóteses de investigação: alguém pode estar tentando extrair credenciais armazenadas na memória do Windows.

Entretanto, o mapeamento não comprova, por si só, que houve comprometimento. É necessário validar os eventos, o processo envolvido, a conta utilizada e as demais evidências.

### 1.2. O que o ATT&CK documenta?

O framework organiza o conhecimento sobre o comportamento dos adversários em três conceitos fundamentais:

|Conceito|Pergunta que responde|Exemplo|
|---|---|---|
|Tática|Por que o adversário está realizando determinada atividade?|Obter acesso inicial ao ambiente|
|Técnica|Como o adversário tenta alcançar seu objetivo?|Utilizar credenciais válidas|
|Subtécnica|Qual é a forma específica de execução da técnica?|Acessar a memória do LSASS para extrair credenciais|
|Procedimento|Como uma ameaça executou a técnica na prática?|Uma sequência específica de comandos, ferramentas e ações|

Atenção: a relação entre esses elementos depende do comportamento observado. Uma mesma ferramenta pode ser utilizada para diferentes objetivos, e um mesmo objetivo pode ser alcançado por diversas técnicas.

### 1.3. Matrizes do MITRE ATT&CK

O material apresenta três matrizes principais:

- Enterprise: técnicas utilizadas contra ambientes corporativos, sistemas operacionais, redes, serviços em nuvem e outras tecnologias empresariais.
    
- Mobile: comportamentos adversários direcionados a dispositivos móveis.
    
- ICS: técnicas relacionadas a sistemas de controle industrial.
    

Para quem está começando no SOC, a matriz Enterprise costuma ser o melhor ponto de partida, especialmente ao estudar logs de Windows, Linux, autenticação, processos, redes e serviços corporativos.

## 2. Diferenças entre MITRE ATT&CK, SIEM, EDR, Sigma e NIST

Esses recursos podem ser utilizados em conjunto, mas cada um tem uma finalidade diferente.

|Recurso|Finalidade principal|
|---|---|
|MITRE ATT&CK|Organizar e classificar comportamentos e técnicas adversárias|
|SIEM|Centralizar, pesquisar e correlacionar eventos de diferentes fontes|
|EDR|Monitorar endpoints e detectar ou responder a comportamentos suspeitos|
|Sigma|Descrever regras de detecção em um formato genérico que pode ser convertido para diferentes plataformas|
|NIST|Fornecer estruturas, recomendações e orientações para gestão de riscos e segurança|

### Exemplo prático

Considere uma atividade suspeita no Windows:

1. O Sysmon registra a criação de um processo.
    
2. O SIEM recebe e armazena o evento.
    
3. Uma regra de detecção identifica uma combinação suspeita de processo pai, processo filho e linha de comando.
    
4. O alerta é associado a uma técnica do MITRE ATT&CK.
    
5. O analista investiga a atividade e determina se é legítima ou maliciosa.
    

Nesse cenário:

- O evento fornece os dados observados.
    
- A regra define a lógica que identifica o comportamento.
    
- O SIEM permite pesquisar e correlacionar os registros.
    
- O ATT&CK contextualiza o comportamento.
    
- O analista valida as evidências e decide as ações necessárias.
    

O ATT&CK não substitui as regras de detecção nem o trabalho de investigação.

## 3. Como o MITRE ATT&CK ajuda nas operações de segurança?

O framework pode ser aplicado em diferentes atividades de um SOC.

### 3.1. Engenharia de detecção

As regras de detecção podem ser associadas aos IDs das técnicas ATT&CK que pretendem identificar.

Isso permite visualizar quais comportamentos estão cobertos e quais ainda precisam de detecções.

Exemplo:

Uma organização possui várias regras para identificar execuções suspeitas de comandos, mas não possui detecções adequadas para técnicas de sequestro do fluxo de execução.

Esse cenário indica uma possível lacuna que merece avaliação.

Contudo, a quantidade de regras não determina a qualidade da cobertura. Dez regras redundantes podem oferecer menos proteção do que duas regras bem construídas, testadas e complementares.

### 3.2. Integração com inteligência de ameaças

Relatórios de inteligência de ameaças frequentemente descrevem técnicas utilizadas por grupos ou campanhas de ataque.

Ao identificar uma técnica relevante, o SOC pode verificar:

- Se possui fontes de log capazes de registrar o comportamento.
    
- Se existem regras para identificar atividades suspeitas.
    
- Se as regras estão funcionando.
    
- Se a técnica já foi observada no ambiente.
    
- Se é necessário criar ou melhorar uma detecção.
    

O objetivo é transformar informações sobre ameaças em ações concretas de defesa.

### 3.3. Exercícios de Purple Team

As equipes Red Team e Blue Team podem utilizar o ATT&CK como referência comum.

- Red Team: simula atividades adversárias autorizadas.
    
- Blue Team: monitora, detecta, investiga e responde às atividades.
    
- Purple Team: promove a colaboração entre as duas equipes para validar e melhorar as defesas.
    

Por exemplo, uma equipe pode executar um teste controlado de acesso à memória do LSASS e verificar se o EDR registra o comportamento esperado.

O exercício deve avaliar não apenas se um alerta apareceu, mas também se a telemetria foi coletada, se a regra identificou o comportamento correto e se o analista conseguiu investigar o evento.

### 3.4. Relatórios de incidentes

Em vez de escrever apenas:

> O invasor acessou outros computadores da rede.

O relatório pode identificar técnicas específicas relacionadas ao comportamento observado, como:

- T1021.001 — Remote Desktop Protocol (RDP).
    
- T1021.002 — SMB/Windows Admin Shares.
    

Esse detalhamento torna o relatório mais preciso e facilita a reprodução da investigação.

A técnica só deve ser registrada quando houver evidências suficientes para sustentar o mapeamento.

# 4. Principais táticas do ATT&CK para estudar no SOC

O artigo apresenta exemplos de cinco táticas. Elas ajudam a compreender objetivos comuns durante uma intrusão.

## 4.1. Acesso inicial — TA0001

Objetivo: obter o primeiro acesso ao ambiente da vítima.

Algumas possibilidades incluem:

- Phishing.
    
- Exploração de aplicações expostas.
    
- Uso de credenciais válidas comprometidas.
    
- Acesso por serviços remotos expostos.
    

### Exemplo de investigação

Um SIEM registra várias tentativas de autenticação malsucedidas seguidas de uma autenticação bem-sucedida.

O analista deve investigar:

1. Qual conta foi utilizada?
    
2. De quais endereços IP vieram as tentativas?
    
3. As tentativas ocorreram em um intervalo curto?
    
4. O login bem-sucedido veio do mesmo IP ou de outro?
    
5. A localização, o horário e o dispositivo são compatíveis com o comportamento habitual?
    
6. Quais atividades ocorreram depois do login?
    

Uma sequência de falhas seguida de sucesso pode indicar comprometimento de credenciais, mas também pode resultar de erros legítimos de digitação ou de aplicações mal configuradas.

Técnica relacionada no artigo: T1078 — Valid Accounts (Contas Válidas).

Essa técnica descreve o uso de contas legítimas para realizar atividades que podem ser maliciosas. Ela não significa que toda autenticação bem-sucedida seja suspeita.

## 4.2. Execução — TA0002

Objetivo: executar código, comandos ou instruções no sistema.

Exemplos de comportamentos que merecem investigação:

- Execução suspeita de PowerShell.
    
- Uso incomum do `cmd.exe`.
    
- Scripts com comandos codificados.
    
- Criação de processos por aplicações inesperadas.
    
- Execução de tarefas em horários atípicos.
    

### Exemplo

O SIEM identifica a seguinte relação:

`cmd.exe` → `powershell.exe`

Essa relação significa que o processo `cmd.exe` iniciou o processo `powershell.exe`.

Essa sequência, isoladamente, é legítima em muitos cenários. A investigação deve considerar:

- A linha de comando completa.
    
- O usuário responsável.
    
- O processo pai e sua origem.
    
- O horário da execução.
    
- O destino de eventuais conexões de rede.
    
- A existência de arquivos baixados ou outras atividades suspeitas.
    

Técnica relacionada: T1059 — Command and Scripting Interpreter.

A subtécnica T1059.001 corresponde especificamente ao PowerShell.

## 4.3. Persistência — TA0003

Objetivo: manter o acesso ao sistema mesmo após eventos como reinicializações ou alterações no ambiente.

Alguns mecanismos citados no artigo:

- Chaves de execução do Registro do Windows.
    
- Tarefas agendadas.
    
- Assinaturas ou mecanismos de execução associados ao WMI.
    
- Criação de serviços.
    
- DLL sideloading.
    

### Exemplo: chaves de inicialização do Registro

Uma localização relevante é:

`HKLM\Software\Microsoft\Windows\CurrentVersion\Run`

Programas registrados nessa chave podem ser executados automaticamente durante a inicialização ou o logon, conforme o mecanismo utilizado.

O analista deve verificar:

- Qual valor foi criado ou modificado.
    
- Qual executável ou comando será iniciado.
    
- Se o caminho é esperado.
    
- Qual processo realizou a alteração.
    
- Se a mudança coincide com uma instalação legítima ou janela de manutenção.
    

Uma entrada nova não é necessariamente maliciosa. O contexto e a legitimidade do programa precisam ser avaliados.

## 4.4. Elevação de privilégios — TA0004

Objetivo: obter permissões superiores às que o adversário possui inicialmente.

Um usuário com privilégios limitados pode tentar obter acesso administrativo ou executar ações reservadas a contas privilegiadas.

O artigo destaca comportamentos como:

- Manipulação suspeita de tokens.
    
- Uso de privilégios incomuns.
    
- Exploração de drivers vulneráveis.
    
- Relações incomuns entre processos.
    
- Uso de ferramentas administrativas ou de segurança ofensiva.
    

### Exemplo de análise

Um processo iniciado por um usuário comum passa a executar atividades com privilégios elevados.

O analista deve investigar:

1. Qual usuário iniciou a atividade?
    
2. Qual processo originou a execução?
    
3. Houve uma solicitação legítima de elevação?
    
4. Qual mecanismo concedeu os privilégios?
    
5. Qual foi a primeira atividade privilegiada?
    
6. O comportamento foi seguido de alterações no sistema ou acesso a outros recursos?
    

O uso de uma ferramenta administrativa, como o PsExec, não comprova um ataque. A avaliação depende do contexto, da autorização e do comportamento observado.

## 4.5. Movimento lateral — TA0008

Objetivo: deslocar-se de um sistema para outros sistemas do ambiente.

Depois de comprometer uma máquina, um invasor pode tentar utilizar credenciais, ferramentas administrativas ou serviços remotos para acessar outros computadores.

Técnicas relacionadas apresentadas no artigo:

|ID|Técnica ou subtécnica|O que observar|
|---|---|---|
|T1021.001|Remote Desktop Protocol (RDP)|Conexões e autenticações remotas incomuns|
|T1021.002|SMB/Windows Admin Shares|Acesso a compartilhamentos administrativos e movimentação entre hosts|
|T1047|Windows Management Instrumentation (WMI)|Execução ou administração remota via WMI|

### Padrão de investigação

Imagine que a mesma conta administrativa acesse cinco servidores em poucos minutos, embora normalmente seja utilizada em apenas um deles.

O analista pode:

1. Identificar todos os hosts acessados.
    
2. Comparar os horários das autenticações.
    
3. Verificar o IP de origem e o usuário.
    
4. Examinar os processos executados em cada host.
    
5. Procurar sinais de execução remota.
    
6. Correlacionar as atividades com outros alertas.
    

A sequência pode representar administração legítima, automação ou movimento lateral. É necessário verificar se os acessos são esperados.

# 5. Técnicas ATT&CK frequentemente investigadas

Esta seção reúne as técnicas destacadas no artigo e explica como utilizá-las na investigação.

## 5.1. T1059 — Interpretador de Comandos e Scripts

A técnica T1059 descreve o uso de interpretadores de comandos e linguagens de script para executar instruções.

Exemplos:

- PowerShell.
    
- Windows Command Shell (`cmd.exe`).
    
- Outros interpretadores e ambientes de scripting contemplados pelo framework.
    

### Fontes de telemetria citadas

- Logs operacionais do PowerShell.
    
- Sysmon Event ID 1, relacionado à criação de processos.
    
- Windows Event ID 4104, relacionado ao registro de blocos de script do PowerShell, quando essa funcionalidade está habilitada e configurada.
    

### Indicadores que merecem atenção

- Comandos codificados sem justificativa aparente.
    
- Tentativas suspeitas de contornar políticas de execução.
    
- Scripts iniciados por processos inesperados.
    
- Comandos que baixam arquivos de fontes externas.
    
- Execução de scripts por contas ou em horários incomuns.
    

### Como investigar

Não se limite a pesquisar a palavra `powershell.exe`.

Analise a linha de comando, o processo pai, o usuário, o horário, os arquivos envolvidos e os eventos de rede associados.

O registro de blocos de script pode ajudar a compreender o conteúdo executado, desde que a coleta esteja habilitada e os eventos estejam disponíveis.

## 5.2. T1003 — OS Credential Dumping

Essa técnica descreve métodos utilizados para obter credenciais de sistemas operacionais.

A subtécnica T1003.001 trata especificamente da memória do LSASS.

### Artefatos citados no artigo

- Arquivos de despejo de memória (memory dumps).
    
- Acesso suspeito ao processo `lsass.exe`.
    
- Uso de componentes como `comsvcs.dll` em determinados procedimentos.
    
- Resquícios de ferramentas de extração de credenciais.
    
- Solicitações de replicação de diretório não autorizadas, quando aplicáveis ao ambiente.
    

### Como investigar

1. Identifique qual processo acessou o LSASS.
    
2. Verifique o usuário e os privilégios do processo.
    
3. Examine a árvore de processos.
    
4. Procure arquivos de despejo de memória.
    
5. Correlacione o evento com outras atividades suspeitas.
    
6. Verifique se houve uso de credenciais em outros sistemas.
    

Ponto importante: o acesso ao LSASS pode ocorrer em operações legítimas de segurança e administração. Uma detecção precisa de contexto e evidências complementares.

## 5.3. T1021 — Serviços Remotos

Essa técnica descreve o uso de serviços remotos para acessar ou controlar outros sistemas.

O artigo destaca três exemplos:

- Tentativas de força bruta contra RDP.
    
- Uso de SMB com credenciais comprometidas.
    
- Execução remota utilizando WMI.
    

### Evidências úteis

- Autenticações remotas bem-sucedidas e malsucedidas.
    
- IPs de origem e destino.
    
- Contas utilizadas.
    
- Criação de processos após o acesso remoto.
    
- Acesso a compartilhamentos administrativos.
    
- Eventos correlacionados em vários hosts.
    

A presença de conexões RDP ou SMB não é suficiente para concluir que existe movimento lateral. O analista precisa determinar se a atividade é anômala e se está relacionada a uma intrusão.

## 5.4. T1036 — Masquerading

Essa técnica descreve o disfarce de arquivos, processos ou outras atividades para parecerem legítimos.

### Exemplo

Um executável chamado `svchost.exe` é encontrado em:

`C:\Users\Public\`

O nome se parece com o de um processo legítimo do Windows, mas o caminho é incomum para esse tipo de binário.

### Como investigar

- Verifique o caminho completo do arquivo.
    
- Confira a assinatura digital.
    
- Calcule e consulte o hash, quando apropriado.
    
- Examine o processo pai.
    
- Verifique a linha de comando.
    
- Analise conexões de rede e atividade associada.
    

Um nome suspeito não comprova malware. Arquivos legítimos podem ter nomes semelhantes, e um arquivo malicioso pode utilizar um nome aparentemente normal.

## 5.5. T1053 — Scheduled Task/Job

Essa técnica trata do uso de tarefas agendadas e mecanismos semelhantes para executar atividades automaticamente.

O artigo destaca o uso de `schtasks.exe` e a criação de tarefas que podem servir como mecanismos de persistência.

### Indicadores de investigação

- Tarefas criadas por contas inesperadas.
    
- Executáveis iniciados a partir de diretórios incomuns.
    
- Agendamentos em horários suspeitos.
    
- Tarefas criadas logo após uma atividade potencialmente maliciosa.
    
- Alterações não autorizadas em tarefas existentes.
    

### Perguntas úteis

- Qual tarefa foi criada ou modificada?
    
- Qual comando será executado?
    
- Qual conta executará a tarefa?
    
- Quando ela foi criada?
    
- O processo responsável pela criação é legítimo?
    
- A tarefa faz parte de uma aplicação ou rotina conhecida?
    

Uma tarefa agendada que executa PowerShell pode envolver tanto a técnica relacionada a tarefas agendadas quanto a execução de scripts. O mapeamento depende do objetivo e do comportamento investigado.

## 5.6. T1047 — Windows Management Instrumentation (WMI)

O WMI é um mecanismo legítimo de administração e gerenciamento do Windows que também pode ser utilizado por adversários.

### Fontes de telemetria citadas

- Sysmon Event ID 1, para criação de processos.
    
- Sysmon Event ID 3, para conexões de rede, quando habilitado.
    
- Logs de eventos relacionados ao WMI.
    

### Exemplo de comportamento

O processo `WmiPrvSE.exe` aparece relacionado à criação de processos como `powershell.exe` ou `cmd.exe`.

O analista deve verificar se a atividade faz parte de uma operação administrativa legítima ou se está associada a execução remota não autorizada.

# 6. Como mapear um alerta para o MITRE ATT&CK

O artigo apresenta um processo de quatro etapas.

## 1

Identificar o comportamento observável

O que foi efetivamente registrado? Uma conexão, uma autenticação, a criação de um processo ou uma alteração no Registro?

## 2

Determinar o objetivo provável

A atividade parece relacionada a acesso inicial, execução, persistência, coleta de dados ou outro objetivo?

## 3

Escolher a técnica mais específica sustentada pelas evidências

Prefira a subtécnica quando os dados permitirem identificá-la. Não atribua detalhes que não foram observados.

## 4

Validar as fontes de dados

Confirme que os logs necessários existem e realmente permitem identificar o comportamento que a detecção pretende reconhecer.

## 6.1. Exemplo: PowerShell suspeito

Suponha que um alerta identifique o PowerShell executando um comando codificado.

Etapa 1 — Comportamento observado: o PowerShell foi iniciado com parâmetros incomuns.

Etapa 2 — Intenção provável: pode haver execução de comandos suspeitos. Ainda é necessário entender o que o comando faz.

Etapa 3 — Mapeamento: T1059.001 pode ser adequado se as evidências sustentarem o uso do PowerShell como interpretador.

Etapa 4 — Validação: consulte a linha de comando, os eventos de criação de processos, os logs do PowerShell e as conexões de rede relacionadas.

A mesma execução pode fazer parte de outra técnica ou de uma sequência maior de ataque. Por exemplo, se o PowerShell estiver sendo iniciado por uma tarefa agendada criada para manter acesso ao sistema, a persistência também deve ser investigada.

### Por que não mapear tudo automaticamente?

O PowerShell é utilizado em administração, manutenção e automação legítimas.

Por isso, uma regra que alerta sobre qualquer execução de PowerShell pode produzir muitos falsos positivos.

Uma detecção mais útil considera elementos como:

- Argumentos suspeitos.
    
- Origem do processo.
    
- Usuário e privilégios.
    
- Contexto temporal.
    
- Acesso a recursos incomuns.
    
- Evidências correlacionadas.
    

Regra de estudo: um comportamento observado não é a mesma coisa que uma intenção comprovada. O mapeamento ATT&CK deve representar a interpretação sustentada pelos dados.

# 7. MITRE ATT&CK aplicado à investigação de incidentes

Durante a resposta a incidentes (Incident Response — IR), o ATT&CK pode ajudar a organizar as evidências e reconstruir a sequência de atividades.

## 7.1. Triagem inicial

Na triagem, o analista recebe um alerta e determina sua prioridade, seu contexto e os próximos passos.

Exemplos de alertas iniciais:

- E-mail suspeito.
    
- Login anômalo.
    
- Detecção de malware.
    
- Execução suspeita de um processo.
    
- Conexão de rede incomum.
    

O alerta inicial orienta quais evidências devem ser coletadas.

Por exemplo, uma suspeita de phishing pode exigir a análise do cabeçalho do e-mail, dos anexos, dos links e das atividades posteriores no endpoint.

Uma suspeita de uso de credenciais comprometidas pode exigir logs de autenticação, informações sobre o dispositivo e atividades realizadas após o login.

## 7.2. Construção da linha do tempo

O analista organiza os eventos cronologicamente para entender o que aconteceu antes, durante e depois do alerta.

Uma sequência possível é:

01

Acesso inicial

O adversário obtém acesso ao ambiente.

02

Execução

Comandos ou programas são executados.

03

Persistência

O adversário tenta manter o acesso.

04

Elevação de privilégios

O adversário tenta obter permissões superiores.

05

Movimento lateral

Outros sistemas são acessados.

06

Exfiltração

Dados são transferidos para fora do ambiente.

Essa sequência é apenas ilustrativa. Ataques reais não precisam seguir todas essas etapas nem obedecer a uma ordem fixa.

O valor da linha do tempo está em relacionar os eventos e identificar a progressão real do incidente.

## 7.3. Análise de causa raiz

A análise de causa raiz busca entender como o incidente começou, quais condições permitiram sua ocorrência e como o adversário manteve ou ampliou o acesso.

Perguntas importantes:

- Qual foi o vetor de entrada?
    
- Quais sistemas foram afetados?
    
- Quais contas foram comprometidas?
    
- Houve persistência?
    
- Houve elevação de privilégios?
    
- O adversário acessou outros sistemas?
    
- Houve coleta ou exfiltração de dados?
    
- Quais medidas são necessárias para impedir a recorrência?
    

O artigo ressalta que reinstalar um sistema, isoladamente, pode não ser suficiente quando existem outros mecanismos de persistência ou contas comprometidas que continuam oferecendo acesso.

A remediação precisa considerar o escopo completo do incidente.

## 7.4. Exemplo de estrutura para relatório de incidente

O artigo fornece um exemplo de relatório organizado por tática:

|Etapa|Mapeamento apresentado no artigo|
|---|---|
|Acesso inicial|T1566.001 — Spearphishing Attachment|
|Execução|T1204.002 — Malicious File|
|Persistência|T1547.001 — Registry Run Keys / Startup Folder|
|Elevação de privilégios|T1055 — Process Injection|

Nota de interpretação: essa tabela reproduz os mapeamentos apresentados na fonte. Em um relatório real, os IDs e os nomes devem ser conferidos na versão vigente do ATT&CK, e cada técnica precisa ser sustentada pelas evidências do incidente.

# 8. ATT&CK Navigator: visualizando a cobertura de detecção

O ATT&CK Navigator é uma ferramenta para visualizar e compartilhar camadas de técnicas e subtécnicas do framework.

Uma aplicação comum é montar um mapa da cobertura de detecção do SOC.

## 8.1. Construção da camada de cobertura

A equipe identifica as detecções existentes e associa cada regra às técnicas pertinentes.

Depois, registra quais técnicas possuem detecções implementadas, quais foram testadas e quais ainda precisam de trabalho.

A visualização pode utilizar cores para representar diferentes estados, conforme a convenção definida pela equipe.

Por exemplo:

- Técnicas com detecções implementadas.
    
- Técnicas com detecções testadas.
    
- Técnicas sem cobertura conhecida.
    
- Técnicas prioritárias para desenvolvimento.
    

As cores não possuem um significado universal obrigatório; o importante é documentar o que cada estado representa.

## 8.2. Priorização de lacunas

Nem todas as técnicas têm a mesma relevância para todas as organizações.

Uma instituição financeira pode priorizar comportamentos relacionados ao roubo de dados e ao comprometimento de contas. Uma organização de saúde pode dar atenção especial a técnicas observadas em ataques de ransomware.

A priorização deve considerar:

- O perfil da organização.
    
- Os ativos críticos.
    
- As ameaças relevantes.
    
- Os incidentes anteriores.
    
- A disponibilidade de fontes de log.
    
- O impacto potencial de cada cenário.
    

## 8.3. Compartilhamento das camadas

O Navigator permite compartilhar camadas, inclusive por meio de arquivos JSON.

Isso ajuda a equipe a documentar:

- O estado da cobertura.
    
- As prioridades de detecção.
    
- Os resultados de exercícios.
    
- As técnicas relevantes para ameaças específicas.
    
- A evolução das capacidades de defesa.
    

## 8.4. O que significa cobertura real?

Uma técnica não deve ser considerada adequadamente coberta apenas porque existe uma regra relacionada a ela.

É necessário verificar se:

1. Os eventos relevantes são coletados.
    
2. A regra detecta o comportamento pretendido.
    
3. A detecção foi testada.
    
4. Os falsos positivos são conhecidos e tratados.
    
5. O alerta fornece evidências úteis para a investigação.
    
6. O processo de resposta é conhecido pela equipe.
    

Uma regra existente, mas que nunca foi validada, não oferece a mesma garantia que uma detecção testada e operacional.

# 9. Erros comuns ao utilizar o MITRE ATT&CK

## Erro 1 — Mapear somente para a tática

Indicar apenas “Execução” ou “Persistência” é pouco específico.

Melhor abordagem: registrar a técnica ou subtécnica que corresponde ao comportamento identificado, quando as evidências permitirem.

## Erro 2 — Mapear tudo para execução

Muitas atividades envolvem a execução de algum código, mas isso não significa que execução seja sempre a classificação mais útil.

Uma tarefa agendada pode ser relevante principalmente por estabelecer persistência, ainda que execute PowerShell.

O mapeamento deve considerar o objetivo e o comportamento detectado.

## Erro 3 — Ignorar subtécnicas

O uso de IDs mais específicos ajuda a criar detecções mais direcionadas.

Por exemplo:

- T1003: OS Credential Dumping.
    
- T1003.001: LSASS Memory.
    

Se a evidência identifica especificamente acesso à memória do LSASS para extração de credenciais, a subtécnica oferece uma descrição mais precisa do que a técnica geral.

## Erro 4 — Tratar o ATT&CK como uma lista de conformidade

Não basta marcar uma técnica como coberta porque a organização possui logs de autenticação.

É preciso verificar se os dados disponíveis e as regras existentes conseguem detectar os comportamentos relevantes, como uso anômalo de contas válidas.

O objetivo é melhorar a segurança, e não apenas aumentar o número de técnicas marcadas.

## Erro 5 — Não validar as fontes de log

Uma regra que pretende detectar injeção de processos, por exemplo, depende de visibilidade adequada sobre o comportamento investigado.

Se a fonte não registra os eventos necessários, a detecção pode falhar sem que a equipe perceba.

Por isso, o analista deve conhecer as limitações da telemetria e testar as regras em condições controladas.

# 10. MITRE ATT&CK para a maturidade do SOC e Blue Team

O framework também pode servir como roteiro para desenvolver as capacidades técnicas de uma equipe.

## 10.1. Avaliação de maturidade

A organização pode acompanhar:

- Quantas técnicas relevantes possuem detecções.
    
- Quantas dessas detecções foram testadas.
    
- Quantas fontes de dados estão disponíveis.
    
- Quais são as lacunas mais importantes.
    
- Quanto tempo a equipe leva para investigar determinados alertas.
    
- Se os procedimentos de resposta estão documentados.
    

Percentuais de cobertura podem ajudar a acompanhar a evolução, mas precisam ser interpretados com cuidado.

Uma cobertura de 70% não significa automaticamente que o ambiente está 70% protegido. O resultado depende da relevância das técnicas, da qualidade das regras e da capacidade de resposta.

## 10.2. Alinhamento entre Red Team e Blue Team

Os exercícios podem ser planejados em torno de técnicas específicas.

Em vez de solicitar apenas “tente comprometer o ambiente”, a equipe pode definir cenários controlados para testar determinadas técnicas e verificar:

- Se os eventos foram gerados.
    
- Se chegaram ao SIEM.
    
- Se a regra acionou um alerta.
    
- Se a investigação conseguiu recuperar as evidências.
    
- Se as ações de resposta foram adequadas.
    

Esses testes devem ser autorizados, planejados e executados em ambientes apropriados.

## 10.3. Desenvolvimento das habilidades dos analistas

Para analistas juniores, o ATT&CK ajuda a reconhecer comportamentos comuns e a formular perguntas de investigação.

Com maior experiência, o analista pode aprofundar o estudo de:

- Técnicas relacionadas.
    
- Sequências de ataque.
    
- Evasão de detecção.
    
- Limitações das fontes de dados.
    
- Correlação entre endpoints, identidade e rede.
    
- Engenharia e validação de detecções.
    

Cada investigação pode se tornar uma oportunidade de aprendizado quando as evidências, as hipóteses e os resultados são documentados de maneira consistente.

## 10.4. Melhoria contínua

O artigo sugere acompanhar a evolução da cobertura de detecção ao longo do tempo.

Por exemplo, uma equipe pode comparar o estado das detecções no primeiro e no terceiro trimestre, desde que utilize critérios consistentes de avaliação.

O objetivo é identificar se as mudanças realizadas produziram melhorias reais, e não apenas um aumento no número de regras cadastradas.

# 11. Exemplo prático de investigação em um SOC

Este cenário reúne os conceitos estudados em uma situação hipotética.

## Cenário

O SIEM identifica diversas autenticações malsucedidas para uma conta corporativa, seguidas de uma autenticação bem-sucedida. Em seguida, a conta acessa um segundo servidor, onde aparece uma execução suspeita de PowerShell.

### Etapa 1 — Validar o alerta de autenticação

Investigue:

- Conta envolvida.
    
- IP de origem.
    
- Horários das tentativas.
    
- Resultado das autenticações.
    
- Dispositivos acessados.
    
- Histórico de uso da conta.
    

Objetivo: determinar se existe evidência de uso anômalo de credenciais.

### Etapa 2 — Investigar o login bem-sucedido

Verifique se o acesso foi realizado a partir de um dispositivo, IP ou horário incomum.

Procure também outros logins relacionados à conta.

Possível relação com ATT&CK: T1078 — Valid Accounts, caso as evidências sustentem essa hipótese.

### Etapa 3 — Examinar o segundo servidor

Investigue os eventos de autenticação remota, os processos iniciados e os comandos executados.

Procure identificar como a conta chegou ao segundo sistema.

Possível relação com ATT&CK: técnicas de serviços remotos, se o comportamento observado corresponder a elas.

### Etapa 4 — Analisar a execução de PowerShell

Colete a linha de comando, o processo pai, o usuário, o horário e os eventos correlacionados.

Determine se a execução faz parte de uma atividade administrativa legítima ou se apresenta indicadores de comprometimento.

Possível relação com ATT&CK: T1059.001, se houver evidências do uso de PowerShell como mecanismo de execução relevante para a investigação.

### Etapa 5 — Construir a linha do tempo

Organize os eventos em ordem cronológica e identifique as relações entre eles.

Exemplo de estrutura:

|Horário|Evento|Pergunta investigativa|
|---|---|---|
|10:00|Falhas de autenticação|Houve tentativa de adivinhar ou abusar de credenciais?|
|10:03|Login bem-sucedido|O acesso foi legítimo?|
|10:07|Acesso a outro servidor|Como ocorreu a conexão?|
|10:09|PowerShell iniciado|Qual comando foi executado e por quê?|

Os horários são ilustrativos, não evidências de um incidente real.

### Etapa 6 — Determinar a resposta

Com base nas evidências e na gravidade do caso, o analista pode recomendar medidas como:

- Validar a atividade com o responsável pela conta.
    
- Revogar sessões ou redefinir credenciais comprometidas.
    
- Isolar um endpoint, se houver indícios suficientes e conforme o procedimento de resposta.
    
- Preservar logs e artefatos.
    
- Investigar outros sistemas relacionados.
    
- Criar ou ajustar detecções.
    

Importante: a investigação não deve presumir que todas as etapas fazem parte de um ataque. Cada hipótese precisa ser confirmada ou descartada com base nos dados.

# 12. Exemplo de consulta SPL do artigo

A fonte apresenta a seguinte consulta para investigar falhas de autenticação no Splunk:

```
index=authentication action=failure src_ip=* user=*
| stats count by user, src_ip
| where count > 5
| join user [search index=authentication action=success]
```

## 12.1. Entendendo cada linha

### Linha 1

```
index=authentication action=failure src_ip=* user=*
```

Pesquisa eventos no índice `authentication` que tenham:

- `action=failure`: autenticação malsucedida.
    
- `src_ip=*`: campo `src_ip` preenchido.
    
- `user=*`: campo `user` preenchido.
    

Os nomes dos campos e o valor de `action` dependem da forma como os dados foram normalizados e ingeridos no Splunk.

### Linha 2

```
| stats count by user, src_ip
```

Agrupa os eventos por usuário e IP de origem e calcula quantos eventos existem em cada grupo.

O resultado terá uma contagem para cada combinação de usuário e IP.

### Linha 3

```
| where count > 5
```

Mantém somente os grupos que apresentaram mais de cinco eventos.

Esse limite é um exemplo. O valor adequado depende do ambiente, da janela temporal e do comportamento normal de autenticação.

### Linha 4

```
| join user [search index=authentication action=success]
```

Tenta combinar os resultados das falhas com eventos de sucesso utilizando o campo `user`.

Limitação importante: essa consulta, tal como apresentada, não demonstra necessariamente que as falhas foram seguidas por um login bem-sucedido do mesmo IP. O `join` utiliza apenas o usuário como chave, e a pesquisa interna não especifica uma janela temporal nem restringe o sucesso ao IP de origem.

Portanto, ela serve para estudar a estrutura da consulta, mas precisa ser revisada antes de ser usada como detecção de produção.

### O que uma investigação mais robusta deveria considerar?

- Uma janela temporal explícita.
    
- A relação cronológica entre falhas e sucesso.
    
- O IP associado ao evento de sucesso.
    
- A possibilidade de a conta acessar legitimamente vários dispositivos.
    
- Os limites e o custo de desempenho de operações de correlação no Splunk.
    

A consulta também não confirma, sozinha, que houve um ataque. Ela identifica um padrão que pode merecer investigação.

# 15. Resumo final

O MITRE ATT&CK ajuda o analista SOC a transformar eventos técnicos em uma compreensão estruturada dos comportamentos adversários.

Os pontos essenciais deste material são:

1. Táticas representam objetivos: ajudam a entender por que determinada atividade pode estar ocorrendo.
    
2. Técnicas e subtécnicas descrevem comportamentos: fornecem uma classificação mais específica para as atividades observadas.
    
3. Os logs sustentam a investigação: o mapeamento deve partir de evidências, e não apenas do nome de um alerta.
    
4. O contexto é fundamental: uma atividade legítima pode se parecer com uma técnica adversária.
    
5. O ATT&CK ajuda a construir detecções: permite identificar lacunas, planejar testes e melhorar regras do SIEM e do EDR.
    
6. O framework apoia a resposta a incidentes: ajuda a relacionar eventos, construir linhas do tempo e documentar os comportamentos identificados.
    
7. Cobertura precisa ser validada: possuir uma regra ou uma fonte de log não garante que a técnica será detectada.
    
8. O aprendizado deve ser prático: analisar eventos, testar regras e documentar evidências transforma conhecimento teórico em habilidade operacional.
    

## Fontes e referências

- Artigo-base: [CyberDefenders — MITRE ATT&CK Framework](https://cyberdefenders.org/blog/mitre-attack-framework/?utm_source=chatgpt.com)
    
- [MITRE ATT&CK — site oficial](https://attack.mitre.org/?utm_source=chatgpt.com)
    
- [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/?utm_source=chatgpt.com)
    

Estas anotações foram estruturadas a partir do conteúdo fornecido. Os exemplos de investigação servem para estudo e não representam incidentes reais. Para uso operacional, confirme os IDs, as descrições das técnicas e os requisitos de telemetria na documentação vigente do ATT&CK e nas documentações das ferramentas utilizadas.