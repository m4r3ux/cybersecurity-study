Objetivos de aprendizagem

- Definir firewalls e sua evolução
- Descrever como funcionam os firewalls
- Explicar o status mais recente do firewall

---
## Evolução dos firewalls:
- Filtro de pacotes / firewall sem estado
- Firewall com estado
- Firewall de terceira geração
- Next-generation firewall (NGFW)

---

## Firewall de primeira geração
Analisa camadas de rede e transporte - analisando em cada um dos pacotes:
- Endereços de rede de origem e destino
- Protocolos
- Números de portas

Ele funciona com uma lista de regras, e na última regra, é definido se vai ser tomada uma decisão implícita ou explícita, deixando o pacote passar ou aplicando determinada ação.

Explícita: política aplicada se não for encontrada nenhuma outra correspondência na lista de políticas de firewall.

Implícita: criada para especificar se o tráfego é permitido ou negado.

Desvantagens:
- Precisa de configuração adicional para determinados casos
- Falha ao gerenciar protocolos
- Genérico

---

## Firewall de segunda geração
Observa as conexões de rede ao longo do tempo. Examina continuamente.

Se o pacote de retorno não condizer com o pacote de envio prévio, ele bloqueia.

Ele não bloqueia se for um protocolo autorizado, como o HTTP que funciona em:
- Conteúdo de texto estático
- Comércio eletrônico
- Host de arquivo
- Aplicações de web

Como todos usam o mesmo protocolo, ele não consegue bloquear ou não.

---

## Firewall de terceira geração
Esse já vem mais completo, com além dos anteriores fazendo análises mais aprofundadas e complexas, como:
- Filtro da camada de aplicação (FTP, HTTP, DNS) -> o tornando mais seguro.
- Proteções combinadas -> ele vem com Antivírus, Antispam, IPS (Intrusion Prevention System), VPN.

---

## O status mais recente do firewall
O Firewall deve verificar todos os dispositivos em uma rede e ao mesmo tempo garantir - segurança, confiabilidade, desempenho da rede.

O NGFW tem varios pontos de verificação de segurança.

Sendo eles:

Defesa de primeira linha - verifica pacotes de usa decisões baseadas em regras pra determinar se envia o tráfego ou descarta.

Defesa de segunda linha - ele desempenha a inspeção profunda de pacotes (DPI), que verifica códigos maliciosos e uso da rede.

Defesa de terceira linha - se algo suspeito for encontrado, ele envia o conteúdo malicioso para uma sandbox para análise posterior. 

Os NGFW, podem:
- Controlar aplicações por classificação ou usuários - protegendo-os na web contra ataques e ameaças
- Adota várioas abordagens de segmentação - segrega usuários e aplicações, diminuindo superfície de ataque.
- Passou de reativo para proativo - usa IA para aplicar políticas de segurança.

