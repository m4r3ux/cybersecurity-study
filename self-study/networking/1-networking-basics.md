## Como estudei

Dividi em três partes que se alimentam: entender (simulador e calculadora de máscara), fazer (lab por SSH na minha VPS) e provar (teste de saída). Os vídeos de apoio são do Professor Messer, playlist CompTIA Network+, só os tópicos de modelo OSI, TCP/IP, portas, DNS e DHCP.

## A viagem completa de um acesso

Quando digito um endereço e aperto Enter, acontecem quatro conversas em sequência: DNS para descobrir o IP, roteamento para decidir por onde sair, TCP para abrir o canal e HTTP dentro de TLS para pedir a página.

Na ordem exata:

1. O navegador olha o cache de DNS e o arquivo hosts.
2. Consulta DNS ao resolvedor, em UDP porta 53.
3. O resolvedor responde com o IP de destino.
4. A máscara decide: o destino está fora da minha rede, então o pacote vai para o gateway.
5. Handshake TCP na porta 443: SYN, SYN-ACK, ACK.
6. Negociação TLS: ClientHello (com o SNI) e ServerHello com o certificado.
7. Pedido HTTP (`GET /`) já dentro do TLS.
8. Resposta HTTP (`200 OK` com o HTML).
9. Encerramento com FIN e ACK.

Um detalhe que me ajudou a fixar: a porta de origem do navegador é aleatória (acima de 1024), e a de destino é a do serviço. É assim que o servidor sabe para onde devolver a resposta e que o PC consegue manter várias conversas ao mesmo tempo.

## Base: IP, máscara, portas, TCP e UDP

A máscara separa os bits que identificam a rede dos bits que identificam o host. Com `192.168.0.15/24`, a rede é `192.168.0.0`, o broadcast é `192.168.0.255` e sobram 254 hosts úteis. Antes de enviar qualquer pacote, o PC faz a mesma pergunta: o destino está na minha rede? Se sim, descobre o MAC por ARP e entrega direto. Se não, manda para o gateway.

Portas que preciso saber de cor:

|Porta|Protocolo|Serviço|
|---|---|---|
|22|TCP|SSH|
|25 / 587|TCP|SMTP (587 é envio autenticado)|
|53|UDP e TCP|DNS (UDP nas consultas comuns, TCP em respostas grandes e transferência de zona)|
|67 / 68|UDP|DHCP (servidor / cliente)|
|80|TCP|HTTP|
|443|TCP|HTTPS|
|3389|TCP|RDP|

TCP abre conexão antes, confirma entrega, reordena e retransmite, por isso é mais pesado. UDP só envia, sem garantia, por isso é leve e serve para DNS, DHCP, voz e vídeo.

OSI e TCP/IP no mínimo necessário: Aplicação (HTTP, DNS, TLS, SSH), Transporte (TCP, UDP, portas), Rede/Internet (IP, ping, traceroute) e Enlace/Acesso à rede (MAC, ARP, switch).

## Lab no servidor

Comandos que rodei e o que cada um me mostrou:

|Comando|O que aprendi|
|---|---|
|`nslookup`|Qual IP o domínio aponta e qual resolvedor respondeu. "Non-authoritative" quer dizer resposta de cache.|
|`dig`|O TTL mostra por quanto tempo a resposta fica em cache. `NOERROR` é sucesso, `NXDOMAIN` é domínio inexistente. `+trace` mostra raiz, TLD e autoritativo.|
|`ping`|Usa ICMP, que não tem porta. Sem resposta não quer dizer host fora do ar, muitos firewalls bloqueiam ICMP.|
|`traceroute`|Cada linha é um roteador. Asteriscos nem sempre são erro.|
|`sudo ss -tulnp`|Lista quem está escutando e qual processo. `0.0.0.0` está aberto para a rede, `127.0.0.1` só para a própria máquina.|
|`curl -v`|A viagem inteira em texto: DNS, TCP, TLS, pedido (linhas com `>`) e resposta (linhas com `<`).|

Experimentos que valem repetir: comparar `nslookup` com e sem `1.1.1.1`, rodar `dig` duas vezes seguidas para ver o cache, pingar `127.0.0.1`, o gateway e um site para comparar as latências, e testar `curl -v http://` para ver o 301 para HTTPS.

### Wireshark

Filtros usados, um por vez:

```
dns
tcp.flags.syn == 1
tcp.port == 443
tls.handshake
http
```

O desafio é comparar a captura com o simulador e achar os mesmos pacotes na mesma ordem. Em HTTPS o conteúdo aparece embaralhado no Follow TCP Stream, que é o TLS fazendo o trabalho dele.

## Onde isso aparece num SOC

Esta é a parte que mais me interessa, porque conecta a teoria com o trabalho de analista:

- Host interno falando direto com outro host interno, sem passar pelo gateway, pode ser movimento lateral.
- IP interno falando com IP externo estranho de madrugada é um dos primeiros alertas que se aprende a caçar.
- Scan de portas aparece como vários SYN sem o ACK final.
- DNS em UDP 53 com nomes enormes e estranhos é um jeito clássico de esconder dados saindo da rede (DNS tunneling).
- TTL muito baixo e domínio recém-criado são sinais clássicos de domínio malicioso.
- Porta aberta no `ss` que eu não reconheço é achado de segurança, porque o `ss` é o inventário da superfície de ataque.
- SSH na porta 22 é alvo número 1 de força bruta.

## Teste de saída

Desenhar no papel, sem consultar, o caminho de um acesso do DNS à resposta HTTP, com as portas. Checklist para me corrigir:

- [ ] DNS antes de tudo, marcado como UDP 53
- [ ] IP de destino vindo da resposta do DNS
- [ ] Decisão de sair pelo gateway (máscara)
- [ ] Os 3 passos do handshake na porta 443
- [ ] TLS separado de HTTP, dizendo o que cada um faz
- [ ] Porta de origem aleatória do cliente
- [ ] Resposta HTTP voltando e o fechamento

Meta da semana: explicar em voz alta, em um minuto, o caminho de um acesso. Se travar, volto à etapa correspondente do simulador.

## Próximos passos

- Refazer o desenho de memória amanhã e conferir com o checklist.
- Capturar um handshake real no Wireshark e anotar os pacotes.
- Repetir os comandos do lab no meu ambiente do Splunk e procurar esses mesmos eventos nos logs.