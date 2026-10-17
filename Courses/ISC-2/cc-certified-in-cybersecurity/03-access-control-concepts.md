## What is Security Control? (O que é um Controle de Segurança?)

Um **controle de segurança** é uma proteção ou contramedida criada para preservar a **Confidencialidade, Integridade e Disponibilidade (CIA Triad)** das informações.

O **controle de acesso** limita quais usuários (sujeitos) podem acessar determinados recursos (objetos), de acordo com regras definidas.

Exemplo:

- **Firewall:** controla o tráfego de rede, impedindo acessos não autorizados e evitando que informações sensíveis saiam do ambiente sem permissão.

### Ponto principal

Controles de segurança são mecanismos utilizados para proteger dados e sistemas, garantindo os princípios da **Confidencialidade, Integridade e Disponibilidade**.

---

## Controls Overview (Visão Geral dos Controles)

Os **controles de acesso** são fundamentais para a segurança da informação, pois definem quem pode acessar recursos da organização e o que pode fazer com eles.

O objetivo não é apenas bloquear acessos, mas também garantir que usuários autorizados tenham o nível correto de acesso.

### Subject (Sujeito)

É a entidade que solicita acesso a um recurso.

Pode ser:

- Usuário

- Processo

- Programa

- Dispositivo

- Cliente ou servidor


Características:

- É **ativo**, pois inicia a solicitação.

- Possui permissões (clearance) que determinam o que pode acessar.


### Object (Objeto)

É o recurso que está sendo acessado.

Pode ser:

- Arquivo

- Sistema

- Servidor

- Programa

- Dispositivo

- Serviço


Características:

- É **passivo**, respondendo apenas às solicitações recebidas.

- Não controla seu próprio acesso.


### Rule (Regra)

É a instrução que determina se o acesso será permitido ou negado.

Uma regra pode:

- Permitir acesso.

- Negar acesso.

- Definir o nível de acesso.

- Aplicar restrições baseadas em horário.

- Comparar atributos para decidir o acesso.


### Exemplo

Quando um usuário (**subject**) tenta abrir um arquivo (**object**), uma **rule** verifica suas permissões e decide se o acesso será permitido ou negado.

### Ponto principal

O controle de acesso é baseado em três elementos:

**Subject → Rule → Object**

Ou seja, um sujeito solicita acesso a um objeto, e uma regra determina o que ele pode ou não fazer.

---

## Defense in Depth (Defesa em Profundidade)

**Defense in Depth** é uma estratégia de segurança que utiliza múltiplas camadas de proteção para dificultar ataques e reduzir riscos.

Ela combina:

- **Pessoas**
- **Tecnologia**
- **Processos e operações**

A ideia é que, se uma camada falhar, outras continuem protegendo o ambiente.

### Exemplos

- **Autenticação Multifator (MFA):**
- Algo que você sabe (senha).
- Algo que você possui (código enviado ao celular).
- **Múltiplos firewalls:**
- Separação entre redes confiáveis e não confiáveis.
- Proteção adicional para sistemas com dados sensíveis.
- **Controles físicos, técnicos e administrativos:**
- Fechaduras e controle de acesso físico.
- Regras de acesso na rede.
- Políticas definindo quem pode acessar os recursos.

### Ponto principal

A defesa em profundidade utiliza várias camadas de segurança trabalhando juntas para proteger ativos da organização. Ela reduz as chances de sucesso de um ataque, mas não garante proteção absoluta.

---

## Examples of Least Privilege (Exemplos de Privilégio Mínimo)

O **Princípio do Privilégio Mínimo (Least Privilege)** determina que cada usuário deve possuir apenas os acessos necessários para realizar suas funções, nada além disso.

Exemplos:

- Funcionários do setor financeiro podem visualizar dados financeiros, mas apenas alguns podem alterá-los ou excluí-los.
- Profissionais da saúde podem acessar apenas as informações necessárias ao seu trabalho.
- Médicos podem visualizar somente os dados de seus próprios pacientes.
- Acessos podem ser temporários ou restritos a determinados horários.

Medidas complementares:

- Monitoramento e registro de acessos.
- Alertas automáticos para tentativas de acesso não autorizado.
- Uso de autenticação multifator (MFA), especialmente para contas com acesso a informações críticas.

### Ponto principal

O privilégio mínimo reduz o risco de acessos indevidos e ajuda a proteger a **Confidencialidade, Integridade e Disponibilidade (CIA)** das informações, concedendo apenas as permissões estritamente necessárias para cada usuário.

---

## Logical Access Controls (Controles de Acesso Lógico)

**Controles de acesso lógico** são mecanismos eletrônicos que restringem o acesso a sistemas, dados e recursos digitais.

