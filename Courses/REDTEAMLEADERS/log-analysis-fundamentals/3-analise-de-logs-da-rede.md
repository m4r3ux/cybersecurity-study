# Firewall logs e monitoramento de perímetro

Firewalls são o ponto mais externo de visibilidade da rede. Antes mesmo de um ataque chegar no sistema operacional ou gerar logs de autenticação, ele já passa pelo firewall. Por isso, esses logs costumam ser o primeiro indício de atividade maliciosa.

Enquanto logs de host (Linux/Windows) mostram o que aconteceu *dentro* da máquina, logs de firewall mostram *quem tentou chegar até ela* — e isso muda completamente a perspectiva de análise.

Pensamento chave:
firewall = tentativa de acesso  
host = execução real  



# O que realmente aparece nos logs

Um log de firewall, na prática, descreve uma tentativa de comunicação entre dois pontos.

Ele sempre responde perguntas essenciais:
- quem tentou (IP de origem)
- para onde (IP de destino)
- como (protocolo/porta)
- o que aconteceu (allow ou deny)

Exemplo clássico:
tentativa de conexão TCP na porta 22 com flag SYN

Isso, isoladamente, já indica início de conexão — e possivelmente tentativa de acesso SSH.



# Mais importante que o log: o padrão

Assim como em autenticação, um evento isolado raramente diz algo relevante.

O valor está no comportamento:

- muitas conexões negadas → possível scan
- várias portas diferentes → enumeração
- padrão repetitivo → automação (ferramenta)

Exemplo típico:
um único IP tentando dezenas de portas em poucos segundos  
→ port scanning

Outro padrão:
vários IPs tentando a mesma porta  
→ ataque distribuído (botnet)



# Entrada, saída e lateral

Logs de firewall não são só sobre ataques externos.

Eles mostram três direções importantes:

Entrada (inbound):
- internet → rede interna  
- foco: tentativas de invasão

Saída (outbound):
- host interno → internet  
- foco: malware, C2, exfiltração

Lateral (east-west):
- comunicação entre máquinas internas  
- foco: movimento lateral

Isso é crítico.

Exemplo:
um endpoint comprometido pode parecer normal no host  
mas no firewall ele começa a:
- tentar conexões externas estranhas
- usar portas incomuns
- comunicar com IPs maliciosos

Ou seja:
firewall revela comportamento que o endpoint esconde



# Correlação com outros logs

Aqui é onde firewall ganha valor real.

Exemplo prático:

Firewall:
- IP externo tentando acessar porta 22

Linux:
- várias falhas de SSH

→ confirmação de brute force

Outro:

Firewall:
- host interno conectando em IP suspeito

Endpoint:
- execução de powershell ou malware

→ possível C2 ativo

Firewall sozinho levanta suspeita  
correlação confirma ataque  



# Casos clássicos de detecção

Port scanning:
muitas conexões negadas para várias portas  
→ reconhecimento inicial

Acesso não autorizado:
tentativa de acessar serviços internos  
(ex: RDP, SSH, SMB)

Movimento lateral:
máquinas internas se comunicando fora do padrão  
(ex: workstation acessando servidor via SMB sem motivo)

Beaconing (malware):
conexões periódicas para o mesmo IP externo  
→ padrão de C2

Exfiltração:
grande volume de tráfego de saída  
ou uso de portas incomuns



# O papel do "deny"

Muita gente ignora logs bloqueados — erro crítico.

Logs de deny são:
- tentativas de ataque
- reconhecimento
- atividade que *não deveria existir*

Na prática:
deny mostra o atacante tentando  
allow pode mostrar ele conseguindo  

Ambos são importantes, mas deny revela intenção.



# Enriquecimento: transformando log em contexto

Um IP sozinho não diz muito.

Mas quando você adiciona contexto:
- país (GeoIP)
- reputação (threat intel)
- tipo de serviço (porta)

o mesmo log ganha significado.

Exemplo:
IP tentando SSH vindo de outro país + listado em blacklist  
→ risco muito maior

Isso é o que SIEM faz bem:
transformar dados em contexto acionável



# SIEM e deteccao

No SIEM, logs de firewall viram campos como:
- source.ip
- destination.ip
- destination.port
- action

Isso permite criar regras como:

muitos bloqueios do mesmo IP  
ou  
conexões para portas críticas (22, 3389, 445)

E o mais importante:
correlacionar com logs de autenticação e endpoint



# Pensamento operacional

Firewall mostra o início da história.

Ele responde:
"quem está tentando entrar ou sair?"

Mas não responde sozinho:
"o ataque funcionou?"

Por isso:
firewall + endpoint + autenticação = visão completa



# Resumo mental

Firewall = perímetro e visibilidade inicial  

Muitas conexões → scan  
Muitas portas → enumeração  
Saída suspeita → possível malware  
Tráfego interno estranho → movimento lateral  

Logs bloqueados mostram tentativa  
Logs permitidos podem mostrar sucesso  

Firewall não confirma ataque sozinho  
mas quase sempre mostra onde ele começou


---

# IDS/IPS logs e deteccao de ameacas em rede

Diferente do firewall, que decide se o trafego passa ou nao, IDS/IPS analisa *o que existe dentro do trafego*. Ele nao olha so IP e porta — ele olha comportamento, padrao e conteudo.

Isso muda o nivel de visibilidade.

Pensamento chave:
firewall controla acesso  
IDS/IPS entende intencao  



# IDS vs IPS na pratica

A diferenca entre os dois nao e tecnica, e operacional.

IDS observa e alerta.  
IPS observa e interfere.

Isso significa:

- IDS e ideal para visibilidade e investigacao
- IPS e ideal para bloqueio em tempo real

Mas IPS tem risco:
um falso positivo pode derrubar trafego legitimo

Por isso, em ambientes maduros:
IDS ajuda a entender  
IPS entra depois, quando ha confianca nas deteccoes  



# O que um alerta realmente mostra

Um log de IDS/IPS nao e apenas um evento de rede — ele e uma interpretacao.

Ele ja vem com contexto:
- qual regra disparou (signature_id)
- o que foi detectado (signature)
- classificacao (ex: exploit, scan, malware)
- severidade

Ou seja:
ele ja tenta responder "isso parece ataque?"

Exemplo mental:

firewall:
conexao porta 80

