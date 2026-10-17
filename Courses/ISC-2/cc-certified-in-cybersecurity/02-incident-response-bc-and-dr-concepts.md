## Incident Terminology (Terminologia de Incidentes)

Profissionais de segurança não apenas previnem ataques, mas também atuam na resposta a incidentes quando algo dá errado.

Principais termos:

- **Event (Evento):** qualquer ocorrência observável em um sistema ou rede.
- **Incident (Incidente):** evento que compromete ou ameaça a confidencialidade, integridade ou disponibilidade de informações.
- **Threat (Ameaça):** qualquer circunstância com potencial de causar danos a sistemas, dados ou operações.
- **Vulnerability (Vulnerabilidade):** falha ou fraqueza que pode ser explorada por uma ameaça.
- **Exploit:** técnica ou ataque utilizado para explorar uma vulnerabilidade.
- **Intrusion (Intrusão):** tentativa ou obtenção de acesso não autorizado a um sistema.
- **Breach (Violação/Vazamento):** acesso, divulgação ou aquisição não autorizada de informações, especialmente dados pessoais.
- **Zero-Day:** vulnerabilidade desconhecida ou sem correção disponível, que pode ser explorada antes de ser detectada ou corrigida.

### Relação entre os termos

Uma **ameaça** pode explorar uma **vulnerabilidade** usando um **exploit**, causando uma **intrusão**, que pode resultar em um **incidente** ou **breach**. Tudo geralmente começa com um **evento** observado na rede ou sistema.

---

## The Goal of Incident Response (Objetivo da Resposta a Incidentes)

Toda organização deve estar preparada para incidentes, pois eventos que afetam operações e objetivos são inevitáveis.

Um **evento** é qualquer ocorrência observável. Quando esse evento pode prejudicar a missão ou as operações da empresa, ele se torna um **incidente**.

O objetivo da **resposta a incidentes** é reduzir o impacto do incidente e restaurar as operações normais o mais rápido possível. Para isso, a organização deve possuir um **plano de resposta a incidentes**.

A resposta a incidentes faz parte do **Business Continuity Management (BCM)**, que busca garantir a continuidade das operações durante crises.

Pontos principais:

- O principal objetivo é estar preparado para incidentes.
- A preparação exige políticas e planos de resposta definidos.
- Algumas organizações chamam esse processo de gerenciamento de crises.
- A prioridade máxima em qualquer incidente é proteger a vida, a saúde e a segurança das pessoas.

---

## Components of a Business Continuity Plan (Componentes de um Plano de Continuidade de Negócios)

O **Business Continuity Plan (BCP)** é um plano criado para restaurar as operações da organização após desastres ou interrupções significativas.

O plano deve envolver diferentes áreas da empresa para garantir que todos os processos, sistemas e operações sejam considerados.

Principais componentes:

- Lista da equipe responsável pelo BCP, com contatos e substitutos.
- Definição de responsabilidades e autoridades da gestão.
- Contatos de fornecedores, clientes e parceiros críticos.
- Critérios para ativar o plano.
- Procedimentos imediatos de resposta e checklists.
- Procedimentos de segurança e emergência.
- Métodos de notificação e comunicação com funcionários.
- Estrutura para alertar as equipes quando o plano for acionado.

O objetivo do BCP é garantir que a organização consiga manter ou restaurar rapidamente suas operações após um incidente grave ou desastre.

---

## Business Continuity in Action (Continuidade de Negócios na Prática)

Um exemplo de continuidade de negócios ocorre quando uma empresa sofre um incidente grave, como um incêndio que destrói o departamento de faturamento.

Como a empresa já havia realizado uma **Business Impact Analysis (BIA)**, ela sabia a importância do setor e possuía um plano de contingência. Um local alternativo de trabalho já estava disponível e, durante a transição, o atendimento relacionado a faturamento foi assumido pela equipe de atendimento ao cliente.

Graças ao planejamento prévio, à análise de impacto e à existência de recursos alternativos, a empresa conseguiu continuar operando sem interrupções significativas nos serviços prestados aos clientes.

**Lição principal:** um BCP eficaz permite que a organização mantenha suas operações mesmo após incidentes graves, minimizando impactos ao negócio.