Diferentemente dos controles físicos, que impedem o acesso a locais ou equipamentos, os controles lógicos controlam quem pode utilizar ou acessar os sistemas.

Exemplos:

- **Senhas**
- **Biometria** (impressão digital, reconhecimento facial etc.)
- **Leitores de crachá ou token** conectados a sistemas

### Ponto principal

Mesmo que uma pessoa tenha acesso físico a um dispositivo ou local, os controles de acesso lógico podem impedir que ela acesse sistemas ou informações sem a autorização adequada.

---

## Controls and Risks (Controles e Riscos)

Os controles são utilizados para reduzir riscos a um nível aceitável.

Existem diferentes tipos de controles que podem atuar juntos para mitigar um mesmo risco.

### Exemplos

- **Cinto de segurança:** controle físico.

- **Lei que obriga o uso do cinto:** controle administrativo.


Outro exemplo:

- **Risco:** uma estante alta tombar e ferir alguém.

- **Controle administrativo:** norma ou código de construção exigindo fixação da estante.

- **Controle físico:** suporte ou cinta prendendo a estante à parede.


### Ponto principal

Controles físicos e administrativos trabalham em conjunto para reduzir riscos e proteger pessoas, ativos e informações.


---

## Controls Assessments (Avaliação de Controles)

A redução de riscos depende da eficácia dos controles implementados. Os controles devem ser adequados ao cenário e proporcionais ao valor do que está sendo protegido.

Pontos principais:

- Nem sempre o controle mais avançado é o mais adequado.

- A escolha dos controles deve ser baseada em uma avaliação de riscos.

- O custo da proteção deve ser compatível com o valor do ativo protegido.

- Os controles precisam se adaptar às necessidades e mudanças do ambiente.


### Exemplo

Ao transformar uma área em local para armazenar informações confidenciais, uma avaliação pode determinar se é realmente necessário instalar leitores biométricos em todas as portas ou se controles mais simples, como fechaduras reforçadas, já são suficientes.

### Ponto principal

A avaliação de controles busca encontrar o equilíbrio entre **segurança, necessidade operacional e custo**, implementando proteções adequadas ao risco existente.

---

## Role-Based Access Control (RBAC) in the Workplace

O **Role-Based Access Control (RBAC)** concede permissões aos usuários com base em suas funções dentro da organização.

Exemplos:

- RH acessa arquivos de funcionários.

- Financeiro acessa dados bancários.

- Gestores acessam informações de suas equipes.

- Administradores podem ter acesso mais amplo.

- Novos funcionários recebem apenas as permissões necessárias para suas atividades.


Pontos importantes:

- As permissões devem seguir o princípio do **menor privilégio**.

- É necessário monitorar constantemente os acessos atribuídos aos cargos.

- Mudanças temporárias de permissões devem ser removidas quando não forem mais necessárias.


### Privilege Creep (Acúmulo de Privilégios)

Ocorre quando um usuário mantém permissões extras que não deveria mais possuir, geralmente após mudanças de função ou acessos temporários.

### Boa prática

Criar usuários a partir de **papéis (roles) padronizados**, em vez de copiar permissões de outros usuários. Isso reduz erros e garante que cada colaborador receba apenas os acessos adequados ao seu cargo.

---

## Privileged Access Management (PAM)

**Privileged Access Management (PAM)** é o gerenciamento de contas com privilégios elevados, garantindo que acessos administrativos sejam usados apenas quando realmente necessários.

Sem PAM, privilégios administrativos permanecem ativos o tempo todo, aumentando o risco de abuso ou comprometimento.

### Exemplo

Em uma empresa, administradores de TI receberam permissões de **Domain Admin** para facilitar o trabalho. Ao abrir um e-mail malicioso, um ransomware utilizou essas permissões para criptografar arquivos de servidores e estações de trabalho.

Com PAM:

- Privilégios elevados são ativados apenas quando necessários.

- O acesso administrativo é temporário (**Just-in-Time Access**).

- Atividades comuns, como e-mails e navegação, são realizadas com contas de privilégios reduzidos.


### Ponto principal

O PAM reduz o impacto de ataques e erros humanos ao limitar o uso contínuo de privilégios administrativos, seguindo o princípio do **menor privilégio**.

---

## Authorized Versus Unauthorized Personnel (Pessoal Autorizado vs. Não Autorizado)

Após a **autenticação** (confirmação da identidade), o sistema verifica se o usuário possui **autorização** para realizar a ação solicitada.

Em outras palavras:

- **Autenticação:** comprova quem é o usuário.
    
- **Autorização:** determina o que ele pode fazer.
    

Exemplos:

- Um crachá é validado para liberar o acesso a uma sala.
    
- Um sistema verifica se o usuário tem permissão para excluir um arquivo.
    