IDS:
possivel scan Nmap usando User-Agent suspeito

Isso reduz muito o tempo de analise.



# Assinatura = conhecimento encapsulado

Cada alerta vem de uma regra.

Essa regra representa:
- comportamento conhecido
- tecnica de ataque
- padrao de malware

Exemplo:
"ET SCAN Nmap Scripting Engine"

Isso nao e so texto — e inteligencia embutida.

O analista nao precisa identificar manualmente  
o IDS ja fez essa primeira camada



# Suricata e o salto para SIEM

Ferramentas modernas como Suricata usam JSON.

Isso muda tudo:

- campos estruturados (src_ip, dest_ip, severity)
- facil ingestao em SIEM
- correlacao direta com outros logs

Comparando:

Snort (texto) → leitura humana  
Suricata JSON → analise automatizada  

Para Blue Team moderno, isso e essencial.



# Tipos de comportamento detectados

IDS/IPS cobre fases inteiras do ataque.

Reconhecimento:
- scans (Nmap, masscan)
- probes estranhos
- user-agents anormais

Exploit:
- tentativas de vulnerabilidade (ex: Log4j)
- shellcode
- payloads maliciosos

Malware / C2:
- beaconing (conexoes periodicas)
- padroes conhecidos de familias (Emotet, TrickBot)

Exfiltracao:
- volume alto de dados
- protocolos usados de forma anormal

Movimento lateral:
- uso interno de SMB, RDP, PsExec fora do padrao

Ou seja:
IDS nao mostra so tentativa de acesso  
mostra comportamento de ataque em andamento  



# Correlacao com outras camadas

Aqui IDS brilha de verdade.

Exemplo completo:

Firewall:
conexao permitida para web server

IDS:
alerta de exploit (ex: Log4j)

Endpoint:
processo suspeito iniciado

→ ataque confirmado

Sem IDS:
firewall diria que foi trafego normal

Outro exemplo:

IDS:
beaconing detectado

Firewall:
conexoes repetidas para mesmo IP

→ possivel C2 ativo

IDS adiciona contexto ao trafego bruto  



# Signature ID como pivô

Cada alerta tem um ID de assinatura.

Isso permite:
- agrupar eventos iguais
- mapear para MITRE ATT&CK
- entender tipo de ataque rapidamente

Analista experiente nao le tudo  
ele pivota pelo signature_id

Isso acelera triagem absurdamente.



# Escrita de regras: deteccao sob medida

Ambientes reais nao dependem so de regras prontas.

Voce pode criar deteccoes especificas:

ex:
detectar user-agent do Nmap  
detectar padrao de DNS exfiltration  

Isso transforma IDS em algo adaptado ao seu ambiente.

Mas tem risco:
quanto mais regras → mais ruido

Por isso entra tuning.



# Ruido e falso positivo

IDS pode gerar MUITOS alertas.

Sem ajuste, vira inutil.

Problemas comuns:
- regras genericas demais
- ambiente nao considerado
- trafego legitimo parecendo ataque

Solucoes:
- suprimir regras irrelevantes
- usar threshold (limite de alertas)
- ajustar com base no comportamento real da rede

Pensamento importante:
IDS sem tuning = alerta inutil  
IDS ajustado = sensor de alta precisao  



# Correlacao temporal

Assim como logs de autenticacao, tempo importa.

Exemplo:

IDS:
alerta de exploit

segundos depois:
endpoint executa processo

→ forte indicio de sucesso do ataque

Se o tempo nao bate:
pode ser falso positivo ou tentativa falha

Por isso:
timestamp alinhado e essencial (NTP novamente)



# Pensamento operacional

IDS/IPS nao substitui firewall nem endpoint.

Ele conecta os dois.

Firewall diz:
"alguem tentou"

Endpoint diz:
"algo rodou"

IDS diz:
"isso parece um ataque conhecido"

Juntos:
formam uma narrativa completa



# Resumo mental

Firewall = controle de acesso  
IDS = interpretacao do trafego  

Assinatura = conhecimento de ataque  
Alerta = suspeita contextualizada  

Scan → reconhecimento  
Exploit → tentativa real  
Beacon → persistencia / C2  

IDS sozinho alerta  
correlacao confirma ataque  

Sem tuning = ruido  
Com tuning = deteccao poderosa


---

# DNS logs e deteccao de comportamento oculto

DNS e um dos pontos mais subestimados em analise de seguranca — mas, na pratica, quase tudo passa por ele primeiro.

Antes de qualquer conexao HTTP, malware ou C2, existe uma consulta DNS.

Pensamento chave:
DNS nao mostra ataque direto  
mostra a intencao antes do ataque  



# DNS como ponto inicial da comunicacao

Quando um host quer se comunicar, ele resolve um dominio.

Isso significa que logs DNS respondem:
- o que o host quer acessar
- quando tentou
- com que frequencia

Diferente de firewall:
que mostra conexao

DNS mostra *interesse*

Exemplo:
host consulta dominio malicioso  
mesmo que conexao nao aconteca  
isso ja e um sinal forte  



# O que realmente importa no log DNS

Um log DNS simples ja traz muito contexto:

- query (dominio)
- IP de origem
- tipo (A, AAAA, TXT)
- resposta (NOERROR, NXDOMAIN)

Mas o valor real nao esta no campo isolado  
esta no padrao de uso



# Comportamento vs evento

Um dominio estranho sozinho nao significa muito.

Mas quando voce ve:

- muitas consultas para o mesmo dominio
- consultas em intervalos regulares
- nomes estranhos ou aleatorios

isso muda o jogo

Exemplo:
consultas a cada 10 segundos para mesmo dominio  
→ forte indicio de beaconing (C2)

Outro:
varios subdominios longos e diferentes  
→ possivel exfiltracao via DNS



# DNS como canal de ataque

Atacantes usam DNS porque ele quase sempre e permitido.

Isso permite tecnicas como:

C2 disfarcado:
malware consulta dominio para receber instrucoes

DNS tunneling:
dados sao enviados dentro do nome do dominio

Exfiltracao:
informacoes sensiveis codificadas em queries

Ou seja:
DNS vira canal de comunicacao invisivel



# Padroes tipicos de ameaca

Alguns comportamentos sao praticamente assinatura de ataque.

