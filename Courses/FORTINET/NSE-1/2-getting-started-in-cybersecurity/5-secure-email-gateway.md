Objetivos de aprendizagem:
- Definir um secure email gateway
- Explicar os recursos de um secure email gateway
- Listar os mecanismos que ajudam a identificar remetentes legítimos e reduzir a falsificação
- Definir termos relacionados a um secure email gateway, como spam e phishing.
---

Spam: e-mails não solicitados e irrelevantes para muitos destinatários

Phishing: Ataque que usa e-mail e te um alvo amplo e discriminado de pessoas.

---

## O que é um SEG?
Ele é um firewall que funciona para e-mails, visando segurança e evitar organização cair em fraldes via e-mail.

Suas funções incluem:
- Filtro de conteúdo - filtra o que entra e o que sai em uma rede.
- Prevenção contra perca de dados (DLP - Data Loss Prevention)
- Filtros de spam - elimina conteúdo prejudicial, o promocional, etc - quantidade de spam.
- Autenticação e validação de identidade - evita falsificação e personificação de email
- Filtragem de malware - verifica anexos do e-mail em busca de arquivos maliciosos
- Criptografia - para não ser interceptado.

Tecnologias do filtro de spam:
- Filtros bayesianos
- Listas de negação
- Lista de permissões
- Algoritmos de aprendizado de máquina

## Estrutura de política do remetente
É usado um registro DNS chamado SPF (Estrutura de Política de Remetente) - onde a pessoa a qual você envia a mensagem, consulta seu registro e vê, por meio do SPF, se o seu servidor está autorizado a enviar e-mails em nome desse domínio - se não, o e-mail pode ser automaticamente caracterizado como suspeito.