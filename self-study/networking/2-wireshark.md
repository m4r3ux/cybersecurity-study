# Wireshark: ler tráfego e reconhecer padrões suspeitos

## Conceitos que explicam o que aparece no log

**Firewall stateless e stateful.** O stateless olha cada pacote isolado contra regras fixas. O stateful guarda uma tabela das conexões que nasceram de dentro e só deixa entrar o que pertence a elas.

Na prática: com uma regra stateless que libera entrada com porta de origem 80 ou 443, um pacote forjado com origem 80 passa. No stateful ele cai, porque não é resposta a nada que saiu da rede. Num log de firewall, um "allow" sem conexão prévia indica regra frouxa ou alguém testando o perímetro.

**NAT.** Troca o IP privado pelo IP público do roteador e guarda a tabela de tradução. Vários hosts internos aparecem para fora com o mesmo IP, só mudando a porta. O site, o proxy externo e o IDS fora do NAT enxergam apenas esse IP público. Para achar o host infectado, é preciso o log de tradução do firewall ou uma captura feita dentro da rede. Em investigação, o ponto onde o pcap foi capturado muda tudo.

**VPN, proxy, IDS e IPS.**

|Item|O que faz|O que muda no tráfego|
|---|---|---|
|VPN|Túnel criptografado entre dois pontos|Vejo o túnel, não o que passa dentro|
|Proxy|Acessa em nome do cliente|O destino vê o IP do proxy; o log do proxy tem usuário, URL e categoria|
|IDS|Detecta e alerta, só observa|Não bloqueia; falso positivo pode passar despercebido|
|IPS|Detecta e bloqueia, fica no caminho|Derruba pacote ou conexão, às vezes com RST; falso positivo pode tirar serviço do ar|

## Portas e a bandeira vermelha de cada uma

|Porta|Serviço|O que observar|
|---|---|---|
|22|SSH|Muitas tentativas seguidas indicam brute force|
|23|Telnet|Texto puro, inclusive a senha. Não deveria existir|
|25|SMTP|Estação enviando na 25 pode ser bot de spam|
|53|DNS|Consultas enormes ou repetidas para um domínio: tunneling|
|80|HTTP|GET e POST legíveis|
|110|POP3|Sem TLS expõe a senha do e-mail|
|143|IMAP|Mesmo risco do POP3 sem TLS|
|443|HTTPS|Conteúdo oculto, então olhar SNI, volume e regularidade|
|445|SMB|Movimento lateral e ransomware. Nunca exposta à internet|
|3389|RDP|Alvo de brute force e acesso exposto|

## Filtros de exibição

Os que usei no laboratório de captura simulada:

```
ip.addr == 10.0.0.16
dns
http.request
tcp.flags.syn == 1 && tcp.flags.ack == 0
ip.src == 203.0.113.50 && tcp.flags.syn == 1 && tcp.flags.ack == 0
ip.src == 10.0.0.15 && ip.dst == 198.51.100.77 && tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.flags.reset == 1
```

O filtro do SYN sem ACK é o mais útil do conjunto, porque isola o primeiro passo de cada handshake. É por ele que a varredura de portas aparece. A coluna Δt (tempo desde o pacote anterior exibido) revela padrões regulares, e Statistics, Conversations mostra quem fala com quem e quantos bytes trocam.

## Padrões suspeitos

|Padrão|Como aparece|Onde olhar|
|---|---|---|
|Varredura de portas|Um IP manda SYN para muitas portas do mesmo alvo em milissegundos. Porta fechada responde RST, aberta responde SYN-ACK e o scanner derruba com RST|SYN sem ACK e Conversations|
|Beaconing|Mesmo host contata o mesmo destino em intervalos quase fixos, com pacotes pequenos|Filtrar o par e olhar o Δt|
|DNS suspeito|Domínios aleatórios, TLDs baratos, muitas consultas para um domínio, nomes longos|`dns`|
|Credenciais em texto puro|FTP, Telnet, POP3 ou HTTP sem TLS mostrando usuário e senha|Follow TCP Stream|
|Brute force|Dezenas de conexões curtas do mesmo IP para 22 ou 3389|`tcp.dstport == 22` e depois Conversations|
|Exfiltração|Volume de saída muito maior que o de entrada para destino externo incomum, fora do horário|Conversations ordenado por bytes|

Nenhum desses sinais prova nada sozinho. Backup também é regular e monitor de uptime também faz beaconing. O trabalho do analista é combinar sinais, como domínio novo mais regularidade mais processo estranho no endpoint, e relatar com evidência.

## O que a captura simulada mostrou

O scanner é o `203.0.113.50` e o alvo é o `10.0.0.20`. Ele manda SYN de uma mesma porta de origem (44444) para uma sequência de portas, com intervalo de cerca de 10 ms entre uma e outra. Quase todas respondem RST, ACK, ou seja, estão fechadas.

As exceções são as portas 22 e 80. Nelas aparece `44444 > 22 [RST]` e `44444 > 80 [RST]` vindo do scanner, que é o comportamento de quem recebeu SYN-ACK e derrubou a conexão logo depois. Isso indica porta aberta.

Outro host, o `10.0.0.15`, fala com `198.51.100.77` em poucos pacotes pequenos, que é o candidato a beaconing a confirmar pelo Δt.

## Laboratório com pcaps reais

Método que montei, na ordem:

1. Statistics, Conversations (IPv4): o host interno com mais contatos externos é suspeito.
2. Filtro `dns`: anotar domínios estranhos e o IP para o qual resolveram.
3. Filtro `http.request`: observar GET, POST, URIs com executáveis e User-Agent esquisito.
4. Em HTTPS, filtro `tls.handshake.type == 1` e leitura do SNI no ClientHello.
5. Hostname e usuário do infectado em tráfego DHCP, NBNS ou Kerberos.

Fontes: dois exercícios simples do Malware-Traffic-Analysis.net e a página Sample Captures da wiki oficial do Wireshark (uma captura de DNS, uma de HTTP e uma de FTP). Os pcaps só abrem em VM ou máquina isolada, e nada do zip é executado.

Pontos para observar em cada captura da wiki:

- DNS: o par pergunta e resposta, o tipo de registro (A, AAAA, MX), o TTL e uma consulta com NXDOMAIN.
- HTTP: Follow TCP Stream, método, URI, User-Agent e código de status, tudo em texto puro.
- FTP: filtro `ftp` e os comandos USER e PASS, que mostram usuário e senha legíveis. O mesmo problema existe em 23, 110 e 143.

## Teste de saída

Responder sem olhar o gabarito, usando filtros e Conversations:

- [ ] IP que faz a varredura
- [ ] IP alvo da varredura
- [ ] Portas abertas encontradas
- [ ] Host que faz beaconing
- [ ] Destino do beaconing
- [ ] Intervalo aproximado, em segundos
- [ ] Domínio ligado ao beaconing

Depois, escrever o relato do incidente com evidência para cada item.

## O que levo daqui

Ler um pcap é uma sequência de perguntas curtas: quem fala com quem, quanto trafega, com que regularidade e em qual porta. Os filtros respondem cada uma, e o Δt e o Conversations fecham o quadro.

A parte que mais pesa para SOC é o relato. Um sinal isolado não fecha um caso, então o que importa é reunir indícios que se confirmam e anotar de onde veio cada um.