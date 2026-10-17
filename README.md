```
.------------------------------------------------------------------.
|  o o o                       luciano@soc: ~/estudos              |
|------------------------------------------------------------------|
| $ tail -f /var/log/auth.log                                      |
| sshd[4121]: Failed password for root from 203.0.113.7            |
| sshd[4121]: Failed password for root from 203.0.113.7            |
| sshd[4121]: Failed password for admin from 203.0.113.7           |
| sshd[4125]: Accepted password for deploy from 203.0.113.7        |
|                                                                  |
| $ grep 203.0.113.7 auth.log | wc -l                              |
| 47                                                               |
| $ # falso positivo? nao. proximo passo: escalar_                 |
'------------------------------------------------------------------'
```

# blue team em construção

```
$ whoami
luciano

$ cat /etc/role
ex-SysAdmin, ex-front-end, futuro analista SOC N1

$ cat /etc/mission
ler o log, entender o que aconteceu, provar com evidência
```

## Origem

Comecei no front-end e fui para infraestrutura como SysAdmin: servidores Linux, e-mail, hospedagem, firewall. Foi ali, olhando log de acesso e resolvendo bloqueio, que a parte defensiva virou o que mais me interessa.

## Objetivo

Primeira vaga como Analista SOC N1. Receber o alerta, entender o que disparou, separar falso positivo de incidente real, documentar e escalar quando for o caso.

## O que tem aqui

```
$ ls
estudos/         anotações curtas, no meu jeito de explicar
deteccoes/       regras e consultas, cada uma com teste e falso positivo
investigacoes/   casos com linha do tempo, evidência e conclusão
labs/            o que montei e o que quebrou no caminho
```

Nada de print bonito sem raciocínio. Cada relatório aponta de onde veio cada afirmação.

## Regras do jogo

```
- dado real de terceiro nunca entra aqui
- ataque simulado roda só em VM isolada
- conclusão sem evidência não é conclusão
- se não consigo explicar em voz alta, ainda não aprendi
```

## Contato

Feedback técnico em relatório ou detecção é bem-vindo. Abra uma issue ou me chame no [LinkedIn](https://www.linkedin.com/in/SEU-PERFIL).