Dominios aleatorios:
strings longas sem sentido  
→ possivel DGA (malware gerando dominios)

Alta frequencia:
mesmo dominio repetidamente  
→ beaconing

Subdominios exagerados:
abc123.xyz.c2domain.com  
→ tunelamento

Consultas TXT incomuns:
→ transporte de dados

NXDOMAIN em massa:
→ tentativa de encontrar dominio valido (DGA ativo)



# Correlacao com outras fontes

DNS sozinho levanta suspeita  
correlacao confirma

Exemplo:

DNS:
consulta dominio suspeito

Firewall:
conexao para IP resolvido

Endpoint:
processo desconhecido executando

→ cadeia completa de ataque

Outro:

DNS:
dominio recem-criado

Threat intel:
dominio malicioso conhecido

→ alerta imediato

DNS e muitas vezes o primeiro sinal  
antes de qualquer outra evidência  



# Dominios novos e reputacao

Um dos sinais mais fortes e idade do dominio.

Dominios recem-criados:
- muito usados em phishing
- usados em campanhas de malware

Se um host acessa dominio criado ontem  
isso e altamente suspeito

Aqui entra enriquecimento:
WHOIS + threat intel + passive DNS



# DNS e horario (contexto comportamental)

Horario tambem importa.

Exemplo:
workstation fazendo consultas DNS de madrugada  
→ comportamento anormal

Ou:
pico de consultas fora do padrao  
→ possivel atividade automatizada

DNS ajuda a identificar comportamento fora do perfil normal  



# DNS em endpoint vs rede

DNS pode ser visto em varios lugares:

Resolver:
mostra consultas gerais da rede

Zeek/Suricata:
mostra trafego observado

Endpoint (Sysmon):
mostra qual processo fez a consulta

Esse ultimo e poderoso:

processo + dominio = contexto completo

Exemplo:
powershell.exe fazendo query para dominio suspeito  
→ muito mais forte que apenas o dominio



# SIEM e deteccao

No SIEM, DNS vira dados estruturados:

- dns.query
- source.ip
- response.code

Isso permite detectar:

consultas repetidas  
dominios raros  
dominios suspeitos (.xyz, .top, etc)

E principalmente:
correlacionar com IP, usuario e processo



# Pensamento operacional

DNS nao mostra "exploit"  
mas mostra preparacao e comunicacao

Ele responde:
"o que esse host esta tentando resolver?"

E isso, muitas vezes, revela:
- malware ativo
- tentativa de contato com C2
- exfiltracao silenciosa

Por isso:
DNS e uma das melhores fontes para threat hunting  



# Resumo mental

DNS = intencao antes da conexao  

Consulta estranha = possivel indicio  
Repeticao = comportamento automatizado  
Dominio aleatorio = possivel malware  
Subdominio longo = exfiltracao  

DNS sozinho nao prova ataque  
mas quase sempre aponta onde olhar  

Quem controla e analisa DNS  
ganha visibilidade antes do ataque acontecer


---

# Proxy logs e visibilidade de trafego web

Se o DNS mostra *intencao* e o firewall mostra *conexao*, o proxy mostra *o que realmente foi acessado*.

Ele e o ponto mais detalhado da navegacao web.

Pensamento chave:
DNS resolve  
firewall conecta  
proxy revela conteudo  



# O papel do proxy na seguranca

O proxy fica entre o usuario e a internet.

Isso permite:
- aplicar politicas (bloquear/acessar)
- registrar tudo que foi acessado
- categorizar comportamento

Diferente de outras fontes, aqui voce ve:
- URL completa
- metodo HTTP
- resposta do servidor
- volume de dados

Ou seja:
proxy mostra a *acao real do usuario ou malware*  



# O que realmente importa no log

Um log de proxy e rico, mas o valor esta em correlacionar campos.

Exemplo mental:

IP interno + POST + dominio suspeito + resposta 200  
→ possivel envio de dados para C2

Campos chave nao devem ser vistos isoladamente:
- metodo (GET vs POST)
- URL (path e endpoint)
- status HTTP
- bytes transferidos
- user-agent

Tudo junto forma o comportamento  



# GET vs POST (diferenca critica)

GET geralmente representa acesso:
abrir pagina, baixar conteudo

POST geralmente representa envio:
dados, credenciais, arquivos

Isso e essencial.

Exemplo:
GET em dominio estranho → suspeito  
POST em dominio estranho → muito mais grave  

POST + dominio malicioso = possivel exfiltracao ou C2  



# Proxy como detector de C2

Malware moderno frequentemente usa HTTP/HTTPS.

Isso faz o trafego parecer legitimo.

Mas o proxy revela padrao:

- endpoints como /checkin, /gate.php, /api
- conexoes frequentes e regulares
- user-agent estranho ou falso

Exemplo:
host fazendo POST recorrente para /checkin.php  
→ comportamento tipico de beaconing

Mesmo que o firewall permita  
o proxy expoe o padrao  



# User-Agent como indicador

User-Agent deveria representar navegador.

Mas malware frequentemente:
- usa strings falsas
- nao usa padrao de browser
- ou usa ferramentas (curl, python, etc)

Exemplo:
user-agent vazio ou incomum  
→ forte indicio de script ou malware

Outro caso:
user-agent "Mozilla" mas comportamento nao bate  
→ possivel spoofing  



# Categoria e contexto

Ferramentas de proxy categorizam dominios:
- malware
- phishing
- C2
- social media

Isso ajuda muito na triagem.

Exemplo:
acesso permitido a categoria "Command and Control"  
→ falha de politica + incidente potencial

Mas cuidado:
categoria ajuda, mas nao substitui analise  
dominio novo pode nao estar categorizado ainda  



# Exfiltracao via web

Proxy e uma das melhores formas de detectar exfiltracao.

Padroes comuns:
- uploads grandes (POST/PUT)
- envio para cloud (Dropbox, Mega, etc)
- volume alto de dados para destino incomum

Exemplo:
host enviando 50MB via POST para dominio desconhecido  
→ altamente suspeito

Aqui os campos de bytes fazem diferenca  



# Beaconing e comportamento repetitivo

Assim como DNS, repeticao e chave.

Exemplo:
mesmo host acessando mesma URL a cada X segundos  
→ padrao de malware

Proxy permite ver isso claramente:
mesma URL + mesmo intervalo + mesmo metodo

