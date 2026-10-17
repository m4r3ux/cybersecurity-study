# Logs e analise em ciberseguranca

Logs sao a base de qualquer operacao de Blue Team. Eles representam o rastro de tudo que acontece em sistemas, redes e aplicacoes. Sem logs, nao existe visibilidade — e sem visibilidade, nao existe deteccao nem investigacao.

Um evento e apenas uma acao isolada (ex: um login). O log e o registro persistente desse evento. Isso e o que permite reconstruir uma timeline depois. Em um incidente, muitas vezes o log e a unica evidencia disponivel.

Na pratica, diferentes fontes de log mostram partes diferentes do ataque. Logs de sistema mostram autenticacao e execucao de processos. Logs de rede mostram comunicacao entre maquinas. Logs web mostram tentativa de acesso e comportamento do usuario. Ferramentas de seguranca mostram deteccoes. Nuvem mostra uso de API e privilegios.

O trabalho real nao e olhar um log isolado, e sim correlacionar.

Exemplo mental:
Falhas repetidas de login (SSH ou Event ID 4625) indicam brute force.  
Se depois aparece login bem-sucedido (4624 ou "Accepted password"), houve comprometimento.  
Se logo depois surge execucao de processo suspeito (4688), ja temos pos-exploracao.

Outro exemplo:
Acesso a /admin no Apache + erro 401 + varios IPs → tentativa de descoberta  
Se combinado com scan detectado por IDS → fase de reconhecimento ativa

Isso mostra que logs so fazem sentido quando conectados.

Por isso eles sao usados para:
- deteccao (regras no SIEM)
- investigacao (timeline de ataque)
- threat hunting (buscar padroes suspeitos)
- compliance (provar atividades)

Mas existe um ponto critico: qualidade importa mais que quantidade.

Problemas comuns que quebram a analise:
- logs espalhados (sem centralizacao)
- horarios inconsistentes (sem NTP)
- formatos diferentes sem padrao
- muito ruido sem filtro
- pouca retencao (sem historico)

Sem resolver isso, o analista fica cego mesmo tendo dados.

---

# Formatos de logs e analise

Entender o formato do log e uma habilidade essencial. Antes de analisar o conteudo, o analista precisa entender como aquele dado esta estruturado.

Isso impacta diretamente:
- velocidade de investigacao
- capacidade de automacao
- qualidade da correlacao no SIEM

Logs em texto simples (comuns em Linux) sao faceis de ler, mas dificeis de escalar. Voce depende de grep, awk e regex. Funcionam bem para analise manual, mas mal para ambientes grandes.

Logs estruturados (JSON ou chave=valor) resolvem isso. Cada campo ja vem separado, entao voce consegue buscar diretamente por IP, usuario ou acao. Por isso sao o padrao em ferramentas modernas como SIEM, EDR e cloud.

CSV e intermediario. Bom para exportar e analisar rapido, mas limitado para correlacao complexa.

XML aparece em sistemas mais antigos ou corporativos. Ainda estruturado, mas menos usado hoje.

Logs binarios (como EVTX do Windows) nao podem ser lidos diretamente. Precisam de ferramentas (Event Viewer, PowerShell). Em compensacao, sao ricos em detalhes, especialmente para deteccao de comportamento (ex: criacao de processo, uso de privilegio).

Na pratica, cada formato muda a forma de trabalhar:

- Texto simples → leitura e filtro manual (ex: detectar brute force em auth.log)
- JSON → consultas diretas (ex: src_ip, alert.signature no SIEM)
- EVTX → analise profunda de comportamento no Windows (ex: cmd.exe, powershell)

Aqui entra um dos conceitos mais importantes: normalizacao.

Cada sistema gera logs com nomes diferentes para os mesmos dados. Um pode usar "src_ip", outro "source", outro "ip". Sem padronizar, nao da pra correlacionar.

Normalizacao resolve isso transformando tudo em um padrao unico, como:
- source_ip
- user_name
- event_time

Isso permite criar deteccoes que funcionam em varias fontes ao mesmo tempo.

Ferramentas como Logstash, Fluent Bit e syslog-ng fazem essa conversao e enriquecimento antes de enviar ao SIEM.

