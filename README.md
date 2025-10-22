# BIP39WalletGen_SplitedRecovery

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Seed Generator - Passphrase - BIP39

## Visão Geral

Este projeto é uma aplicação web que permite gerar, validar e gerar hashes para "seeds" (palavras de recuperação) de carteiras de criptomoedas, seguindo o padrão BIP39. A aplicação foi projetada para permitir o compartilhamento seguro de seeds em blocos, onde cada bloco pode ser mantido por uma pessoa diferente, e somente com a combinação dos hashes de todos os blocos é possível gerar a passphrase final.

## Funcionalidades

- **Geração de palavras aleatórias**: Compatível com as listas BIP39 de 12, 18 ou 24 palavras
- **Validação de palavras**: Verifica se cada palavra pertence à lista BIP39
- **Cálculo de hash**: Gera hashes MD5 para blocos de 6 palavras
- **Visualização organizada**: Interface dividida em colunas para fácil visualização
- **Sistema de cópia**: Clique em qualquer campo para copiar para a área de transferência
- **Verificador de hash avulso**: Permite calcular hash de qualquer string
- **Toggle de visibilidade**: Mostra/oculta valores sensíveis

## Estrutura do Projeto

```
Seeds/
├── Passphrase - BIP39 - 12-18-24 Seeds.html  # Aplicação principal
├── wordlist_bip39.js                         # Lista de palavras BIP39
└── crypto-js-develop/                        # Biblioteca de criptografia
    └── src/
        ├── core.js                           # Componentes principais
        └── md5.js                            # Implementação do algoritmo MD5
```

## Como Funciona

### 1. Sistema de Blocos de 6 Palavras

A aplicação utiliza uma sistemática especial para dividir e proteger seeds:

```
Seed Completo: [P1] [P2] [P3] [P4] [P5] [P6] [P7] [P8] [P9] [P10] [P11] [P12]
                |                          ||                               |
                └─────── BLOCO 1 ──────────┘└────────── BLOCO 2 ────────────┘

Bloco 1: String = "P1 P2 P3 P4 P5 P6"
         Hash1 = MD5("P1 P2 P3 P4 P5 P6")

Bloco 2: String = "P7 P8 P9 P10 P11 P12"
         Hash2 = MD5("P7 P8 P9 P10 P11 P12")

Passphrase Final: HashFinal = MD5(Hash1 + Hash2)
```

### 2. Organograma do Sistema de Segurança

```mermaid
graph TD
    A[Usuário Gera Seed Completo] --> B[Divide em Blocos de 6 Palavras]
    B --> C[Bloco 1: P1-P6]
    B --> D[Bloco 2: P7-P12]
    C --> E[Pessoa/Local 1 recebe Bloco 1 e Hash1]
    D --> F[Pessoa/Local 2 recebe Bloco 2 e Hash2]
    E --> G[Armazena de forma segura]
    F --> H[Armazena de forma segura]
    I[Combinação dos Hashes] --> J[Hash1 + Hash2]
    J --> K[MD5(Hash1 + Hash2)]
    K --> L[Passphrase Final]
```

### 3. Processo de Recuperação

Para recuperar a passphrase final:

1. **Coletar os hashes**: Reúna os hashes de todas as partes/pessoas
2. **Concatenar os hashes**: Combine todos os hashes na ordem correta
3. **Gerar a passphrase final**: Aplique MD5 na string concatenada

```
Hashes Coletados: "Hash1 Hash2"
String Concatenada: "Hash1Hash2"
Passphrase Final: MD5("Hash1Hash2")
```

## Como Usar

### 1. Gerar uma Nova Seed

1. Selecione o número de palavras (12, 18 ou 24)
2. Clique em "Gerar Palavras Aleatórias" ou insira manualmente
3. A aplicação validará automaticamente cada palavra
4. Os hashes dos blocos serão calculados automaticamente

### 2. Compartilhar de Forma Segura

1. **Para seeds de 12 palavras (2 blocos)**:
   - Bloco 1: Palavras 1-6 + Hash1
   - Bloco 2: Palavras 7-12 + Hash2

2. **Para seeds de 18 palavras (3 blocos)**:
   - Bloco 1: Palavras 1-6 + Hash1
   - Bloco 2: Palavras 7-12 + Hash2
   - Bloco 3: Palavras 13-18 + Hash3

3. **Para seeds de 24 palavras (4 blocos)**:
   - Bloco 1: Palavras 1-6 + Hash1
   - Bloco 2: Palavras 7-12 + Hash2
   - Bloco 3: Palavras 13-18 + Hash3
   - Bloco 4: Palavras 19-24 + Hash4

### 3. Recuperar a Passphrase

