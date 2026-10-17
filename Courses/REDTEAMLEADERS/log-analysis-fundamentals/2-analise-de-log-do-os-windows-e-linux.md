# Linux logs e analise de autenticacao

Em Linux, logs de autenticacao sao um dos pontos mais valiosos para o Blue Team. Eles mostram quem tentou entrar, quem conseguiu, de onde veio e o que fez depois. Na pratica, isso cobre desde o acesso inicial ate possivel escalada de privilegio.

O foco aqui nao e decorar arquivos, e sim entender onde esta o comportamento relevante.

Em sistemas Debian/Ubuntu, o centro disso e o auth.log. Em RHEL/CentOS, o equivalente e o secure. Esses arquivos concentram eventos de SSH e sudo — ou seja, entrada no sistema e uso de privilegios.

Existem outros logs importantes, mas muitos sao binarios (como wtmp, btmp, lastlog). Eles nao servem para leitura direta, mas para reconstruir historico de login com ferramentas como last e lastb.

Pensamento chave:
auth.log mostra tentativa  
wtmp mostra historico real  



# SSH como ponto de entrada

A maioria dos ataques em Linux comeca por SSH. Por isso, esses logs sao essenciais.

Um evento de falha mostra tentativa de acesso:
- usuario (ex: root)
- IP de origem
- porta
- protocolo

Um evento de sucesso mostra comprometimento potencial.

O ponto importante nao e um log isolado, mas a sequencia:

varias falhas → um sucesso  
isso e um dos sinais mais claros de brute force bem-sucedido

Outro sinal relevante e tentativa com usuarios invalidos. Isso indica reconhecimento (descobrir quais usuarios existem antes de atacar).


# Correlacao basica de ataque

Logs de autenticacao fazem sentido quando analisados em sequencia.

Exemplo tipico:
- varios "Failed password" do mesmo IP
- tentativa com varios usuarios
- depois "Accepted password"

Isso pode indicar:
- brute force
- credential stuffing
- uso de credencial vazada

Outro padrao:
- login vindo de IP interno inesperado
→ possivel movimento lateral

Ou ainda:
- login em horario incomum
→ possivel conta comprometida

Aqui entra o raciocinio de Blue Team:
nao olhar eventos, mas comportamento ao longo do tempo



# Sudo e escalacao de privilegio

Depois do acesso inicial, o proximo passo do atacante e ganhar privilegio.

O sudo mostra exatamente isso.

Um log de sudo revela:
- quem executou
- como (terminal)
- de onde (diretorio)
- qual comando virou root

Isso permite detectar coisas como:
- usuario comum abrindo shell como root
- execucao de comandos suspeitos (bash, wget, curl, nc)

Padrao perigoso:
usuario comum + sudo + comando de rede ou shell  
→ possivel pos-exploracao

Outro ponto:
picos de uso de sudo podem indicar atividade anormal



# SSH com chave e persistencia

Nem todo acesso usa senha. SSH com chave (publickey) e comum — e tambem pode ser usado para persistencia.

Se um atacante adiciona uma chave em authorized_keys, ele nao precisa mais de senha.

Entao, detectar:
- uso de chave desconhecida
- novos acessos com publickey

pode indicar backdoor silencioso


# Encerramento de sessao

Logs de disconnect ajudam a fechar a timeline.

Eles permitem:
- saber quanto tempo durou a sessao
- correlacionar inicio e fim
- identificar sessoes longas (possivel atividade manual)

Sessao muito longa ou estranha pode indicar uso ativo por atacante.



# Ferramentas e analise

No dia a dia, analise e feita com ferramentas simples:

grep filtra eventos  
awk extrai campos (ex: IP)  
sort/uniq contam frequencia  

Isso permite rapidamente responder perguntas como:
- qual IP mais tentou login?
- quais usuarios foram alvo?
- houve sucesso depois de falhas?

Logs binarios entram com:
- last (logins)
- lastb (falhas)

Isso complementa a visao do auth.log



# SIEM e deteccao

Quando enviado para SIEM, tudo isso vira campo estruturado:
- source.ip
- user.name
- event.outcome (success/failure)

Isso permite criar regras como:
falhas de SSH nos ultimos X minutos

Ou correlacoes mais avancadas:
falha repetida + sucesso → alerta critico

Sem parsing correto, isso nao funciona.



# Tecnicas de deteccao e hunting

Blue Team nao espera alerta — ele busca padrao.

Alguns exemplos:

Brute force:
muitas falhas em curto tempo do mesmo IP

Enumeracao:
tentativas com usuarios invalidos

Comprometimento:
falhas seguidas de sucesso

Movimento lateral:
logins internos fora do padrao

Escalada:
sudo com comandos suspeitos

Outra abordagem e olhar comportamento:
usuarios que nunca usam sudo comecando a usar
IPs novos acessando sistemas criticos

Isso e threat hunting baseado em log.



# Pensamento operacional

Logs de autenticacao contam uma historia:

entrada → validacao → acesso → privilegio → acao

O analista precisa conectar esses pontos.

Um log isolado raramente diz algo  
uma sequencia de logs mostra um ataque


