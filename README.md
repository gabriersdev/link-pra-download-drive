# Link para Download - Google Drive

Ferramenta simples para gerar links de download ou visualização diretos a partir de links de compartilhamento ou IDs do Google Drive, usando apenas HTML, CSS e JavaScript puro (sem frameworks).

## 📋 Sobre o Projeto

Este projeto fornece uma solução básica para converter links de compartilhamento do Google Drive em links diretos de download ou visualização. Ideal para facilitar o acesso a arquivos compartilhados sem a necessidade de abrir a interface do Google Drive.

## 🎯 Funcionalidades

- ✅ Converter link de compartilhamento do Google Drive em link de download direto
- ✅ Converter link de compartilhamento do Google Drive em link de visualização direto
- ✅ Extrair ID do arquivo a partir de diferentes formatos de URL do Google Drive
- ✅ Interface simples usando apenas HTML, CSS e JavaScript
- ✅ Sem dependências de frameworks ou bibliotecas externas

## 🚀 Como Funciona

### Formatos de Link do Google Drive

O Google Drive usa diferentes formatos de URL para compartilhamento:

```
https://drive.google.com/file/d/FILE_ID/view?usp=sharing
https://drive.google.com/open?id=FILE_ID
https://drive.google.com/uc?id=FILE_ID
```

### Conversão para Links Diretos

**Link de Download Direto:**
```
https://drive.google.com/uc?export=download&id=FILE_ID
```

**Link de Visualização Direta:**
```
https://drive.google.com/uc?export=view&id=FILE_ID
```

**Link de Preview (Visualização Embutida):**
```
https://drive.google.com/file/d/FILE_ID/preview
```

## 💻 Implementação Básica

### HTML

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Link - Google Drive</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Gerador de Link do Google Drive</h1>
        
        <div class="input-group">
            <label for="driveLink">Cole o link ou ID do Google Drive:</label>
            <input type="text" id="driveLink" placeholder="https://drive.google.com/file/d/... ou apenas o ID">
        </div>
        
        <div class="button-group">
            <button onclick="generateDownloadLink()">Gerar Link de Download</button>
            <button onclick="generateViewLink()">Gerar Link de Visualização</button>
            <button onclick="generatePreviewLink()">Gerar Link de Preview</button>
        </div>
        
        <div id="result" class="result"></div>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
```

### CSS (style.css)

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.container {
    background: white;
    padding: 40px;
    border-radius: 10px;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    max-width: 600px;
    width: 100%;
}

h1 {
    color: #333;
    margin-bottom: 30px;
    text-align: center;
}

.input-group {
    margin-bottom: 20px;
}

label {
    display: block;
    margin-bottom: 8px;
    color: #555;
    font-weight: bold;
}

input[type="text"] {
    width: 100%;
    padding: 12px;
    border: 2px solid #ddd;
    border-radius: 5px;
    font-size: 14px;
    transition: border-color 0.3s;
}

input[type="text"]:focus {
    outline: none;
    border-color: #667eea;
}

.button-group {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 20px;
}

button {
    flex: 1;
    min-width: 150px;
    padding: 12px 20px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 14px;
    cursor: pointer;
    transition: background 0.3s;
}

button:hover {
    background: #5568d3;
}

.result {
    padding: 15px;
    background: #f5f5f5;
    border-radius: 5px;
    min-height: 50px;
    word-wrap: break-word;
}

.result.success {
    background: #d4edda;
    border: 1px solid #c3e6cb;
    color: #155724;
}

.result.error {
    background: #f8d7da;
    border: 1px solid #f5c6cb;
    color: #721c24;
}

.result a {
    color: #667eea;
    text-decoration: none;
    font-weight: bold;
}

.result a:hover {
    text-decoration: underline;
}
```

### JavaScript (script.js)

```javascript
/**
 * Extrai o ID do arquivo do Google Drive de diferentes formatos de URL
 * @param {string} input - Link do Google Drive ou ID direto
 * @returns {string|null} - ID do arquivo ou null se não encontrado
 */
function extractFileId(input) {
    // Remove espaços em branco
    input = input.trim();
    
    // Se for apenas o ID (string alfanumérica)
    if (/^[a-zA-Z0-9_-]+$/.test(input)) {
        return input;
    }
    
    // Padrões de URL do Google Drive
    const patterns = [
        /\/file\/d\/([a-zA-Z0-9_-]+)/,           // /file/d/ID
        /id=([a-zA-Z0-9_-]+)/,                   // id=ID
        /\/folders\/([a-zA-Z0-9_-]+)/,           // /folders/ID
        /^([a-zA-Z0-9_-]+)$/                     // apenas ID
    ];
    
    for (const pattern of patterns) {
        const match = input.match(pattern);
        if (match && match[1]) {
            return match[1];
        }
    }
    
    return null;
}

/**
 * Gera link de download direto
 */
function generateDownloadLink() {
    const input = document.getElementById('driveLink').value;
    const fileId = extractFileId(input);
    
    if (!fileId) {
        showResult('Erro: Não foi possível extrair o ID do arquivo. Verifique o link ou ID fornecido.', 'error');
        return;
    }
    
    const downloadLink = `https://drive.google.com/uc?export=download&id=${fileId}`;
    showResult(`Link de download gerado com sucesso!<br><br><a href="${downloadLink}" target="_blank">${downloadLink}</a><br><br><button onclick="copyToClipboard('${downloadLink}')">Copiar Link</button>`, 'success');
}

/**
 * Gera link de visualização direta
 */