Isso dificilmente e comportamento humano  



# Correlacao com outras fontes

Proxy fecha o ciclo.

Exemplo completo:

DNS:
resolve dominio suspeito

Firewall:
permite conexao

Proxy:
mostra POST para /checkin

Endpoint:
processo desconhecido ativo

→ ataque confirmado com alta confianca

Sem proxy:
voce nao veria o endpoint da requisicao  



# Tunelamento e evasao

Proxy tambem ajuda a detectar bypass.

Exemplo:
uso de CONNECT para tunel HTTPS
trafego criptografado vindo de agente estranho

Ou:
uso de proxies externos / VPN / Tor

Isso pode indicar:
- tentativa de anonimato
- evasao de controle corporativo  



# Hunting com proxy

Proxy e excelente para threat hunting.

Algumas abordagens:

- dominios raros ou novos
- user-agents nao padrao
- POST para URLs suspeitas
- comportamento repetitivo (beaconing)
- uploads grandes inesperados

Aqui o foco nao e alerta  
e encontrar comportamento fora do normal  



# Pensamento operacional

Proxy responde uma pergunta essencial:

"o que exatamente esse host fez na web?"

Nao so onde conectou  
mas:
- qual URL
- qual metodo
- quanto dado
- qual resposta

Isso transforma suspeita em evidencia  



# Resumo mental

Proxy = visibilidade completa da web  

GET = acesso  
POST = envio (mais critico)  

URL estranha + POST = alerta forte  
Repeticao = beaconing  
Upload grande = exfiltracao  

User-agent estranho = possivel malware  

Proxy nao substitui DNS/firewall  
mas e onde o comportamento fica claro

---

# Logs de Autenticacao e Acesso

Logs de autenticacao e acesso sao o coracao da visibilidade em seguranca. Eles mostram quem entrou, de onde, como e com qual nivel de privilegio. Na pratica, isso cobre desde tentativa de acesso ate movimento lateral e abuso de conta.

O ponto mais importante aqui nao e o log isolado, mas a identidade sendo usada ao longo do ambiente.

Esses logs aparecem em varios lugares ao mesmo tempo: Windows (Security.evtx), Linux (auth.log), Active Directory, VPNs, SSO (Azure AD, Okta) e ate aplicacoes web. O analista precisa enxergar tudo isso como uma unica linha do tempo de identidade.



# O que realmente importa no log

Todo log de autenticacao gira em torno de alguns campos-chave:

- usuario (quem)
- IP de origem (de onde)
- sistema alvo (para onde)
- tipo de autenticacao (como)
- resultado (sucesso ou falha)

Esses campos permitem responder perguntas criticas:

- esse usuario costuma acessar daqui?
- esse IP faz sentido?
- esse tipo de logon e esperado?
- esse comportamento e normal?

Sem isso, nao existe deteccao — apenas registro.



# Falha vs sucesso (o inicio da historia)

Tentativas de login falhadas mostram ataque em andamento. Logins bem-sucedidos mostram risco real.

Mas o que importa e a relacao entre eles.

Padrao classico:
- varias falhas (4625 ou Failed password)
- seguido de sucesso (4624 ou Accepted password)

Isso indica:
- brute force bem-sucedido
- credential stuffing
- credencial vazada sendo usada

Outro padrao importante:
- falhas vindo de varios IPs contra um unico usuario
→ tentativa automatizada de adivinhar senha

Aqui o raciocinio muda:
falha isolada = ruido  
falha repetida + padrao = ataque  



# Tipo de logon e contexto

Nem todo login e igual.

No Windows, o tipo de logon muda completamente o significado:

- tipo 2 → login local
- tipo 3 → rede (SMB, compartilhamento)
- tipo 10 → RDP (remoto)

Exemplo:
usuario fazendo logon tipo 10 de madrugada
→ acesso remoto fora do padrao

Ou:
conta de servico usando logon interativo (tipo 2)
→ comportamento anormal

O contexto do logon e tao importante quanto o evento em si.



# Movimento lateral (quando o ataque se espalha)

Depois que o atacante entra, ele raramente fica parado.

Ele tenta acessar outros sistemas.

Isso aparece como:
- mesmo usuario logando em varios hosts
- varios logons tipo 3 ou 10 em sequencia
- uso de credenciais explicitas (Event ID 4648)

Padrao suspeito:
um usuario acessa 3, 4, 5 maquinas em poucos minutos

Isso dificilmente e comportamento humano normal  
→ forte indicio de movimento lateral



# Escalada de privilegio via autenticacao

Autenticacao nao e so login — tambem revela aumento de privilegio.

Exemplo no Windows:
- 4624 (login)
- seguido de 4672 (privilegios especiais)

Isso mostra que a sessao ganhou privilegios administrativos.

No Linux:
- login SSH
- seguido de uso de sudo

Padrao perigoso:
usuario comum → acesso → privilegio elevado

Isso indica pos-exploracao ativa.



# Padroes anormais de acesso

Blue Team nao procura apenas ataque direto, mas comportamento estranho.

Alguns exemplos fortes:

- login fora do horario (madrugada, fim de semana)
- login de pais diferente (GeoIP)
- login em sistema nunca acessado antes
- MFA falhando repetidamente
- conta desativada tentando autenticar

Outro caso importante:
"impossible travel"

usuario loga no Brasil  
minutos depois loga na Europa  

→ impossivel fisicamente  
→ credencial comprometida



# Contas sensiveis e alvo prioritario

Nem todas as contas tem o mesmo peso.

Contas como:
- admin / administrator
- root
- contas de servico

devem ser monitoradas com mais rigor.

Padroes suspeitos:

- login direto como admin
- uso frequente de contas privilegiadas
- criacao de novas contas (4720)
- adicionando usuario em grupo admin (4732)

Aqui o risco e alto mesmo com poucos eventos.

---

## Fundamentos de Análise de Log: Pivotando em Fontes de Log

* **Conceito de Pivotagem:** Técnica de investigação que utiliza um dado inicial (IP, Hash, User) para rastrear atividades em diferentes fontes de logs, conectando estágios de um ataque (C2, Movimentação Lateral, Exfiltração).

