## **Objetivos do aprendizado  

Depois de concluir este curso, você será capaz de: 

- Comparar os cinco principais grupos de atores de ameaças com base em seus perfis e características
- Descrever tipos comuns de ataques cibernéticos
- Explicar a estrutura geral de um ataque cibernético típico descrito na estrutura Cyber Kill Chain
- Identificar táticas e técnicas de invasores cibernéticos com a matriz MITRE ATT&CK
- Explicar os principais elementos do ecossistema do crime cibernético
- Descrever técnicas comuns de engenharia social
- Descrever inteligência de código aberto (OSINT) e fontes padrão que os invasores cibernéticos utilizam
- Explicar o propósito e as informações apresentadas por técnicas típicas de varredura técnica
- Realizar reconhecimento de rede com ferramentas de varredura de rede
- Liste os detalhes e as lições aprendidas com vários ataques cibernéticos de alto perfil

---
## Modulo 1 - Grupos de atores de ameaca

## Conceitos

- Atacante: pessoa que realiza um ataque sem autorizacao
- Agente de ameaca: entidade que representa uma possivel ameaca (pode ainda nao ter atacado)

Diferenca:
- Atacante = ja executa ataque
- Agente de ameaca = potencial de ataque

## Tipos de agentes de ameaca

### Script kiddie
- Baixa habilidade tecnica
- Usa ferramentas prontas
- Motivacao: curiosidade ou diversao

### Hacktivista
- Motivacao politica ou ideologica
- Realiza ataques por causa de causas

### Gangue criminosa
- Grupo organizado
- Motivacao: dinheiro
- Atividades: fraude, ransomware, roubo de dados

### Hacker de estado-nacao
- Ligado a governos
- Alto nivel tecnico e recursos
- Objetivo: espionagem ou sabotagem

### Agente interno malicioso
- Pessoa de dentro da organizacao
- Ja possui acesso ao sistema
- Pode agir por vinganca ou ganho financeiro

## Objetivo

- Entender os tipos de agentes de ameaca
- Comparar motivacoes, recursos e comportamentos

## Resumo

- Agente de ameaca = risco potencial
- Existem 5 principais tipos
- Diferenciam-se por motivacao, habilidade e recursos

---

## Modulo 2 - Tipos de ataques ciberneticos

## Conceito

- Ataques ciberneticos sao metodos usados para explorar sistemas
- Podem focar em:
  - Vulnerabilidades tecnicas (sistemas, redes)
  - Vulnerabilidades humanas (engenharia social)

## Tipos de ataques

### DoS (Denial of Service)
- Ataque de negacao de servico
- Objetivo: derrubar um sistema ou servico
- Fazendo sobrecarga de requisicoes

### DDoS (Distributed Denial of Service)
- Versao distribuida do DoS
- Usa varios dispositivos (botnet)
- Mais dificil de bloquear

### Phishing
- Enganar o usuario para roubar informacoes
- Ex: emails falsos
- Objetivo: senhas, dados bancarios

### Spear phishing
- Phishing direcionado
- Alvo especifico (pessoa ou empresa)
- Mais sofisticado e convincente

### Malware
- Software malicioso
- Tipos:
  - Virus
  - Trojan
  - Ransomware
- Objetivo: danificar, roubar ou controlar sistemas

### MitM (Man-in-the-Middle)
- Intercepta comunicacao entre duas partes
- Pode roubar ou alterar dados

### Ataques DNS
- Exploram o sistema de nomes de dominio
- Podem redirecionar usuarios para sites falsos

## Objetivos do modulo

- Entender os principais tipos de ataques
- Reconhecer como a IA pode aumentar ataques
- Analisar ameacas em tempo real

## Resumo

- Ataques podem explorar tecnologia ou pessoas
- Existem varios tipos comuns
- Diferem por tecnica e objetivo
---

## Modulo 3 - Estrutura de um ataque cibernetico

## Conceito

- Ataques ciberneticos evoluem com o tempo
- Dependem de vulnerabilidades (ex: software desatualizado)
- Mesmo com mudancas, existe uma estrutura comum de ataque

## Estruturas utilizadas

### Cyber Kill Chain
- Modelo que descreve as etapas de um ataque
- Ajuda a entender como o ataque acontece do inicio ao fim
- Usado para detectar e interromper ataques

### MITRE ATT&CK
- Matriz que organiza tecnicas e taticas de invasores
- Baseada em ataques reais
- Ajuda a identificar comportamento de atacantes

## Objetivos do modulo

- Entender a estrutura de um ataque cibernetico
- Aplicar o modelo Cyber Kill Chain em cenarios reais
- Identificar tecnicas usando MITRE ATT&CK

## Resumo