function generateViewLink() {
    const input = document.getElementById('driveLink').value;
    const fileId = extractFileId(input);
    
    if (!fileId) {
        showResult('Erro: Não foi possível extrair o ID do arquivo. Verifique o link ou ID fornecido.', 'error');
        return;
    }
    
    const viewLink = `https://drive.google.com/uc?export=view&id=${fileId}`;
    showResult(`Link de visualização gerado com sucesso!<br><br><a href="${viewLink}" target="_blank">${viewLink}</a><br><br><button onclick="copyToClipboard('${viewLink}')">Copiar Link</button>`, 'success');
}

/**
 * Gera link de preview (visualização embutida)
 */
function generatePreviewLink() {
    const input = document.getElementById('driveLink').value;
    const fileId = extractFileId(input);
    
    if (!fileId) {
        showResult('Erro: Não foi possível extrair o ID do arquivo. Verifique o link ou ID fornecido.', 'error');
        return;
    }
    
    const previewLink = `https://drive.google.com/file/d/${fileId}/preview`;
    showResult(`Link de preview gerado com sucesso!<br><br><a href="${previewLink}" target="_blank">${previewLink}</a><br><br><button onclick="copyToClipboard('${previewLink}')">Copiar Link</button>`, 'success');
}

/**
 * Exibe o resultado na tela
 * @param {string} message - Mensagem a ser exibida
 * @param {string} type - Tipo da mensagem (success ou error)
 */
function showResult(message, type) {
    const resultDiv = document.getElementById('result');
    resultDiv.innerHTML = message;
    resultDiv.className = `result ${type}`;
}

/**
 * Copia texto para a área de transferência
 * @param {string} text - Texto a ser copiado
 */
function copyToClipboard(text) {
    navigator.clipboard.writeText(text).then(() => {
        alert('Link copiado para a área de transferência!');
    }).catch(err => {
        console.error('Erro ao copiar:', err);
        alert('Erro ao copiar o link. Por favor, copie manualmente.');
    });
}
```

## 📖 Como Usar

1. **Clone ou baixe este repositório**
   ```bash
   git clone https://github.com/gabriersdev/link-pra-download-drive.git
   ```

2. **Abra o arquivo `index.html` no navegador**
   - Não é necessário servidor web
   - Funciona diretamente abrindo o arquivo HTML

3. **Cole um link ou ID do Google Drive**
   - Exemplo de link: `https://drive.google.com/file/d/1ABC123xyz/view?usp=sharing`
   - Exemplo de ID: `1ABC123xyz`

4. **Clique no botão desejado:**
   - **Gerar Link de Download**: Para baixar o arquivo diretamente
   - **Gerar Link de Visualização**: Para visualizar o arquivo no navegador
   - **Gerar Link de Preview**: Para visualizar o arquivo embutido (útil para iframes)

5. **Copie o link gerado** e use onde precisar

## 🔍 Exemplos de Uso

### Exemplo 1: Converter Link de Compartilhamento

**Entrada:**
```
https://drive.google.com/file/d/1a2b3c4d5e6f7g8h9i0j/view?usp=sharing
```

**Saída (Download):**
```
https://drive.google.com/uc?export=download&id=1a2b3c4d5e6f7g8h9i0j
```

### Exemplo 2: Usar Apenas o ID

**Entrada:**
```
1a2b3c4d5e6f7g8h9i0j
```

**Saída (Visualização):**
```
https://drive.google.com/uc?export=view&id=1a2b3c4d5e6f7g8h9i0j
```

### Exemplo 3: Preview para Iframe

**Entrada:**
```
https://drive.google.com/open?id=1a2b3c4d5e6f7g8h9i0j
```

**Saída (Preview):**
```
https://drive.google.com/file/d/1a2b3c4d5e6f7g8h9i0j/preview
```

**Uso em iframe:**
```html
<iframe src="https://drive.google.com/file/d/1a2b3c4d5e6f7g8h9i0j/preview" width="640" height="480"></iframe>
```

## ⚠️ Limitações e Considerações

- **Permissões**: O arquivo no Google Drive deve estar configurado como "Qualquer pessoa com o link" para que os links diretos funcionem
- **Tamanho do arquivo**: Arquivos muito grandes podem exigir confirmação adicional do Google Drive
- **Tipo de arquivo**: Alguns tipos de arquivo podem não suportar visualização direta
- **Quota do Google**: Downloads massivos podem atingir limites de quota do Google Drive

## 🛠️ Tecnologias Utilizadas

- **HTML5**: Estrutura da página
- **CSS3**: Estilização e design responsivo
- **JavaScript (ES6+)**: Lógica de extração de ID e geração de links
- **Sem frameworks**: Código puro, sem dependências externas

## 📁 Estrutura do Projeto

```
link-pra-download-drive/
│
├── index.html          # Página principal
├── style.css           # Estilos CSS
├── script.js           # Lógica JavaScript
└── README.md           # Documentação
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para:

1. Fazer um fork do projeto
2. Criar uma branch para sua feature (`git checkout -b feature/NovaFuncionalidade`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova funcionalidade'`)
4. Push para a branch (`git push origin feature/NovaFuncionalidade`)
5. Abrir um Pull Request

## 📝 Licença

Este projeto é open source e está disponível para uso livre.

## 👨‍💻 Autor

Desenvolvido com ❤️ para facilitar o compartilhamento de arquivos do Google Drive.

---

**Nota**: Este projeto é uma ferramenta educacional e de utilidade. Certifique-se de respeitar as políticas de uso do Google Drive e os direitos autorais dos arquivos compartilhados.