### Principais Fontes e Elementos de Pivô
| Fonte | Elementos-Chave para Conexão |
| :--- | :--- |
| **DNS** | Domínios consultados e IPs de clientes. |
| **Proxy** | URLs, Métodos (GET/POST) e User-Agents. |
| **EDR** | Hashes de arquivos e Árvore de Processos (Pai-Filho). |
| **Autenticação** | Usernames, Logon Types (Ex: Tipo 10 - RDP) e IPs de origem. |
| **Firewall** | IPs de Origem/Destino e Portas de comunicação. |

### Fluxos de Investigação (Exemplos Práticos)
* **Domínio Suspeito via DNS:**
    1.  **Proxy:** Confirmar comportamento (GET/POST) e anomalias de User-Agent.
    2.  **EDR:** Identificar qual processo (ex: `powershell.exe`) disparou a consulta.
    3.  **Autenticação:** Identificar o usuário logado no host no momento do evento.
* **Execução Suspeita (PowerShell):**
    1.  **Árvore de Processo:** Verificar se o pai foi um binário inesperado (ex: `winword.exe`).
    2.  **Telemetria EDR:** Buscar por arquivos criados ou tarefas agendadas (`Persistence`).
    3.  **Rede:** Verificar conexões de saída imediatas após a execução.

### Técnicas de Pivotagem por Atributo
* **Baseada em IP:** Rastreia comunicações no Firewall/Proxy e busca resoluções DNS reversas.
* **Baseada em Hash:** Localiza a propagação de um arquivo malicioso (DLL/EXE) em todos os endpoints da rede.
* **Baseada em Usuário:** Monitora escalada de privilégios e logins simultâneos em múltiplos sistemas.
* **Comportamento de Beacon:** Identifica padrões temporais de saída no Proxy correlacionando com processos no EDR.

### Consultas e Regras Técnicas
* **Splunk (SPL):** `index=proxy OR index=firewall | search src_ip="10.10.5.12" | stats values(uri) by src_ip, user`
* **Sigma (Lógica):** Detectar `powershell.exe` seguido de resolução DNS de domínio suspeito em um intervalo de 5 minutos.

### Erros Críticos na Análise
* **Ignorar Timestamps:** Correlacionar eventos de tempos distintos gera falsos positivos.
* **Desconsiderar NAT:** IPs de origem podem ser mascarados por proxies, levando a atribuições erradas.
* **Falha em Sessões:** Não rastrear IDs de sessão em sistemas multiusuário quebra a linha do tempo.
* **Negligenciar Falhas de Login:** Ignorar `Logon Failures` esconde ataques de Brute Force ou Password Spraying.

---

## 4.3 Construção de Playbooks de Threat Hunting Baseados em Hipóteses

* **Definição:** O *Threat Hunting* é uma atividade proativa e orientada por inteligência. Diferente da triagem de alertas (reativa), ele busca comportamentos adversários que evadem detecções baseadas em assinaturas.
* **Playbook de Caça:** Um processo documentado, repetível e estruturado que transforma uma suposição teórica em uma investigação prática.



### A Hipótese de Caça
Uma hipótese é uma declaração testável sobre uma técnica ou comportamento atacante no ambiente.
* **Exemplos:** * "Um adversário estabeleceu persistência via tarefas agendadas."
    * "Há tráfego de *beaconing* via HTTP a cada 60 segundos."
* **Fontes para Hipóteses:** MITRE ATT&CK (ex: T1053), Threat Intelligence (IoCs/TTPs de grupos específicos), Lacunas de Detecção (ex: falta de alertas para LOLBins) e Desvios de Linha de Base (Spikes de autenticação).

### Estrutura Passo a Passo do Playbook

| Etapa | Ação Técnica |
| :--- | :--- |
| **1. Definir Hipótese** | Ex: Persistência via `schtasks.exe` em endpoints com logins VPN recentes. |
| **2. Definir Objetivos** | Identificar criação manual de tarefas, vincular ao usuário e detectar execução. |
| **3. Identificar Fontes** | Windows Event Logs (ID 4698), EDR (Pai-Filho), Logs de VPN e Proxy/DNS. |
| **4. Criar Consultas** | **Splunk:** `index=windows EventCode=4698 CommandLine="*schtasks*"`<br>**KQL:** `event.action : "scheduled-task-created" AND process.name : "schtasks.exe"` |
| **5. Correlação** | Vincular Login VPN (IP remoto) -> Criação de Tarefa (15 min) -> Execução (5 min). |
| **6. Documentar** | Capturar hashes e comandos; converter em regra Sigma se o comportamento for confirmado. |

### Exemplo Prático: Exfiltração via PowerShell
* **Hipótese:** "Host comprometido exfiltrando dados via PowerShell codificado sobre HTTP."
* **Plano de Execução:**
    1.  **Proxy:** Buscar POSTs grandes/frequentes para domínios desconhecidos.
    2.  **User-Agent:** Filtrar por `Microsoft.PowerShell/5.1`.
    3.  **EDR:** Pivotar para confirmar uso de `Invoke-WebRequest` ou `Invoke-Expression`.
    4.  **Persistência:** Verificar tarefas agendadas ou WMI no mesmo host.

### Modelo de Playbook (Template)
```
# Playbook: Beaconing via DNS
- **Hipótese**: Adversários utilizam DNS para exfiltração ou C2.
- **Técnicas ATT&CK**: T1048.003 (Exfiltration), T1071.004 (DNS).
- **Logs Necessários**: DNS, EDR, Firewall.
- **Queries**: Frequência alta para um único domínio; domínios com > 50 caracteres.
- **Ações de Resposta**: Isolar host, extrair memória, bloquear domínio.
```

---

## 4.4 Caça ao Movimento Lateral e Persistência

* **Objetivo:** Detectar a progressão do atacante após o acesso inicial. O foco é identificar o salto entre sistemas (Movimento Lateral) e a criação de mecanismos de sobrevivência a reinicializações (Persistência).
* **Desafio Técnico:** Diferenciar comandos administrativos legítimos de atividades maliciosas através da correlação de múltiplas camadas de logs.



### Técnicas Críticas e Identificadores (ATT&CK)