- Ataques seguem uma estrutura, mesmo evoluindo
- Cyber Kill Chain mostra as etapas do ataque
- MITRE ATT&CK mostra como os ataques sao feitos

---

## Cyber Kill Chain

## Conceito

- Modelo criado pela Lockheed Martin
- Descreve as etapas de um ataque cibernetico
- Ataque e visto como um processo (nao evento unico)
- Etapas acontecem em ordem e dependem umas das outras

## Etapas

### 1. Reconhecimento
- Coleta de informacoes sobre o alvo
- Ex: pesquisa, escaneamento, engenharia social

### 2. Armamento
- Criacao ou obtencao de malware
- Baseado na vulnerabilidade encontrada

### 3. Entrega
- Envio do malware para o alvo
- Ex: email, download, USB infectado

### 4. Exploracao
- Execucao do malware
- Explora uma vulnerabilidade do sistema

### 5. Instalacao
- Malware cria persistencia
- Ex: backdoor, novas contas, acesso remoto

### 6. Comando e Controle (C2)
- Invasor se comunica com o sistema infectado
- Envia comandos e recebe dados

### 7. Acoes sobre objetivos
- Execucao do objetivo final
- Ex: roubo, alteracao ou destruicao de dados

## Resumo

- Ataques seguem 7 etapas sequenciais
- Cada etapa depende da anterior
- Objetivo: infiltrar, manter acesso e agir no sistema
---

## Matriz MITRE ATT&CK

## Conceito

- Criada pela organizacao MITRE
- Baseada em dados reais de ataques
- Reune taticas, tecnicas e procedimentos (TTP)
- Uso: entender e analisar ataques ciberneticos
- Disponivel gratuitamente

## Estrutura

- Colunas = taticas (objetivos do invasor)
- Linhas/itens = tecnicas (formas de atingir o objetivo)

## Como funciona

- O invasor tem um objetivo (tatica)
- Escolhe uma tecnica para atingir esse objetivo
- Se falhar, pode tentar outra tecnica

## Exemplo

- Objetivo: obter acesso ao sistema (tatica)
- Tecnica: forca bruta
  - Testa varias combinacoes de usuario e senha
- Se nao funcionar, tenta outra tecnica

## Resumo

- ATT&CK organiza como ataques sao feitos
- Taticas = objetivo
- Tecnicas = metodo
- Ajuda a identificar comportamento de invasores
---
## Módulo 4 - Ecossistema de crime cibernético  

**Objetivos do aprendizado  

Após concluir este módulo, você será capaz de:

- Descrever o ecossistema subterrâneo do crime cibernético
- Descrever os três métodos que os criminosos cibernéticos utilizam para ganhar dinheiro
- Explicar a relação entre criptomoeda e crime cibernético
- Resumir as cinco etapas gerais na economia do crime cibernético
---

## Módulo 5 - Engenharia social  

Quando você pensa em ameaças de segurança cibernética, provavelmente pensa em ameaças digitais, como malware, que exploram vulnerabilidades na tecnologia. Mas os invasores também podem explorar vulnerabilidades entre as **pessoas** para conseguirem o que desejam.

Neste módulo, você aprenderá sobre engenharia social e as técnicas que os invasores utilizam para atacar pessoas, não a tecnologia.

Objetivos do aprendizado  

Após concluir este módulo, você será capaz de:

- Definir engenharia social
- Explicar os motivos pelos quais a engenharia social funciona
- Listar os principais aspectos de um bom ataque de engenharia social
- Listar maneiras de se defender contra a engenharia social
- Identificar sinais comuns de um e-mail de phishing

---

## Conceito  
  
- Engenharia social é a arte de obrigar alguém a fazer o que você deseja  
- Está relacionada a áreas como:  
- Psicologia  
- Biologia  
- Matemática  
  
- Na cibersegurança:  
- Uso do engano para manipular pessoas  
- Objetivo: obter informações confidenciais ou pessoais  
- Induz a vítima a cometer erros de segurança  
  
## Como funciona  
  
- Baseado em interações sociais  
- O atacante leva a vítima a abrir mão de algo privado  
- Pode ocorrer através de:  
- Contato pessoal  
- Telefone  
- Internet (sites, e-mails, redes sociais)  
  
## Impactos  
  
- Acesso a sistemas confidenciais  
- Roubo de ativos  
- Possibilidade de ataques mais complexos  
  
- Técnica considerada:  
- Muito poderosa  
- Comprovada por diversos estudos de caso  
  
## Exemplo  
  
- Fraudes que fazem pessoas perderem economias  
- Uso de truques de confiança  
  
- Em ambientes físicos:  
- Entrada não autorizada em áreas seguras  
- Ex: seguir alguém autorizado para entrar no local  
  
## Objetivos do modulo  
  