1. Reúna todos os hashes das diferentes partes
2. Concatene os hashes na ordem correta
3. Use o verificador de hash avulso para calcular:
   - Insira a string concatenada: "Hash1Hash2Hash3..."
   - O resultado será a passphrase final

## Exemplo Prático

### Geração

1. Seed gerada: `abandon ability able about absent absorb abstract absurd abuse access accident`
2. Dividida em:
   - Bloco 1: "abandon ability able about absent absorb" → Hash1: `a1b2c3d4e5f6...`
   - Bloco 2: "abstract absurd abuse access accident account" → Hash2: `g7h8i9j0k1l2...`
3. Passphrase final: MD5("a1b2c3d4e5f6...g7h8i9j0k1l2...") = `z9x8y7w6v5u4...`

### Recuperação

1. Coleta dos hashes: "a1b2c3d4e5f6..." + "g7h8i9j0k1l2..."
2. String para hash: "a1b2c3d4e5f6...g7h8i9j0k1l2..."
3. Hash resultante: `z9x8y7w6v5u4...` (Passphrase final)

## Considerações de Segurança

- **Validação básica**: A aplicação valida apenas se as palavras existem na lista BIP39, mas não implementa a verificação de checksum completa do padrão.
- **Proteção de dados**: Armazene os blocos e hashes em locais seguros e separados.
- **Ordem dos blocos**: A ordem dos blocos é crucial para a recuperação correta da passphrase.

## Tecnologias Utilizadas

- **HTML5**: Estrutura da aplicação
- **CSS3**: Estilização e design responsivo
- **JavaScript (ES6+)**: Lógica de negócio e interações
- **CryptoJS**: Biblioteca de criptografia para hash MD5
- **BIP39**: Lista padrão de palavras para recuperação de carteiras

## Observações

1. Recomenda-se o uso deste projeto em um ambiente totalmente offline, preferivelmente num sistema amnésico (ex: Tails). 
2. Recomenda-se fortemente que você NÃO SALVE SUAS SEEDS E PASSPHRASE EM ARQUIVO DIGITAL, ESPECIALMENTE DE DISPOSIVITOS QUE POSSUAM CONEXÃO COM A INTERNET.

## Utilização Offline (Tails OS)

Esta aplicação foi projetada para funcionar completamente offline, tornando-a ideal para uso em sistemas como o Tails OS, onde a privacidade e segurança são primordiais.

### Pré-requisitos

- Um navegador web moderno (Chrome, Firefox, Edge, etc.)
- Nenhuma conexão com internet necessária
- Sistema operacional que suporte arquivos HTML e JavaScript

### Passo a Passo para Utilização no Tails OS

#### 1. Preparação do Pendrive

1. **Baixar o projeto do GitHub**:
   - Acesse https://github.com/belarmino85/BIP39WalletGen_SplitedRecovery
   - Clique em "Code" → "Download ZIP"
   - Ou use `git clone` se estiver em um sistema com git

2. **Estrutura de arquivos necessária**:
   ```
   seed-generator/
   ├── Passphrase - BIP39 - 12-18-24 Seeds.html
   ├── wordlist_bip39.js
   └── crypto-js-develop/
       └── src/
           ├── core.js
           └── md5.js
   ```

3. **Copiar para pendrive**:
   - Extraia o arquivo ZIP pendrive
   - Verifique se todos os arquivos e pastas estão presentes
   - Garanta que o pendrive seja formatado em FAT32 ou exFAT para compatibilidade

#### 2. Configuração do Tails OS

1. **Iniciar o Tails**:
   - Ligue o computador com o pendrive inserido
   - Reinicie e pressione F12 (ou a tecla de boot do seu sistema)
   - Selecione o pendrive como dispositivo de boot
   - Inicie o Tails com as configurações padrão

2. **Verificar armazenamento persistente**:
   - Se você tiver armazenamento persistente configurado no Tails, use-o
   - Caso contrário, o sistema funcionará em modo "live" e não salvará alterações

#### 3. Abrindo a Aplicação

1. **Navegador seguro**:
   - No Tails, o navegador padrão é o Tor Browser
   - Abra o Tor Browser a partir do menu de aplicativos

2. **Abrir arquivo local**:
   - Pressione `Ctrl+O` para abrir arquivo
   - Navegue até o pendrive (geralmente em `/media/amnesiac/` ou similar)
   - Selecione o arquivo `Passphrase - BIP39 - 12-18-24 Seeds.html`
   - Ou arraste o arquivo diretamente para a janela do navegador

3. **Confirmar permissões**:
   - O navegador pode perguntar permissão para acessar arquivos locais
   - Clique em "Permitir" ou "Allow"
   - A aplicação deve carregar completamente sem erros

