Data: 02/10/2026 Módulo: introdução às investigações, antes dos 3 ride-alongs e da avaliação final. Ferramentas que vão aparecer: Splunk e Splunk Enterprise Security.

## Por que investigar importa

Para se recuperar de um incidente, primeiro é preciso saber se ele aconteceu. Só depois dá para medir o tamanho do estrago e reagir. Por isso a investigação é a base de contenção, erradicação e recuperação.

Não existe uma metodologia ou ferramenta única que resolva todos os casos. O analista combina ferramentas, método, experiência e até instinto. Daí o nome "arte da investigação".

## Frameworks de resposta a incidentes

Os dois mais citados são o SANS (6 etapas) e o NIST (4 fases). Cada organização adapta o seu plano, mas os elementos se repetem.

SANS, na ordem: preparação, identificação, contenção, eradicação, recuperação e lições aprendidas.

A preparação vem antes do incidente: plano de resposta, papéis e responsabilidades, canais de comunicação definidos. Os detalhes das outras etapas estavam em seções expansíveis que não vieram no material, então vale revisar na plataforma.

## Pensamento crítico

Segundo a Robin, não se aprende em sala de aula, se desenvolve com prática. Os pontos que ficaram comigo:

Entender um achado vai além da ação em si. Um alerta de download de arquivo malicioso exige pensar no impacto, em quem pode ser afetado e em ligações com outros achados que parecem não ter relação.

Manter a mente aberta e deixar vieses de lado. Assumir para onde a investigação vai logo no começo faz perder informação importante.

Uma técnica útil é tentar provar que eu estou errado. Se estou convencido de algo mas não acho evidência, procuro evidência de que não é verdade.

Saber interpretar os achados para largar becos sem saída e não gastar tempo à toa. E saber quando pedir ajuda, principalmente quando algo é sério e precisa de todo mundo.

## Como uma investigação começa

Os engenheiros de detecção criam regras e correlation searches no SIEM. Quando algo dispara, nasce um alerta, ticket ou Notable Event. Os nomes mudam por ferramenta, então o termo genérico é "finding".

Triagem é passar pelos findings, investigar e decidir: fechar ou escalar para incidente. No dia a dia são vários ao mesmo tempo, então priorizar é parte do trabalho. Por enquanto o foco é separar verdadeiro positivo de falso positivo.

Quanto mais contexto o alerta traz (usuário, dispositivo, aplicação, número de tentativas, localização), mais rápido essa separação sai. Exemplo do material: várias falhas de login podem ser alguém errando a senha nova ou uma tentativa de comprometer credenciais.

## As 5 perguntas da Robin

Vêm do blog Enterprise Detection & Response, do David Bianco. A ordem pode variar conforme o alerta.

1. Foi um ataque de verdade?
2. O ataque teve sucesso?
3. Quais ativos foram comprometidos?
4. O que o atacante fez?
5. Como a organização deve responder?

Sobre a 2: a maioria das tentativas de exploração na internet é scan, sonda ou worm automatizado que falha. Mas é preciso responder rápido, porque contenção só começa depois disso. No exemplo das falhas de login, o passo seguinte seria checar se houve um login bem sucedido depois e se ele foi malicioso.

Sobre a 3: ativo é qualquer coisa alvo do ataque (arquivos, credenciais, e-mails, dispositivos), e também o que estiver ligado à origem do ataque. A lista cresce e encolhe durante a investigação. Anotar por que e quando cada ativo entrou ou saiu.

Sobre a 4: montar uma linha do tempo dos eventos relevantes, do primeiro exploit até ter certeza de que está tudo mapeado. Ela também muda ao longo do caso. Perguntas guia: comprometeu conta? Escalou privilégio? Instalou ferramentas? Se movimentou na rede?

Sobre a 5: o analista nem sempre participa da limpeza, mas a linha do tempo, o alcance e os ativos afetados dão à equipe o que ela precisa. Isso define a criticidade e quem deve ser envolvido, de RH a jurídico e TI.

## Melhoria contínua

Findings com falso positivo em excesso causam fadiga de alerta e escondem eventos importantes. O caminho é conversar com os engenheiros para ajustar as detecções e incluir nos alertas os dados que o analista sempre acaba buscando. Também entra atualizar playbooks com o que foi aprendido.

# Anotações: Ride-alongs de investigação (Frothly, Splunk T-shirt e Shadow Copy)

Data: 05/10/2026 Ferramentas: Splunk Enterprise 8.2.2 e Splunk Enterprise Security. Nome de descoberta sugerido pelo curso: "Frothly Insider Investigations".

## Contexto do cliente