| Categoria | Técnica | ID | Indicador de Log |
| :--- | :--- | :--- | :--- |
| **Movimento Lateral** | **RDP Interno** | T1021.001 | Event ID 4624 (Logon Type 10). |
| | **Pass-the-Hash** | T1550.002 | Event ID 4624 (Logon Type 9 / Logon Process: Seclogo). |
| | **SMB/PsExec** | T1569.002 | Event ID 7045 (Novo Serviço instalado: `PSEXECSVC`). |
| | **WMIC** | T1047 | Processo `wmic.exe` com argumento `/node:`. |
| **Persistência** | **Scheduled Tasks** | T1053 | Event ID 4698 (Criação) e 4699 (Deleção). |
| | **Registry Run Keys** | T1547.001 | Sysmon Event ID 13 (RegistryValueset) em `...\Run` ou `...\RunOnce`. |
| | **WMI Event Sub.** | T1546.003 | Sysmon Event ID 19, 20, 21 (WMIEvent). |

### Metodologia de Investigação e Correlação

Para uma caça eficiente, é necessário cruzar dados de diferentes fontes em uma janela temporal curta.

#### Exemplo de Cadeia de Eventos (Pivô):
1.  **Logon (Host A):** Sucesso de login via RDP (ID 4624, Type 10) vindo de um IP incomum.
2.  **Execução (Host B):** Uso de `wmic.exe` para criar um processo em uma máquina alvo (`/node:10.0.4.15`).
3.  **Persistência (Host B):** Criação imediata de uma tarefa agendada (ID 4698) executando um binário em `C:\Users\Public\`.



### Consultas de Caça (Hunting Queries)

* **Splunk - Anomalia Pai-Filho (Explorer disparando WMIC):**
```splunk
index=sysmon OR index=edr
| transaction host startswith=(process_name="explorer.exe") endswith=(process_name="wmic.exe")
| stats count by host, user, parent_process, child_process
```
---

````
## 4.5 Engenharia de Detecção: Sigma, KQL, SPL e Consultas Personalizadas

* **Definição:** A Engenharia de Detecção é o processo de transformar inteligência de ameaças e hipóteses de caça em regras automatizadas. Ela preenche a lacuna entre o *Threat Hunting* proativo e a resposta a incidentes.
* **Linguagens de Consulta:** Fluência em diferentes sintaxes (Sigma, KQL, SPL) permite que a Blue Team crie detecções portáteis e eficazes em diversos SIEMs e EDRs.



### 1. Regras Sigma (O Padrão Genérico)
O Sigma funciona como um "tradutor" universal para regras de detecção. Escrito em YAML, pode ser convertido para Splunk, Elastic, Sentinel, entre outros.

**Exemplo: Execução de PowerShell Suspeito**
```yaml
title: Suspicious PowerShell Execution
logsource:
  product: windows
  category: process_creation
detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains:
      - ' -enc '
      - ' -EncodedCommand '
  condition: selection
level: high
````

### 2. KQL (Microsoft Sentinel / Defender)

Linguagem otimizada para logs de segurança da Microsoft e dados de séries temporais.

**Exemplo: Detecção de Carga Útil Base64**

Snippet de código

```
DeviceProcessEvents
| where FileName =~ "powershell.exe"
| where ProcessCommandLine has_any (" -enc ", " -EncodedCommand")
| project Timestamp, DeviceName, InitiatingProcessAccountName, ProcessCommandLine
```

### 3. SPL (Splunk Search Processing Language)

Utilizada para buscas rápidas e correlações complexas em ambientes Splunk.

**Exemplo: Correlação de Tarefa Agendada + Execução (Janela de 15min)**

Snippet de código

```
index=wineventlog EventCode=4698 OR EventCode=4688
| eval type=case(EventCode==4698,"Created", EventCode==4688,"Exec")
| transaction host startswith=(type="Created") endswith=(type="Exec") maxspan=15m
| table _time, host, user, CommandLine, type
```

### 4. Mapeamento com Frameworks (MITRE ATT&CK)

A detecção deve ser vinculada a Táticas, Técnicas e Procedimentos (TTPs) para garantir cobertura estratégica.

|Técnica|Sigma (Lógica)|KQL (Tabela)|SPL (Índice)|
|---|---|---|---|
|**T1059 (Scripting)**|`category: process_creation`|`DeviceProcessEvents`|`index=sysmon`|
|**T1569.002 (PsExec)**|`Image|endswith: 'psexec.exe'`|`where FileName == "psexec.exe"`|

### Dicas para Engenharia de Detecção Eficaz

- **Validação:** Teste as regras com ferramentas de simulação de ataque (ex: Atomic Red Team).
    
- **Fidelidade:** Priorize regras com baixo índice de falso positivo (alta fidelidade).
    
- **Métricas:** Marque cada regra com o ID da técnica MITRE correspondente.
    
- **Versionamento:** Utilize repositórios Git para controle de versão das regras de detecção.
    

### Resumo de Benefícios

1. **Portabilidade:** Regras Sigma podem ser movidas entre diferentes plataformas.
    
2. **Precisão:** Consultas específicas reduzem a fadiga de alertas.
    
3. **Agilidade:** Acelera a resposta a incidentes com alertas contextuais e precisos.

---

## 4.6 Integrando Inteligência de Ameaças (TI) em Correlação

* **Conceito:** A Inteligência de Ameaças (Threat Intelligence) fornece o "porquê" por trás dos dados dos logs. Ela enriquece os eventos com contexto externo (IOCs e TTPs), permitindo priorização e resposta acelerada.
* **Objetivo:** Reduzir o *Dwell Time* (tempo de permanência do invasor) e guiar caçadas proativas baseadas em infraestrutura adversária conhecida.



### Categorias de Inteligência de Ameaças

| Tipo | Foco Técnico | Aplicabilidade em Logs |
| :--- | :--- | :--- |
| **Tática (IOCs)** | IPs, Hashes, Domínios, User-Agents. | Correspondência direta em tempo real (SIEM). |
| **Operacional** | TTPs, Campanhas, Famílias de Malware. | Guia para Playbooks de Hunting. |
| **Estratégica** | Tendências de atores e motivações. | Planejamento de longo prazo e governança. |
| **Técnica** | Artefatos de engenharia reversa. | Detecção de beacons e padrões de rede. |

### Fontes e Tipos de Indicadores (IOCs)