#### 4. Usando a Aplicação Offline

1. **Gerando uma nova seed**:
   - Selecione o número de palavras (12, 18 ou 24)
   - Clique em "Gerar Palavras Aleatórias"
   - Não é necessária nenhuma conexão com internet

2. **Validando palavras**:
   - Insira palavras manualmente ou copie-as de outra fonte
   - A validação ocorre instantaneamente usando a lista local

3. **Trabalhando com blocos**:
   - Divida a seed em blocos de 6 palavras
   - Cada bloco gera seu hash automaticamente
   - Anote ou imprima os hashes para distribuição segura

4. **Recuperando a passphrase**:
   - Use o verificador de hash avulso
   - Concatene os hashes coletados
   - Calcule a passphrase final sem conexão com internet

### Dicas de Segurança no Tails OS

1. **Desconexão de rede**:
   - Pressione `Ctrl+Shift+Q` para desligar completamente a rede
   - Confirme que você está offline com o botão "Disconnect" no canto superior direito

2. **Armazenamento temporário**:
   - O Tails usa RAM temporária
   - Feche o navegador ao terminar para limpar dados sensíveis
   - Use o "Shutdown" em vez de "Restart" ao finalizar

3. **Verificação de integridade**:
   - Antes de usar, verifique se todos os arquivos estão presentes
   - Abra o arquivo HTML diretamente do pendrive
   - Teste a geração de palavras aleatórias para confirmar que tudo funciona

4. **Backup seguro**:
   - Mantenha uma cópia do projeto em múltiplos pendrives
   - Use pendrives criptografados se possível
   - Armazene os pendrives em locais seguros e separados

### Solução de Problemas Comuns

#### Problema: Arquivos não encontram os scripts
- **Solução**: Verifique se a estrutura de pastas está correta
- **Verifique**: Os arquivos `core.js` e `md5.js` devem estar em `crypto-js-develop/src/`

#### Problema: A página não carrega
- **Solução**: Tente abrir o arquivo com um navegador diferente
- **Alternativa**: No Tails, use o Firefox padrão além do Tor Browser

#### Problema: Botões não funcionam
- **Solução**: Atualize a página (`Ctrl+R`)
- **Causa comum**: O arquivo pode ter sido corrompido durante a transferência

#### Problema: Lista de palavras não carrega
- **Solução**: Verifique se `wordlist_bip39.js` está no mesmo diretório
- **Dica**: Abra o console do navegador (`Ctrl+Shift+I`) para ver erros

### Considerações Finais

O uso offline oferece máxima privacidade, pois:
- Nenhuma informação é enviada para servidores externos
- A geração de números aleatórios é feita localmente
- Todas as operações de hash ocorrem no navegador
- Não há logs ou histórico de uso

Ao seguir este guia, você pode usar o Seed Generator de forma segura e privada em ambientes como o Tails OS, garantindo que suas seeds de criptomoedas sejam geradas e gerenciadas sem riscos de vazamento de dados.

## Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

### Disclaimer de Responsabilidade

**ATENÇÃO: LEIA COM ATENÇÃO ANTES DE USAR ESTE PROJETO**

Este projeto é uma ferramenta educacional para demonstração de conceitos de geração e gerenciamento de seeds para carteiras de criptomoedas. É fornecido "como está", sem qualquer garantia explícita ou implícita.

**Responsabilidade do Usuário**:
- Você é o único responsável pela segurança de suas seeds e passphrases
- Este projeto não deve ser usado para armazenar ou gerencir fundos reais sem uma avaliação de segurança completa
- O desenvolvedor não se responsabiliza por qualquer perda, roubo ou acesso não autorizado a ativos digitais
- Use por sua conta e risco, após compreender completamente os riscos envolvidos

**Limitações**:
- Esta ferramenta usa MD5, que não é considerado seguro para aplicações que requerem alta segurança
- A validação de palavras é básica e não implementa a verificação de checksum completa do padrão BIP39
- O armazenamento digital de seeds e passphrases é inerentemente inseguro

**Recomendações de Segurança**:
- Nunça salve seeds ou passphrases em arquivos digitais, especialmente em dispositivos com conexão à internet
- Use sempre sistemas operacionais focados em privacidade (como Tails OS) para operações sensíveis
- Mantenha cópias físicas de seeds e hashs em locais seguros e separados
- Considere usar soluções de hardware wallet para armazenamento de longo prazo

**ATENÇÃO**:

Ao usar este projeto, você concorda que o desenvolvedor não será responsabilizado por quaisquer danos diretos, indiretos, incidentais, especiais, consequentes ou exemplares resultantes do uso ou da incapacidade de usar este projeto.