- Entender o conceito de engenharia social  
- Compreender como ataques exploram pessoas  
- Identificar formas de execução  
- Reconhecer os riscos envolvidos  
  
## Resumo  
  
- Engenharia social usa manipulação e engano  
- Explora o comportamento humano  
- Pode ocorrer em vários meios  
- Permite acesso, roubo e ataques mais avançados

---

## Principais aspectos de um ataque de engenharia social  

- Um ataque de engenharia social normalmente tem esses elementos  

### 1. É bastante conhecido  

- O invasor tenta se passar por membro de uma empresa  
- Usa:
  - Papel timbrado  
  - Jargão  
  - Formato da empresa  

- Objetivo: construir credibilidade  

- Nem todos os métodos são igualmente eficazes  
- Os ciberatacantes pesquisam para determinar o melhor método para a vítima  

### 2. É entregue com confiança  

- Engenheiros sociais são preparados e confiantes  
- Conseguem tranquilizar as vítimas  

- Fatores importantes:
  - Saber quando lançar o ataque  
  - Desenvolver relacionamento com a vítima  

- O ataque pode acontecer em várias trocas:
  - Aumentando credibilidade  
  - Reduzindo inibições  

- Apressar o ataque pode:
  - Fazer o atacante parecer desesperado  
  - Revelar o golpe  

### 3. É plausível e realista  

- O ataque parece real  
- A vítima muitas vezes não percebe que foi enganada  
---

## Como você pode se defender da engenharia social?  

- Todos devem estar cientes e se proteger contra ataques de engenharia social  

## Regra principal  

- Se algo parecer bom demais para ser verdade, provavelmente é  

- Fique atento a:
  - Ganhos financeiros inesperados  
  - Solicitações de emprego inesperadas  
  - Prêmios de concursos que você não participou  

## Comportamento seguro  

- Não confiar facilmente  
- Questionar solicitações incomuns  
- Pedir mais detalhes quando algo parecer estranho  
- Verificar antes de agir  

- Exemplos:
  - Colega desconhecido fazendo pedido estranho  
  - Pessoa em área restrita  
  - Alguém alegando urgência para acessar local  

- Verificação é mais segura do que confiar  

## Elementos de defesa  

### Processo  
- Implementar políticas de uso aceitável  
- Definir procedimentos para lidar com informações confidenciais  

### Treinamento  
- Realizar treinamentos regulares  
- Garantir que funcionários conheçam políticas e riscos  

### Tecnologia  
- Utilizar softwares de segurança  
- Ex:
  - Filtros de spam  
  - Antimalware  

- Medidas combinadas fortalecem a defesa  

## Cuidado com phishing  

- E-mails de phishing são comuns  

### Como identificar  

- Verificar se você esperava o e-mail  
- Avaliar se a mensagem faz sentido  
- Desconfiar de mensagens boas demais ou urgentes  

- Verificar remetente:
  - É conhecido?  

- Observar saudação:
  - Genérica (ex: “Prezado membro”)  

- Procurar erros:
  - Ortografia  
  - Gramática  

- Analisar solicitação:
  - Links suspeitos  
  - Números falsos  
  - Anexos não solicitados  

- Atenção a pedidos de:
  - Senhas  
  - Dados bancários  

- Observar urgência:
  - Pressão para agir rapidamente  

### Verificação de links  

- Não clicar sem verificar  
- Conferir URL:
  - Começa com https  
  - Não possui erros ou domínios falsos  

## Observação importante  

- Não responder e-mails suspeitos  
- Não clicar em links  
- Não abrir anexos  

- Denunciar como spam  

- Em caso de dúvida:
  - Contatar remetente por canal confiável  
  - Usar contatos previamente conhecidos  
- ---

## **Objetivos do aprendizado  

Depois de concluir este módulo, você será capaz de: 

- Definir informações de fontes abertas (OSINT)  
- Explicar os benefícios do OSINT versus fontes alternativas de inteligência  
- Identificar fontes de informações abertas  
- Descrever diretrizes para coletar informações abertas  
- Explicar as razões pelas quais todos deveriam se preocupar com OSINT  
- Coletar OSINT  

---

## Modulo 6 - Informações de fontes abertas  

## Conceito  

- Inteligência de código aberto (OSINT) é a inteligência coletada de fontes publicamente disponíveis  

- Fontes incluem:
  - Blogs  
  - Sites de notícias  
  - Outros sites públicos  

- Qualquer pessoa com acesso à internet pode coletar OSINT:
  - Jornalistas  
  - Pesquisadores  
  - Atacantes  

- Não requer métodos ativos:
  - Não envolve hackeamento  
  - Não envolve escutas telefônicas  

## Introdução  

- O interesse em OSINT cresceu na última década  
- Utilizado tanto no setor:
  - Governamental  
  - Privado  

