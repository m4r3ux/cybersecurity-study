A caça às ameaças (Threat Hunting) é a prática proativa de identificar ataques cibernéticos que permanecem ocultos em uma rede antes de serem detectados por sistemas automáticos, como SIEMs ou firewalls. Diferente do trabalho tradicional em SOC, que depende de alertas e pode gerar falsos positivos e fadiga mental, o caçador de ameaças foca na análise das **técnicas do atacante**, investigando padrões e comportamentos suspeitos de forma ativa.

Essa abordagem permite detectar ameaças avançadas que escapam das ferramentas tradicionais e exige uma mentalidade investigativa, capaz de formular hipóteses sobre possíveis ataques. Enquanto o analista de SOC atua de forma reativa, a caça às ameaças é **proativa e analítica**, essencial para proteger organizações modernas de incidentes críticos.

---

O aumento global de ataques cibernéticos reforça a necessidade de caçadores de ameaças. Ter profissionais desse tipo na equipe melhora a qualidade geral da cibersegurança, permitindo que organizações identifiquem e neutralizem ataques que a tecnologia sozinha não consegue detectar.

A SANS, empresa americana de segurança da informação, destaca a **caça às ameaças como uma das carreiras mais promissoras** no setor. Além disso, estudos da Lacework mostram que quase 90% das organizações concordam que essa prática deve ser prioridade máxima em segurança, já que apenas tecnologia não é suficiente para sinalizar e eliminar todas as ameaças potenciais.

A ideia central é que, enquanto os sistemas automáticos não forem perfeitos, **seres humanos capacitados com ferramentas e expertise corretas são essenciais para proteger organizações**.

---

## Gerando indicadores - Indicadores de Compromisso (IOCs)
**Definição:** Artefatos usados para identificar atividades maliciosas e compartilhar informações entre organizações. Servem para **detecção, resposta e prevenção**.

 Exemplos comuns:
- **IP:** endereços maliciosos detectados em ataques (ex.: DDoS).
- **E-mail:** remetentes de phishing ou spam.
- **Tamanho de arquivo:** arquivos maliciosos podem ter tamanhos específicos.
- **Hashes (MD5/SHA):** assinatura única de arquivos; qualquer alteração muda o valor.
- **Strings:** sequências de texto específicas de malware (ex.: `Lun4rSquad`).

**Dica:** IOCs devem ser usados como ponto de partida, correlacionados com contexto para reduzir falsos positivos.

## Gerando Indicadores de Propriedades de Arquivo

**Objetivo:** Extrair IOCs básicos de arquivos maliciosos, como **nome e tamanho**.

### 1. Tamanho do arquivo

- **Linux:** `ls -lh` no diretório do arquivo mostra tamanho em bytes.
- **GUI Linux/Windows:** clique com o botão direito → _Propriedades_ → _Size_.

### 2. Nome do arquivo

- **Método simples:** visualizar ícone ou propriedades do arquivo.
- **Linha de comando:** listar diretório e copiar o nome
---
