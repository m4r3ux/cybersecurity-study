Objetivos de aprendizagem:
- Maneiras de proteger um endpoint usando controles administrativos comuns
- Descrever métodos usados para proteger software e firmware de endpoint
- Explicar por que o gerenciamento de endpoints é fundamental para manter a segurança cibernética geral
- Descrever como proteger um endpoint sem acesso administrativo.


---

Categorias de hardening:
- Controles administrativos - senhas, restrições, principio do least preivlege
- Proteção de endpoint local - hardening de inicialização e so, gerenciamento de inicialização, segurança e criptografia de disco e prevenção contra perca de dados
- Manutenção de endpoint - patches, atualizações automáticas, verificações de  política e backups
- Monitoramento de endpoint - plataforma de proteção de endpoint (EPP - endpoint protection platform) - sistema de detecção de intrusão (IDS - intrusion detection system) - detecção e resposta de endpoint (edr - endpoint detection and response)

## Controles administrativos
Garantir que os dispositivos tenha senha segura (não deixar a padrão)

Princípio do privilégio mínimo (PoLP) - usuários devem ter apenas as permissões necessárias para desempenhar suas funções.

Restrições - bloquear acesso com senhas bem seguras ou autenticação de dois fatores, ou restrição de endereços IP.

Quanto mais camadas de segurança, melhor.

## Proteção de endpoint local

Sistema operacional e inicialização - proteger fisicamente os dispositivos para que os invasores não tenham acesso físico.

Bloquear BIOS (Basic Input/Output System) e outros sistemas de inicialização.

Firmware - executado a partir de um chip no endpoint, é responsável por relatar o hardware conectado ao dispositivo, auxilia no carregamento do sistema operacional.

Restringir o firmware para carregar somente softwares aprovados.

Se possível escolher, selecionar e usar um sistema operacional que seja fácil de gerenciar e seguro.

Ter lista fixa de sistemas operacionais confiáveis, garantindo que somente os softwares confiáveis e atualizados tenham acesso a rede.

Proteger e criptografar totalmente os endpoints é um aspecto fundamental da segurança cibernética - ainda mais em dispositivos delicados que tem informações confidenciais.

Podemos pra isso, usar a criptografia completa de disco (FDE - Full Disk ENcryption) - solução baseada em software onde o disco é criptografado

É protegido pelo TPM - Trusted Platform Module - então se for roubado, nenhum dado será roubado.

Outra solução é usar uma unidade com criptografia automática (SED - Self-encrypting Drive) - um disco rígido que gerencia automaticamente a criptografia e descriptografia do conteúdo.

Outra solução é usar um software de DLP - ele detecta se tem alguém tentando exfiltrar dados de um endpoint e envia-los pela rede. Bloquea e registra a transação - impede ou limita o uso de unidades flash, usb ou disco rígidos.

## Manutenção de endpoint 
Ter um modelo padronizado simplifica a manutenção de patches e atualizações. 

Manter os patches atualizados é fundamental - atualizar SOs, firmware e programas e aplicações é a maneira mais simples de reduzir o risco

Ter um sistema totalmente corrigido e atualizado por desacelerar e restringir o comprometimento dos sistemas.

Ter uma solução de backup abrangente para ambientes críticos é essencial (dispositivos, servidores, aplicações)