- OSINT pode ser coletado facilmente a partir de fontes públicas  

## Uso por atacantes  

- Atacantes utilizam OSINT para:
  - Coletar informações sobre pessoas ou organizações  
  - Realizar reconhecimento  

- Essas informações são usadas em ataques maiores  

## Objetivos do modulo  

- Entender o que é OSINT  
- Identificar fontes de informação abertas  
- Compreender como o OSINT é utilizado  
- Entender os riscos para pessoas e organizações  

## Resumo  

- OSINT é baseado em informações públicas  
- Pode ser coletado por qualquer pessoa com internet  
- Não exige técnicas invasivas  
- É amplamente usado para reconhecimento em ataques  
---

## Informações de fontes abertas versus alternativas  

## Conceito  

- Métodos tradicionais de coleta de informações podem ser:
  - Caros  
  - Complexos  
  - Ilegais  

- Exemplos:
  - Hackear bancos de dados privados  
  - Campanhas de phishing  
  - Uso de malware para monitoramento  

- Informações de fontes abertas (OSINT):
  - Mais fáceis de obter  
  - Podem ser gratuitas  

## Exemplo 1  

- Objetivo: descobrir a localização de uma pessoa  

- Método tradicional:
  - Instalar malware no celular  
  - Obter coordenadas de GPS  

- Método OSINT:
  - Monitorar redes sociais  
  - Usar:
    - Postagens com localização  
    - Fotos com pontos de referência  

- Observação:
  - Técnicas semelhantes são usadas por:
    - Militares  
    - Polícia  

## Benefício do OSINT  

- Pode ser indetectável para a vítima  

## Exemplo 2  

- Objetivo: coletar informações sobre sistemas de uma usina  

- Método tradicional:
  - Varredura de rede  
  - Pode ser detectado  

- Método OSINT:
  - Encontrar informações em blogs  
  - Ex: engenheiro discutindo planos  

- Observação:
  - A empresa pode não detectar essa coleta  
---

## Fontes abertas de informações  

## Conceito  

- Invasores utilizam diversas fontes públicas para coletar informações  
- Existem muitas fontes e novas surgem constantemente  

## Fontes comuns  

### Sites de empresas  
- Fonte básica de informações  
- Pode revelar:
  - Contatos  
  - Redes sociais  
  - Endereços  

- Empresas podem divulgar mais do que deveriam  

### Pesquisa avançada (Google)  
- Uso de técnicas de busca avançada  
- Pode encontrar:
  - Informações ocultas  
  - Arquivos expostos  

- Uso de ferramentas como:
  - Máquina de retorno (Wayback Machine)  
  - Permite ver versões antigas de sites  

### Conteúdo jornalístico e análises  
- Jornalistas processam informações abertas  
- Artigos podem fornecer dados úteis  
- Outras fontes:
  - Analistas do setor  
  - Agências de classificação  

### Redes sociais  
- Pessoas compartilham muitas informações  
- Pode revelar:
  - Vida pessoal  
  - Vida profissional  

- Exemplos de exposição:
  - Fotos de crachás  
  - Diagramas de rede  
  - Senhas em post-its  

- Informações ajudam em ataques:
  - Ex: spear phishing baseado em eventos reais  

### Registros públicos  

- Países mantêm dados sobre cidadãos e empresas  
- Pode incluir:
  - Local e data de nascimento  
  - Endereço  

- Outras fontes:
  - Registros hospitalares  
  - Registros eleitorais  
  - Informações financeiras (ex: bolsas de valores)  

### Plataformas online  

- Ex:
  - Reddit  
  - Quora  
  - Fóruns técnicos  

- Usuários compartilham:
  - Experiências  
  - Conhecimento  
  - Informações sensíveis sem perceber  

### Postagens de emprego  

- Podem revelar:
  - Softwares utilizados  
  - Tecnologias da empresa  

- Podem indicar:
  - Vulnerabilidades possíveis  
  - Estrutura organizacional  
  - Projetos futuros  

### Registros DNS e WHOIS  

- Ferramentas permitem consultar:
  - Dados de domínio  
  - Informações de contato  

- Pode revelar:
  - Infraestrutura de rede  
  - Subdomínios  

---

## **Objetivos do aprendizado  

Depois de concluir este módulo, você deverá ser capaz de: 

- Explicar o objetivo e as informações fornecidas por técnicas de varredura típicas  
- Fazer reconhecimento de rede com ferramentas de varredura de rede  

---

## Modulo 7 - Varredura técnica  

## Introdução  

- Neste módulo, você aprenderá sobre técnicas de varredura  
- Entenderá por que os atacantes utilizam essas técnicas  

- A varredura é utilizada por agentes de ameaças durante:
  - Estágio de reconhecimento  
  - Coleta de informações em um ataque  