# Resumo mental

SSH = porta de entrada  
Sudo = controle de privilegio  

Falha repetida = tentativa  
Sucesso apos falha = possivel comprometimento  
Usuario invalido = reconhecimento  
Sudo suspeito = pos-exploracao  

Logs de autenticacao nao mostram tudo  
mas mostram o inicio e a escalada do ataque

---

# Comparacao entre Linux e Windows logging

Linux e Windows registram os mesmos tipos de eventos — login, execucao, privilegio, persistencia — mas fazem isso de formas completamente diferentes. Essa diferenca muda diretamente como o analista pensa, investiga e detecta ataques.

No Linux, os logs sao simples, diretos e legiveis. No Windows, eles sao estruturados, ricos e orientados a eventos.

O impacto disso e claro:
no Linux, voce interpreta  
no Windows, voce consulta  



# Diferenca de abordagem

No Linux, voce trabalha com texto bruto. Cada linha precisa ser entendida, filtrada e correlacionada manualmente. Isso da flexibilidade, mas exige mais experiencia.

No Windows, os eventos ja vem organizados por ID. Um logon falho nao e apenas uma mensagem — ele ja e classificado como 4625. Isso permite deteccao mais direta.

Pensamento chave:
Linux mostra o que aconteceu  
Windows ja classifica o que aconteceu  



# Autenticacao como exemplo direto

Ambos registram autenticacao, mas de formas diferentes.

No Linux:
uma linha indica sucesso ou falha ("Accepted password", "Failed password")

No Windows:
cada acao tem um ID (4624 sucesso, 4625 falha)

Isso muda tudo na pratica.

No Linux, voce procura padroes no texto  
No Windows, voce filtra por evento

O comportamento detectado e o mesmo:
- varias falhas → tentativa
- sucesso depois → comprometimento

Mas no Windows isso vira regra facil  
No Linux isso vira analise manual  



# Execucao de processos

Aqui a diferenca fica ainda mais forte.

No Windows, a criacao de processo e registrada nativamente (4688). Se bem configurado, voce ve:
- qual processo rodou
- quem executou
- linha de comando

Com Sysmon, voce ganha ainda mais contexto.

No Linux, isso nao e padrao. Sem auditd, voce nao tem visibilidade completa da execucao.

Isso significa:
no Windows, execucao maliciosa deixa rastro claro  
no Linux, pode passar despercebida se nao houver configuracao  



# Escalada de privilegio

Nos dois sistemas, isso existe — mas aparece diferente.

No Linux:
sudo mostra diretamente quem virou root e qual comando executou

No Windows:
voce precisa correlacionar eventos
- 4672 (privilegios)
- 4688 (processo)

Ou seja:
Linux mostra a acao direta  
Windows exige correlacao de eventos  



# Persistencia e alteracoes

No Windows, persistencia costuma aparecer bem definida:
- criacao de servico (4697)
- alteracoes no sistema

No Linux, depende de onde o atacante mexe:
- cron
- systemd
- chaves SSH

E muitas vezes isso nao gera log claro sem auditoria ativa.

Insight importante:
Windows tende a expor persistencia  
Linux pode esconder se nao estiver bem monitorado  



# Visibilidade geral

Os dois sistemas nao sao completos por padrao.

Linux:
- forte em autenticacao
- fraco em processo e arquivo sem tuning

Windows:
- forte em autenticacao e processo
- ainda depende de ferramentas para profundidade (Sysmon)

Em ambos:
rede quase sempre depende de ferramentas externas

Conclusao:
visibilidade nao e padrao — e configurada  



# Investigacao na pratica

A diferenca aparece no dia a dia.

No Linux:
voce usa grep, awk, filtros manuais  
investigacao e mais direta, mas mais trabalhosa

No Windows:
voce usa Event Viewer, PowerShell ou SIEM  
investigacao e mais estruturada e escalavel

Isso cria dois estilos de analise:
Linux → exploratorio  
Windows → orientado a consulta  



# Mesmo ataque, rastros diferentes

Imagine um atacante:

Acesso inicial:
- Linux → SSH no auth.log
- Windows → 4624

Execucao:
- Linux → pode nao aparecer claramente
- Windows → 4688

Escalada:
- Linux → sudo
- Windows → 4672 + processo

Persistencia:
- Linux → cron/systemd (nem sempre logado)
- Windows → eventos claros

Resultado:
no Windows, a cadeia do ataque aparece mais completa  
no Linux, voce precisa montar essa cadeia manualmente  



# Pensamento de Blue Team

Nao existe melhor sistema — existe melhor visibilidade.

Linux exige:
- configuracao (auditd, syslog, etc)
- analise ativa

Windows exige:
- filtragem
- correlacao de eventos

Ambos exigem:
- centralizacao
- normalizacao
- SIEM



# Conclusao operacional

Logs sao diferentes, mas o objetivo e o mesmo:
reconstruir o ataque

Linux te da liberdade, mas cobra em visibilidade  
Windows te da estrutura, mas cobra em complexidade  

No fim:
nao importa o sistema  
importa sua capacidade de conectar eventos em uma linha do tempo coerente