Pensamento de analista:
Nao importa o formato original — o importante e conseguir transformar em algo consultavel e correlacionavel.

Resumo mental final:

Logs sao a materia-prima da seguranca  
Formato define como voce analisa  
Correlacao define se voce detecta ou nao  

Sem estruturacao e normalizacao, log vira ruido  
Com estruturacao e correlacao, log vira inteligencia

---

# Timestamps e correlacao de eventos

Tempo e o eixo central da analise de logs. Tudo em Blue Team gira em torno de responder uma pergunta: "o que aconteceu primeiro, depois e por que". Sem timestamps confiaveis, essa linha do tempo quebra — e sem timeline, nao existe investigacao.

Mesmo que os logs estejam corretos individualmente, se o tempo entre sistemas estiver desalinhado, o analista pode interpretar tudo errado. O problema nao e falta de dado, e sim falta de sincronizacao.

Na pratica, diferentes sistemas registram tempo de formas diferentes:
- Linux pode usar hora local
- Firewall pode usar UTC
- Aplicacao pode usar outro fuso

Isso cria um erro comum: o mesmo evento parece acontecer em momentos diferentes.

Exemplo mental:
Um login acontece de verdade uma unica vez, mas aparece assim:
- Linux: 15:12 (UTC-3)
- Firewall: 18:12 (UTC)
- Aplicacao: 19:12 (UTC+1)

Sem ajuste, parecem tres eventos distintos. Com correlacao correta, e um unico evento atravessando camadas.

Esse tipo de erro causa:
- ordem de eventos invertida
- falha na correlacao
- investigacao incorreta
- falso positivo ou falso negativo

Por isso, antes de analisar ataque, o analista precisa confiar no tempo.


# Formatos de timestamp

Nem todo timestamp e igual. O formato muda dependendo do sistema, e o analista precisa reconhecer isso rapidamente.

Alguns aparecem legiveis (ex: "Jun 30 14:22:43"), outros ja padronizados (ISO 8601), outros em formato numerico (Unix epoch).

O mais importante nao e o formato em si, mas entender:
- qual fuso horario esta sendo usado
- qual a precisao (segundos, milissegundos)
- se e consistente entre sistemas

Logs modernos (especialmente JSON e cloud) usam UTC com alta precisao. Isso facilita muito a correlacao.


# NTP e sincronizacao

Para resolver o problema de tempo, existe o NTP (Network Time Protocol). Ele garante que todos os sistemas usem a mesma referencia de tempo.

Sem NTP:
cada maquina segue seu proprio relogio → caos na analise

Com NTP:
todos sincronizados (geralmente em UTC) → correlacao confiavel

Na pratica, isso e obrigatorio em ambientes reais. Um SOC sem sincronizacao de tempo simplesmente nao funciona corretamente.

O ponto chave:
nao adianta ter log bom se o tempo esta errado


# Normalizacao de tempo

Assim como os campos de log precisam ser padronizados, o tempo tambem precisa.

Durante a ingestao no SIEM, timestamps diferentes sao convertidos para um padrao unico (geralmente UTC). Isso permite:

- ordenar eventos corretamente
- criar alertas baseados em tempo
- montar timeline precisa

Ferramentas como Logstash fazem isso automaticamente ao transformar diferentes formatos em um campo padrao (ex: @timestamp).

Se isso falha:
- eventos podem aparecer fora de ordem
- alertas podem nao disparar
- investigacao fica inconsistente


# Pensamento de analista

Antes de confiar em qualquer analise, verifique o tempo.

Perguntas basicas:
- os sistemas estao sincronizados?
- estao usando UTC?
- os logs foram normalizados corretamente?

Se a resposta for nao, qualquer conclusao pode estar errada.


# Resumo mental

Tempo e mais importante que parece

Log sem tempo confiavel = historia errada  
Logs com tempos diferentes = correlacao quebrada  
Tempo padronizado (NTP + UTC) = investigacao confiavel  

Em Blue Team, entender tempo nao e detalhe tecnico  
e o que permite transformar log em evidencia

---

