A evidência digital é qualquer informação relevante para uma investigação que seja armazenada, transmitida ou recebida por dispositivos eletrônicos. Com a variedade de dispositivos atuais (computadores, redes e telemóveis), essas evidências podem assumir muitos formatos e origens. No processo de perícia digital, a fase de identificação busca localizar onde a evidência está e em qual formato.

As evidências digitais podem ser agrupadas em três grandes categorias:

**1. Evidência via Computadores**  
Inclui arquivos e dados armazenados em discos rígidos e outros meios digitais, como e-mails, registros de chat, imagens, vídeos, áudios e documentos. Esses dados podem estar visíveis ou ocultos, por exemplo, em espaço de folga do disco ou por meio de esteganografia. São comuns em investigações internas e criminais.

**2. Evidência via Rede**  
Relaciona-se à atividade online, como histórico de navegação, registros de proxies e roteadores, acessos a sites e uso de redes sociais. Postagens, mensagens instantâneas e interações online podem ser usadas como prova, desde que haja vínculo confiável com o usuário investigado.

**3. Evidência via Telemóveis**  
Os smartphones concentram grande volume de informações pessoais e de uso diário, como histórico de chamadas, mensagens, contatos, fotos, vídeos, aplicativos, dados de localização e navegação. Mesmo dados apagados podem ser recuperados com ferramentas forenses especializadas.

Em conjunto, essas fontes formam a base da investigação forense digital moderna.

---

## Cadeia de Custódia
A Cadeia de Custódia é o processo que garante o controle e a integridade das evidências físicas ou digitais desde a sua recolha até a apresentação em juízo. Se não for corretamente seguida, a prova pode ser considerada inválida e inadmissível em tribunal.

Os elementos básicos da cadeia de custódia incluem o registo de:

- quem entregou a evidência;
- quem a recebeu;
- a data;
- o horário.

Seguir rigorosamente esse processo assegura que a evidência não foi adulterada e mantém a sua credibilidade legal.

Para garantir a cadeia de custódia, os analistas forenses adotam boas práticas como:

- manter registos claros de quem coletou a evidência e quem é responsável por ela em cada etapa;
- nunca trabalhar diretamente na evidência original, mas sim numa cópia forense;
- gerar hashes da evidência original e da cópia, garantindo que ambas são idênticas;
- preservar a evidência original sem alterações;
- higienizar completamente os discos forenses antes de copiar qualquer dado, evitando contaminação da prova.

---

# Steganography
Esteganografia é a prática de ocultar mensagens ou informações secretas dentro de dados aparentemente inofensivos, como imagens, áudios ou outros arquivos comuns. Um exemplo típico é esconder um arquivo de texto confidencial dentro de uma imagem, que aparenta ser normal quando enviada, por exemplo, como anexo de e-mail. Apenas com ferramentas adequadas é possível extrair o conteúdo oculto.

Além disso, informações secretas também podem ser inseridas nos metadados de arquivos, na forma de sequências de texto invisíveis ao utilizador comum.

A compreensão da esteganografia é essencial em contextos forenses e de segurança, pois permite identificar e analisar arquivos que podem conter dados ocultos indevidamente. O módulo _Challenge_ do curso baseia-se justamente na identificação dessas informações escondidas, tornando fundamental entender como detectar esse tipo de técnica.

### Conceitos Importantes

- **Arquivo de capa (Cover File)**: imagem ou áudio usado para esconder dados.
- **Arquivo secreto (Embedded File)**: arquivo que será ocultado (ex.: `.zip`, `.txt`).
- **Frase-senha (Passphrase)**: protege a extração dos dados ocultos.
- O arquivo de capa **continua aparentemente normal** após a incorporação.

---

### Exemplo Prático – Preparação do Arquivo Secreto

- Criar um arquivo compactado com o conteúdo secreto:
    - `zip -r secret.zip secret`
- Resultado:
    - `secret.zip` contendo arquivos confidenciais (ex.: `1.txt`)

---

### Incorporar Arquivo Oculto (Embed)

Comando:

`steghide embed -cf laptop.jpg -ef secret.zip`

Significado dos parâmetros:

- `steghide` → ferramenta utilizada
- `embed` → modo de incorporação
- `-cf laptop.jpg` → arquivo de cap
- `-ef secret.zip` → arquivo secreto a ser ocultado

Observações:

- Será solicitada uma **frase-senha**
- O arquivo original pode ser sobrescrito
- Para criar um novo arquivo stego:
    - Usar `-sf laptop2.jpg`

---

### Extrair Arquivo Oculto (Extract)

Comando:

`steghide extract -sf laptop2.jpg`

Significado dos parâmetros:

- `extract` → modo de extração
- `-sf laptop2.jpg` → arquivo stego

Observações:

- A ferramenta pedirá a **frase-senha**
- O arquivo oculto (ex.: `secret.zip`) será extraído no diretório atual

---

### Importância Forense

- Arquivos aparentemente inofensivos podem conter dados ocultos
- Fundamental verificar:
    - Tamanho do arquivo
    - Metadados
    - Presença de esteganografia
- Essencial para **investigações digitais** e **desafios práticos do curso**