Se o usuário não possuir autorização, o acesso ou a ação será negada.

### Provisionamento de Usuários

O provisionamento define a criação, alteração e remoção de acessos dos usuários.

#### Novo funcionário

- Criação de conta e permissões de acordo com a função.
    
- Acessos aprovados pelo gestor e/ou políticas da empresa.
    

#### Mudança de cargo

- Concessão de novas permissões necessárias.
    
- Remoção dos acessos que não são mais necessários.
    

#### Desligamento

- Conta desativada após o término do vínculo.
    
- Remoção de grupos e permissões.
    
- Manutenção temporária da conta para auditorias, quando necessário.
    

### Boa prática

Não copiar permissões de usuários existentes para criar novas contas.

Em vez disso:

- Utilizar **roles padronizados**.
    
- Aplicar o princípio do **menor privilégio**.
    
- Evitar **Privilege Creep** (acúmulo indevido de permissões).
    

### Ponto principal

Um usuário deve primeiro ser **autenticado** e depois **autorizado**. O gerenciamento correto do ciclo de vida das contas (criação, alteração e remoção) é essencial para manter a segurança e evitar acessos indevidos.

---

## Privileged Accounts (Contas Privilegiadas)

**Contas privilegiadas** são contas que possuem permissões superiores às de usuários comuns, permitindo executar ações administrativas ou sensíveis.

Exemplos de usuários com contas privilegiadas:

- Administradores de sistemas.
    
- Equipes de suporte técnico (Help Desk).
    
- Analistas de segurança.
    
- Gestores ou responsáveis por projetos específicos.
    

### Riscos

Como possuem acesso elevado, o uso indevido ou comprometimento dessas contas pode causar grandes impactos à organização.

### Medidas de Segurança

- **Logs detalhados:** registrar todas as ações realizadas pelas contas privilegiadas.
    
- **Controles de acesso mais rigorosos:** uso obrigatório de MFA e autenticações adicionais.
    
- **Just-in-Time Access (JIT):** privilégios ativados apenas quando necessários.
    
- **Verificações de confiança:** avaliações mais rigorosas dos usuários que receberão esses acessos.
    
- **Auditorias frequentes:** monitoramento constante das atividades realizadas.
    

### Exemplo Prático

Um técnico de Help Desk pode precisar redefinir senhas de usuários. Em vez de receber privilégios completos de administrador de domínio, ele recebe apenas as permissões específicas para redefinir senhas e desbloquear contas.

Todas essas ações devem ser registradas e auditadas para garantir que foram realizadas de forma legítima.

### Ponto principal

Contas privilegiadas devem seguir o princípio do **menor privilégio**, recebendo apenas os acessos necessários para suas funções, com monitoramento, auditoria e controles mais rigorosos do que os aplicados a usuários comuns.

---

## Monitoring (Monitoramento)

O monitoramento é uma parte essencial da segurança física, ajudando a detectar, registrar e responder a atividades suspeitas.

### Câmeras

- Servem como vigilância e monitoramento.
    
- Podem desencorajar atividades criminosas.
    
- Quando gravam imagens, fornecem evidências para investigações.
    
- São úteis em locais de difícil acesso ou que exigem registro forense.
    

### Logs

- São registros de eventos ocorridos.
    
- Podem ser físicos (livros de registro) ou eletrônicos.
    
- Devem ser protegidos contra alterações e acessos não autorizados.
    
- Auxiliam em auditorias, conformidade e investigações forenses.
    
- Devem ser mantidos pelo período definido na política da organização.
    

**Anomalias em logs** são eventos incomuns que podem indicar problemas de segurança, como:

- Falhas em registros de data e hora.
    
- Bloqueios de contas.
    
- Tentativas de acesso ou escrita não autorizadas.
    

### Sistemas de Alarme

Alertam quando ocorre uma situação anormal.

Exemplos:

- Abertura não autorizada de portas ou janelas.
    
- Alarmes de incêndio acionados por fumaça ou calor.
    
- Botões de pânico para emergências.
    

### Guardas de Segurança

- Reforçam os controles físicos.
    
- Dificultam invasões, falsificação de identidade e tailgating (seguir uma pessoa autorizada para entrar sem autorização).
    
- Monitoram movimentação de pessoas e equipamentos.
    

### Sensores

Podem detectar tentativas de invasão usando:

- Infravermelho.
    
- Micro-ondas.
    
- Lasers.
    
- Sensores de vibração em cercas, portões e barreiras.
    

### Ponto principal

O monitoramento combina **câmeras, logs, alarmes, guardas e sensores** para detectar, registrar e responder a incidentes, fortalecendo a segurança física e fornecendo evidências para auditorias e investigações.