* **Fontes:** Open Source (Abuse.ch, AlienVault OTX), Comerciais (Recorded Future), Internas (Incidentes passados) e ISACs (Setoriais).
* **Mapeamento de Logs:**
    * **IPs:** Firewall, Proxy, NetFlow.
    * **Domínios/URLs:** DNS, Web Proxy.
    * **Hashes/Nomes de Arquivo:** EDR, Sysmon, AV.

### Técnicas de Integração e Correlação

#### 1. IOC Matching (Tempo Real)
O SIEM cruza logs de entrada com feeds de inteligência automaticamente.

* **KQL (Sentinel):**
```kql
let bad_ips = externaldata(ip:string) [@"[https://threatfeed.io/badips.csv](https://threatfeed.io/badips.csv)"];
DeviceNetworkEvents
| where RemoteIP in (bad_ips)

    Splunk (ThreatMatch):

Snippet de código

index=proxy_logs
| lookup ti_bad_ips_lookup RemoteIP as dest_ip OUTPUT threat_info
| where isnotnull(threat_info)

2. Enriquecimento de Alertas

Adiciona metadados ao alerta disparado para facilitar a triagem.

    Exemplo: Um alerta de DNS para um domínio suspeito é enriquecido com: “Domínio C2 vinculado ao APT37 (AsyncRAT), ativo desde Nov/2023.”

3. Hunting Orientado por TI

Utiliza relatórios de campanhas (ex: TA551/Ursnif) para buscar comportamentos específicos, como documentos Office disparando cmd.exe ou powershell.exe.
Ferramentas de Suporte

    MISP (Open Source): Plataforma para compartilhamento e gerenciamento de IOCs.

    OpenCTI: Analisa relações entre ameaças e indicadores.

    SOAR: Automatiza o enriquecimento de alertas durante a execução de playbooks.

Melhores Práticas

    Qualidade vs. Quantidade: Feeds ruidosos geram fadiga de alertas; valide a fidelidade da fonte.

    Pontuação de Confiança: Use níveis de confiança para decidir se um alerta deve ser crítico ou apenas informativo.

    Normalização: Garanta que os logs e os feeds de TI usem o mesmo formato (ex: hashes em SHA256) para evitar falhas na correlação.
```

---

## Aula 01: Introdução ao SIEM e ELK Stack

* **SIEM (Security Information and Event Management):** Uma solução que combina **SIM** (armazenamento e análise de longo prazo) e **SEM** (monitoramento e correlação em tempo real).
* **Objetivo:** Centralizar logs, normalizar dados e gerar alertas proativos sobre incidentes de segurança.



### A Stack ELK (Elasticsearch, Logstash, Kibana)
O ELK é uma plataforma de gerenciamento de logs composta por três pilares, frequentemente alimentada por **Beats** (agentes coletores).

| Componente | Função Principal | Analogia |
| :--- | :--- | :--- |
| **Beats** | Coletores leves (Filebeat, Winlogbeat) instalados na origem. | O Mensageiro. |
| **Logstash** | Processamento, filtragem e enriquecimento de dados. | A Refinaria. |
| **Elasticsearch** | Motor de busca e indexação distribuída (JSON). | O Cérebro/Arquivo. |
| **Kibana** | Interface de visualização, dashboards e alertas. | O Painel de Controle. |



### Conceitos Chave do Elasticsearch
* **Index:** Coleção de documentos com características similares (ex: `firewall-logs-*`).
* **Document:** A unidade básica de informação em formato JSON.
* **Shard/Replica:** **Shard** é a subdivisão do índice para ganho de performance; **Replica** é a cópia para redundância e alta disponibilidade.

### Logstash: O Pipeline de Dados
O processamento ocorre em três etapas dentro do arquivo de configuração:
1.  **Input:** De onde os dados vêm (ex: porta 5044 para Beats).
2.  **Filter:** Onde a mágica acontece (Grok para parsear texto, GeoIP para localizar IPs).
3.  **Output:** Para onde os dados vão (geralmente para o Elasticsearch).

### ECS (Elastic Common Schema)
O **ECS** é uma especificação que padroniza os nomes dos campos.
* **Por que usar?** Garante que logs de um firewall Cisco e de um Windows Event Log usem os mesmos campos (ex: `source.ip`), permitindo que um único dashboard visualize ambos simultaneamente.



### Fontes de Dados Críticas

| Categoria | Exemplo de Fonte | Valor para Segurança |
| :--- | :--- | :--- |
| **Rede** | Firewall, IDS/IPS, DNS. | Detecção de C2 e exfiltração. |
| **Endpoint** | Sysmon, WinEventLog, Auditd. | Movimentação lateral e execução de malware. |
| **Aplicação** | Web Server Logs (Nginx/Apache). | Ataques de SQLi e XSS. |
| **Nuvem** | CloudTrail (AWS), Activity Logs. | Acesso não autorizado a APIs. |

### Exercício de Fluxo: Ataque Brute Force SSH
1.  **Source:** O servidor registra falhas de login no `/var/log/auth.log`.
2.  **Filebeat:** Lê o arquivo e envia para o Logstash.
3.  **Logstash:** Extrai o IP de origem, aplica GeoIP (identifica a origem na China) e marca o evento como `event.outcome: failure`.
4.  **Elasticsearch:** Armazena o JSON indexado.
5.  **Kibana:** O analista visualiza um pico de falhas no dashboard e o alerta de SIEM é disparado.

---

### Perguntas de Revisão
1.  **Quais os 3 componentes do ELK e seus papéis?** (E: Busca, L: Processamento, K: Visualização).
2.  **Por que normalizar logs?** Para permitir buscas e correlações uniformes entre fontes diferentes.
3.  **Qual a diferença entre Shard e Replica?** Shard divide o dado para velocidade; Replica copia o dado para segurança.

---

## Lição 03: Investigação de Incidentes e Fluxos de Trabalho no ELK

* **Objetivo:** Transformar dados brutos em uma linha do tempo de ataque (Kill Chain). A investigação no ELK foca em correlação de eventos, análise de comportamento e isolamento de indicadores (IOCs).
* **Fluxo de Trabalho:** Inicia na detecção (Alerta), passa pela análise ad-hoc (Discover) e termina na visualização do impacto (Dashboards/Timeline).



### 1. Análise de Linha do Tempo (Timeline)
A investigação eficaz reconstrói os passos do atacante cronologicamente. No Kibana, o campo `@timestamp` é o eixo principal.