A Frothly Beverages cresceu comprando duas empresas menores: a Thirsty Berner Brewing (kombucha) e a Yellow Talon (ginger ale e cream sodas). Agora tem leitores de crachá em todas as unidades e VPN para quem trabalha remoto ou híbrido, e o Splunk recebe tudo pelo Wonderland SOC.

A CEO, Grace Hopper, pediu três investigações: suprimentos sumindo (segurança física), acesso suspeito ao Salesforce (acesso remoto) e alertas de alteração de arquivos em estações de trabalho.

## Revisão de insider threat

Insider é qualquer pessoa ou sistema com acesso autorizado: funcionário atual ou antigo, terceirizado, até processos. Nem sempre há má intenção. Acidentes e políticas de segurança confusas também contam.

Indicadores que valem mais quando correlacionados: variação de padrão de login (frequência, horário, remoto ou local), login durante férias, envio de arquivos sensíveis por e-mail, tráfego de saída incomum, aumento de uso de impressora e exportação de relatórios grandes.

Dados não técnicos também ajudam, como violações de AUP, acesso físico e relatórios de despesa de cartão. Às vezes eles nem estão no Splunk, mas podem ser carregados numa instância local para cruzar com os logs.

## 1.1 Segurança física

Origem: inventário de fim de julho apontou material faltando num armário de suprimentos, reportado pelo CTO Fyodor Malteskesko.

Sourcetype dos crachás: `st_frothly_events`. Campos que olhei primeiro: `reader_desc` (qual leitor, em qual local) e `event_desc` (acesso concedido, negado e outros status). Os "interesting fields" do Splunk são os que aparecem em pelo menos 20% dos eventos.

## 1.2 Acesso remoto (Richard Schlitzer)

Richard é gerente regional de vendas, viaja bastante e normalmente fica no norte da Califórnia. Ele diz que não logou no Salesforce enquanto estava no México. A conta dele já foi comprometida outras vezes.

Primeiro achado: 18 IPs diferentes em uma semana na VPN (Checkpoint, sourcetype `cp_log`, usuário `richards`).

```spl
index=main sourcetype="cp_log" user=richards
```

Para ver a localização dos IPs, `iplocation` gera cidade, região, país, lat e long a partir de uma base de terceiros.

```spl
index=main sourcetype="cp_log" user="richards"
| iplocation src
| where City!=""
| table src City Region Country lat long _time
| dedup src
| sort _time
```

O resultado mostra o caminho da Califórnia descendo até o México, coerente com a viagem. Os nomes de usuário mudam entre sistemas: no Salesforce é `richard@yellowtalon.co`, o sourcetype é `sfdc_streaming_api_events://login_events` e o campo de IP se chama `SourceIp`, não `src`.

```spl
index=main source="sfdc_streaming_api_events://login_events" Username="richard@yellowtalon.co"
| iplocation src
| where City!=""
| table _time Username SourceIp City
| dedup _time
| sort -_time
```

Observação sobre o material: na busca com `geostats` o texto traz `yellowtalon.com` e `sfdc_streaming_api_events:///login_events` (três barras). Parece erro de digitação, o correto nas outras buscas é `.co` e duas barras. Vale conferir antes de copiar.

O mapa com `geostats count by City latfield=lat longfield=lon` não mostrou nada estranho. A virada veio da tabela combinada, que normaliza os campos com `coalesce`:

```spl
index=main (source="sfdc_streaming_api_events://login_events" OR sourcetype="cp_log") (Username="richard@yellowtalon.co" OR user=richards)
| eval src=coalesce(src,SourceIp)
| eval user=coalesce(Username, user)
| iplocation src
| eval State=coalesce(Region, Subdivision)
| where City!=""
| table _time user src City State Country
| dedup _time
| sort -_time
```

Pontos que precisei guardar: uso `OR` e não `AND` porque os dois eventos são de fontes diferentes, então um mesmo evento nunca teria as duas coisas. O `dedup _time` limpa duplicidade dos logs de VPN. Conclusão: a credencial do Richard provavelmente foi comprometida, ou ele está compartilhando o acesso com alguém.

Lição de método: o mapa bonito não respondeu nada, a tabela correlacionada sim. Vale mudar a forma de olhar o dado quando uma visualização não ajuda.

## 1.3 Atividade de arquivos (DTEX)

DTEX InTERCEPT é um agente focado em insider threat que gera telemetria de endpoint parecida com Sysmon. O SOC usa o add-on do Splunkbase para coletar os eventos.

Busca inicial, ampla de propósito, porque ainda não sabíamos o que procurar:

```spl
index=dtex sourcetype=dtex_st_activities Activity_Group=FileSystemActivity
```

