Objetivos de aprendizagem:
- Explicar como os WAF são diferentes dos firewalls de borda tradicionais.
- Descrever a evolução dos WAFs e das aplicações WAF.

---

## O que é um WAF?
Um dispositivo ou software que monitora tráfego HTTP ou HTTPS e pode bloquear tráfego malicioso de e para uma aplicação web.

O WAF atua na camada de aplicação, enquanto o firewall atua na camada de rede e transporte.

Ele visa impedir ataques de falha de aplicações web.

O ancestral do WAF é o firewall de aplicação - baseado em rede mas tem como alvo algumas apps ou protocolos, como o FTP e RSH.

## Evolução WAF

### 2 geração
WAFs estudariam comportamento da aplicação para avaliar se as tentativas de acesso foram normais ou irregulares

Monitoramento de seção heurística, que permitiu ao firewall detectar variantes de assinaturas desconhecidas

Porém, foi impossível acompanhar o rápido crescimento de novas explorações.

### 3 geração
Aprendizado de máquina - defesa contra DDOS
Reputação de IP
Antivirius
Prevenção de perca de dados

Identifica um usuário, correlaciona suas ações com permissões e interrompa qualquer ação que ultrapasse o escopo de sua função.

Compartilhe informações e colabore com outros dispositivos de segurança na rede, como firewalls e sandboxes

Exponha e coloque em quarentena ataques de dia zero no ambiente sandbox e compartilhe as assinaturas com outros dispositivos na rede

Carregue novas descobertas em um centro de threat intelligence para que possam ser comunicadas a outras redes.