**Passos para Investigação de Endpoint:**
1.  **Identificar o Alerta:** Ex: Execução de `powershell.exe` com comando codificado.
2.  **Filtrar por Host e Usuário:** `host.name: "WIN-01" AND user.name: "suporte"`.
3.  **Expandir a Janela de Tempo:** Olhar 5 minutos antes e depois do evento para identificar o "Pai" do processo.
4.  **Verificar Conexões de Rede:** `process.name: "powershell.exe" AND event.category: "network"`.

### 2. Correlação de Eventos entre Fontes
A força do SIEM está em unir logs de diferentes origens para confirmar uma suspeita.

| Evento A (Network) | Evento B (Endpoint) | Conclusão |
| :--- | :--- | :--- |
| Conexão na porta 4444 (Firewall) | `nc.exe` em execução (Sysmon) | Possível Reverse Shell ativo. |
| Pico de 404 Not Found (Proxy) | `dirbuster` ou `gobuster` (WAF) | Varredura de diretórios (Recon). |
| Login VPN às 03:00 AM (VPN) | `mimikatz.exe` detectado (EDR) | Conta comprometida / Mov. Lateral. |



### 3. Técnicas de Investigação Ad-Hoc (KQL/Lucene)

#### Investigando Movimentação Lateral
Para encontrar um atacante pulando de uma máquina para outra via RDP:
* **KQL:** `event.code: 4624 AND winlog.logon.type: 10`
* **Próximo Passo:** Filtrar pelo `source.ip` para ver quais outros hosts essa mesma conta acessou no mesmo período.

#### Investigando Persistência
Busca por modificações suspeitas em chaves de inicialização:
* **KQL:** `registry.path: "*\\CurrentVersion\\Run*" AND NOT process.executable: "C:\\Program Files\\*"`
* **Análise:** Qualquer binário executado fora de diretórios protegidos (ex: `\Temp\` ou `\Public\`) é altamente suspeito.

### 4. Dashboards de Resposta a Incidentes
Dashboards não são apenas para monitoramento, mas para triagem rápida durante um incidente.

* **Filtros Globais:** Aplicar um filtro de IP suspeito no dashboard e ver instantaneamente todos os usuários e hosts associados.
* **Visualizações Úteis:**
    * **Sankey Diagrams:** Visualizar o fluxo de tráfego de rede (Origem -> Destino).
    * **Heatmaps:** Identificar horários anômalos de login.



### 5. Melhores Práticas de Investigação
1.  **Preserve as Evidências:** Não altere o estado do host antes de coletar os logs necessários.
2.  **Use o ECS:** Sempre utilize campos do *Elastic Common Schema* para que suas queries funcionem em qualquer log normalizado.
3.  **Documente o Pivot:** Cada vez que você pular de um IP para um processo, ou de um processo para um arquivo, anote a relação.
4.  **Crie Regras de Detecção:** Se uma investigação manual encontrou um ataque, transforme essa query em um alerta automático.

---

### Exercício de Laboratório: "O Usuário Curioso"
**Cenário:** O EDR alertou sobre o uso de `certutil.exe` para baixar um arquivo.
1.  **Busca:** `process.name: "certutil.exe" AND process.command_line: "*urlcache*"`
2.  **Correlação:** Pegue o IP de destino no comando e pesquise nos logs de Firewall/Proxy.
3.  **Impacto:** Verifique se esse IP está associado a algum feed de *Threat Intelligence* (Lição 4.6).

---

## 5.3 Investigando Incidentes de Segurança com ELK

A investigação no ELK é um processo iterativo que transforma alertas isolados em uma narrativa completa de ataque. O objetivo é responder às perguntas fundamentais: **O que, Quem, Quando, Onde e Como.**

### 1. Fluxo de Trabalho de Investigação (Workflow)

Uma investigação estruturada segue quatro fases principais antes da documentação final:



1.  **Detect (Detectar):** O gatilho inicial (Alerta de SIEM ou anomalia em dashboard).
2.  **Triage (Triagem):** Validação da severidade e descarte de falsos positivos.
3.  **Analyze (Analisar):** Expansão do escopo usando pivotação e correlação.
4.  **Contain (Conter):** Ações imediatas como isolamento de host ou bloqueio de IPs.

---

### 2. Técnicas de Pivotação (Pivot Points)

Pivotar é usar uma evidência confirmada para encontrar eventos relacionados em outras fontes de dados.

| Ponto de Partida | Pivote Para... | Exemplo de Consulta (KQL) |
| :--- | :--- | :--- |
| **Endereço IP** | Sessões, DNS e Firewall | `source.ip: "10.0.0.6" OR destination.ip: "10.0.0.6"` |
| **User Name** | Atividade total do usuário | `user.name: "admin"` |
| **Processo** | Relação Pai/Filho | `process.parent.name: "cmd.exe"` |
| **Hash de Arquivo** | Outros hosts afetados | `file.hash.sha256: "abc123..."` |



---

### 3. Análise de Linha do Tempo (Timeline)

A construção da linha do tempo é essencial para entender a ordem dos eventos. No Kibana, isso é feito organizando os logs pelo campo `@timestamp`.

**Exemplo de Cronologia de Ataque (Brute Force):**

| Timestamp | Fonte | Evento | Detalhes |
| :--- | :--- | :--- | :--- |
| 10:08:00 | access.log | Web Scan | Busca por `/wp-login.php` (404) |
| 10:15:10 | auth.log | SSH Fail | Falha de senha para usuário `admin` |
| 10:15:20 | firewall.log | Blocked | Firewall bloqueia IP após múltiplas falhas |



---

### 4. Correlação Multi-Fonte

A correlação conecta pontos entre diferentes camadas (Rede, Endpoint, Autenticação) para revelar a história completa.

* **Cenário:** Um login bem-sucedido vindo de um IP que acabou de realizar um scan de portas.
* **Lógica de Correlação:** `SAME IP + SAME TIME` entre `firewall.log` e `auth.log`.

**Consulta de Agregação (DSL) para Correlação:**
```json
{
  "aggs": {
    "eventos_por_categoria": {
      "terms": { "field": "event.category" },
      "aggs": {
        "acoes_detalhadas": {
          "terms": { "field": "event.action" }
        }
      }
    }
  }
}
```






