Objetivos de aprendizagem:
- Explicar como funciona o controle de acesso à rede (Network Access Control - NAC) para proteger as redes
- Descrever a evolução do NAC, incluindo a introdução de dispositivos (Bring your own device - byod) e IoT
- Identificar recursos NAC adicionais
- Listar os benefícios que o NAC oferece para uma organização.

---

## O que é
O NAC avalia e classifica a conformidade de segurança por 
- Usuario
- Dispositivo
- Localização
- Sistema operacional

Alguns tem arquitetura centralizada - melhora o controle de dispositivos em redes grandes e com vários locais.

Surgiu como um método de autenticação e autorização para dispositivos que acessavam a rede e seguiam os padrões IEEE 802.1X

Ele tem um autenticador, que pode ser um:
- Switch de rede
- Acesso sem fio

E pode ser autenticado com 
- Nome de usuário e senha
- Certificado digital

Os dispositivos que tentam se conectar à rede são de propriedade pessoa, então o departamento de MIS (Sistema de informação gerencial) não controla o que é executado nesses dispositivos

Os dispositivos IoT transmitem dados de um lugar para outro pela Internet, expandindo a superfície de ataque.

---

## Recursos atuais do NAC
Quando implantado, ele cria perfis para todos os dispositivos conectados na rede, assim se um dispositivo não condizer com um perfil ou não existir, não tem acesso liberado.

Uma câmera IP, não deve ter acesso ao servidor financeiro, por exemplo - mas sim ter acesso ao servidor de gravação de vídeo (passando por firewall e outros) - assim, segmentando a rede usando VLAN

Dessa maneira, se ocorresse um ataque direcionado ao servidor NVR (gravação de vídeo), os atacantes não conseguiriam se movimentar lateralmente com facilidade pela rede ser isolada das demais.

As permissões por perfil (de contratados, parceiros ou convidados) são configuradas de acordo com necessidade, então um convidado que está na recepção não precisa ter acesso à rede organizacional, mas um técnico de TI e rede sim.

O NAC permite que uma empresa gerencie e autentique usuários e dispositivos temporários.

Ele pode ser integrado a uma estrutura de segurança, como o SOC - gerando um alerta em caso de ameaça ou incidente.

---

# Benefícios
- Segurança melhorada - melhora a segurança e autitentica usuários
- Economia de custo - oferece economia de custos pois são necessários menos recursos de TI
- Automação - evita trabalho manual
- Experiências de TI aperfeiçoadas - experiência de usuáiro sem atrito.
- Facilidade de controle - inventário 24/7 de todos os endpoints autorizados - por motivos de segurança e de logística.