Ao adicionar `Source_File_Extension` aos campos selecionados apareceu a extensão `.lockbit`. Pesquisando (OSINT), é ransomware LockBit. Variantes: a primeira usa `.abcd` e deixa o `Restore-My-Files.txt` em cada pasta, a segunda usa `.LockBit` e é quase igual, a terceira não exige mais o navegador Tor nas instruções de resgate. Aqui seria a variante 2 ou 3, e só com a nota de resgate dá para decidir.

Dica da Robin: `stats values()` lista todos os valores de um campo numa linha só. A ordem é lexicográfica (UTF-8).

```spl
index=dtex sourcetype=dtex_st_activities Activity_Group=FileSystemActivity Source_File_Extension=lockbit
| stats values(Activity_Details)
```

Para saber quantas máquinas foram afetadas:

```spl
index=dtex sourcetype=dtex_st_activities Activity_Group=FileSystemActivity Source_File_Extension=lockbit
| stats count by Device_Name
```

Só um host apareceu: o do Richard. Ou seja, não era insider threat, era ransomware. Próximo passo é avisar o time de resposta a incidentes e a Frothly para coordenar contenção e recuperação antes de espalhar.

## Ride-along da Ashley: Splunk T-shirt company

Finding no Splunk ES: "Brute force access Behavior detected from 172.16.16.245". Perguntas para triar: o que o finding deveria detectar, quais ativos e identidades estão ligados e se há informação suficiente para confirmar um incidente.

Pelo quiz, o comportamento (várias falhas de login com alguns sucessos no meio, vindos de um host) combina com password spray ou dictionary attack. Rainbow table e pass the hash trabalham com senhas em hash, então não se encaixam. Técnicas MITRE mapeadas pela engenharia: T1110 (Brute Force) e T1201 (Password Policy Discovery).

Estranhei o IP: a rede interna do cliente usa 192.168.x.x, então um 172.16.x.x pede investigação. Sobre o tempo de resposta, quanto mais o atacante fica conectado, mais ele faz. Em alguns casos a equipe prefere não avisar o atacante, mas isso normalmente não é decisão só do analista. No caso, o time de IR decidiu bloquear a conexão enquanto reunia dados.

A correlation search "Brute Force Access Behavior Detected - 1" usa o data model de autenticação. Logo, os logs têm a tag `authentication` e são compatíveis com o CIM. Ter relação com autenticação, só, não basta. Se não houver permissão para ver a correlation search, dá para perguntar ao time e usar o resto dos dados.

Alvos do IP de origem: DBell, PPark, JSam e Main-Inv, com volume muito alto de eventos em pouco tempo. O Main-Inv pode ser importante. (O material escreve o IP como 172.16.16.254 numa parte, provavelmente erro de digitação do .245.)

## Ride-along da Robin: Creation of Shadow Copy

Finding de urgência alta: criação de shadow copy no host `labrador.froth.ly`. Shadow copy é um snapshot de um volume, mesmo com dados em uso. Técnica MITRE: T1003 (credenciais do Peat Cerf usadas no servidor Active Directory).

Contexto pelo Asset Investigator (IP 192.168.70.150): labrador é um domain controller, ativo de alta prioridade, de propriedade do usuário pcerf. Tem outros findings recentes: muito tráfego web de saída, PowerShell remoto e autoruns de registro sendo adicionados.

Pelo Identity Investigator: Peat é usuário técnico, com privilégio elevado, e as credenciais dele aparecem em outros findings, incluindo excesso de logins falhos.

Processos do finding: `spoolsv.exe` e o filho `vssadmin.exe`. Para confirmar, Windows Event, EventCode 1 (criação de processo):

```spl
index=main sourcetype=xmlwineventlog EventCode=1 dest=labrador process_name=vssadmin.exe
| table _time user process process_exec parent_process
| sort _time +
```

O `vssadmin.exe` rodou duas vezes com menos de 3 minutos de diferença. Na primeira criou a shadow copy do C:, na segunda a apagou. Mesmo usuário (FROTHLY\pcerf) e mesmo processo pai, então os eventos estão ligados. Criar e apagar logo em seguida é suspeito.

Fechamento com a especialista em Splunk: a Grace confirmou que a TI não autorizou e que não foi o Peat. Portanto, verdadeiro positivo, e a investigação continua.

## Para revisar depois

- Resumo do que são `spoolsv.exe` e `vssadmin.exe` (a plataforma tem esse conteúdo expansível que não veio no texto)
- Revisar como funciona `coalesce`, `iplocation`, `geostats` e `stats values`
- Os quizzes precisam ser concluídos para liberar as lições (o quiz final 4.1 aparece como bloqueio)
- Passar tudo para o template de notas do curso antes da avaliação