---

## The Importance of Business Continuity (A Importância da Continuidade de Negócios)

O objetivo da continuidade de negócios é manter as operações da organização funcionando durante e após uma interrupção significativa.

A comunicação é um dos elementos mais importantes do plano, incluindo múltiplas formas de contato, números de emergência e alternativas caso os sistemas normais falhem.

Pontos principais:

- Acionar rapidamente as pessoas responsáveis pelo plano.
- Envolver a gestão para definir prioridades e tomar decisões.
- Manter contatos críticos de fornecedores, parceiros e serviços de emergência.
- Utilizar planos de comunicação alternativos em caso de falha dos sistemas convencionais.
- Ter uma árvore de contatos (phone tree) para garantir que sempre haja alguém disponível para ser acionado.
- Seguir procedimentos e checklists previamente definidos para evitar erros durante a crise.

**Lição principal:** a eficácia de um plano de continuidade depende de preparação, comunicação clara e procedimentos bem documentados para manter o negócio operando durante incidentes.

---

## Components of the Incident Response Plan (Componentes do Plano de Resposta a Incidentes)

O Plano de Resposta a Incidentes define como a organização se prepara, responde e se recupera de incidentes de segurança.

Principais componentes:

- Criar uma política aprovada pela gestão.
- Identificar sistemas, dados críticos e pontos únicos de falha.
- Treinar funcionários para resposta a incidentes.
- Formar uma equipe de resposta a incidentes.
- Definir papéis e responsabilidades.
- Planejar a comunicação entre as partes envolvidas.
- Prever métodos alternativos de comunicação.
- Coletar e preservar evidências.
- Identificar o atacante e os vetores de ataque.
- Conter e isolar o incidente.
- Monitorar possíveis novas tentativas de ataque.
- Analisar o incidente com base em evidências e inteligência de ameaças.
- Priorizar as ações de resposta.
- Padronizar a documentação dos incidentes.
- Registrar lições aprendidas.

### Fases da Resposta a Incidentes

1. **Preparação**
2. **Detecção e Análise**
3. **Contenção, Erradicação e Recuperação**
4. **Atividades Pós-Incidente (Lições Aprendidas)**

**Lição principal:** um plano de resposta a incidentes bem definido permite identificar, conter, investigar e recuperar-se de incidentes de forma rápida e organizada.

---

## Components of a Disaster Recovery Plan (Componentes de um Plano de Recuperação de Desastres)

Um **Disaster Recovery Plan (DRP)** contém documentos específicos para diferentes públicos dentro da organização, facilitando a recuperação dos sistemas após um desastre.

Principais componentes:

- **Resumo executivo:** visão geral do plano para a gestão.
- **Guias técnicos:** instruções detalhadas para as equipes de TI restaurarem sistemas e infraestrutura.
- **Cópias completas do plano:** destinadas aos membros da equipe de recuperação de desastres.
- **Checklists:** auxiliam as equipes a seguirem procedimentos durante situações de crise.
- **Documentos para gestores e relações públicas:** orientam a comunicação do incidente de forma clara e consistente.

**Lição principal:** o DRP deve fornecer informações adequadas para cada grupo envolvido, permitindo uma recuperação organizada, rápida e eficiente dos sistemas e serviços.

---

## Disaster Recovery in the Real World (Recuperação de Desastres no Mundo Real)

Para que um plano de recuperação de desastres seja eficaz, é necessário identificar sistemas críticos e manter backups testados regularmente.

Pontos principais:

- Incidentes podem ser descobertos dias ou meses após ocorrerem.
- Não basta fazer backup apenas dos servidores; é preciso considerar bancos de dados e dependências entre sistemas.
- Em ambientes corporativos, os dados geralmente são compartilhados entre vários sistemas interligados.
- É fundamental entender o fluxo de dados e como um sistema depende de outro.
- O mapeamento dessas dependências ajuda a garantir uma recuperação correta e completa após um desastre.

**Lição principal:** um bom plano de recuperação de desastres deve proteger não apenas os dados, mas também as integrações e dependências entre sistemas para garantir a restauração completa das operações.