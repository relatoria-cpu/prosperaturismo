# Guia de Instalação — Sistema Prospera Turismo

**Diagnóstico Único: Diagnóstico Empresarial e Diagnóstico de Turismo Comunitário**
Instituto BR Arte

---

## Sumário

1. Visão geral do sistema
2. Onde cada arquivo deve ficar
3. Antes de começar
4. Passo 1 — Criar a planilha Google Sheets
5. Passo 2 — Criar a pasta de relatórios no Google Drive
6. Passo 3 — Configurar o Google Apps Script
7. Passo 4 — Configurar as constantes no Code.gs
8. Passo 5 — Inserir as funções dos PDFs visuais
9. Passo 6 — Executar a instalação do sistema
10. Passo 7 — Publicar como Web App
11. Passo 8 — Configurar o index.html com a URL do Web App
12. Passo 9 — Subir os arquivos no GitHub e ativar o GitHub Pages
13. Passo 10 — Teste obrigatório do formulário
14. Passo 11 — Gerar relatórios pela planilha
15. Erros comuns e como resolver
16. Checklist final de instalação
17. Observação importante sobre o GitHub

---

## ARQUIVOS QUE PRECISAM SER COLOCADOS MANUALMENTE

O sistema nao pode gerar as logomarcas automaticamente. Voce precisa providenciar os tres arquivos de imagem abaixo e coloca-los exatamente com estes nomes e nesta estrutura de pasta:

```
assets/
├── logo-instituto-br.png
├── logo-prospera-turismo.png
└── logo-prefeitura-rio.png
```

**Descricao de cada arquivo:**

| Arquivo | Conteudo esperado |
|---|---|
| `logo-instituto-br.png` | Logomarca do Instituto BR Arte em formato PNG |
| `logo-prospera-turismo.png` | Logomarca do Prospera Turismo em formato PNG |
| `logo-prefeitura-rio.png` | Logomarca da Prefeitura do Rio de Janeiro em formato PNG |

**Instrucoes:**

1. Obtenha os tres arquivos de logomarca com a equipe de comunicacao ou com quem forneceu o sistema.
2. Renomeie cada arquivo com o nome exato indicado acima (letras minusculas, hifens, extensao `.png`).
3. Crie uma pasta chamada `assets` (minusculo, sem acentos) na mesma pasta onde esta o `index.html`.
4. Coloque os tres arquivos dentro da pasta `assets`.
5. Quando for subir no GitHub, suba a pasta `assets` junto com o `index.html`.

**Atencao importante:**

As logomarcas aparecem no formulario (GitHub Pages) usando caminho relativo (`assets/logo-...png`). Isso funciona quando os arquivos estao no repositorio correto.

Nos PDFs visuais gerados pelo Apps Script, as logomarcas precisam de URL publica completa. Configure as constantes `LOGO_INSTITUTO`, `LOGO_PROSPERA` e `LOGO_PREFEITURA` no `Code.gs` com os enderecos completos do GitHub Pages apos a publicacao (conforme explicado no Passo 4).

Se voce nao tiver as logomarcas disponíveis no momento da instalacao, o sistema funciona normalmente sem elas. As imagens vao aparecer como espacos em branco até que sejam adicionadas.


---

## 1. Visão geral do sistema

O sistema funciona em duas partes separadas que trabalham juntas.

**Parte 1 — GitHub Pages (o formulário público)**

O GitHub Pages hospeda apenas o formulário que o entrevistador vai preencher. Quando você acessa o link do GitHub Pages, o formulário aparece no navegador. O entrevistador preenche os dados e clica em "Enviar Diagnóstico".

O GitHub Pages não armazena nada. Ele apenas exibe o formulário.

**Parte 2 — Google Apps Script com Google Sheets e Google Drive (o cérebro do sistema)**

Ao clicar em "Enviar Diagnóstico", o formulário envia os dados para um endereço do Google Apps Script. O Apps Script recebe os dados, organiza tudo e grava na planilha do Google Sheets. A partir daí, a pessoa responsável pelo projeto pode:

- selecionar uma linha na planilha;
- abrir o menu "Prospera Turismo";
- gerar relatórios editáveis em Google Docs;
- gerar PDFs a partir do Google Docs;
- gerar PDFs visuais com layout institucional a partir dos templates HTML.

Os arquivos gerados ficam salvos no Google Drive.

**Resumo do fluxo completo:**

```
Entrevistador acessa o formulário no GitHub Pages
       |
       v
Preenche e envia o formulário
       |
       v
Google Apps Script recebe os dados
       |
       v
Dados gravados na aba 01_RESPOSTAS_DIAGNOSTICO_UNICO
       |
       v
Responsável seleciona a linha na planilha
       |
       v
Menu "Prospera Turismo" -> Gerar relatórios
       |
       v
Google Docs e PDFs salvos no Google Drive
```

---

## 2. Onde cada arquivo deve ficar

| Arquivo | Onde colocar | Para que serve |
|---|---|---|
| `index.html` | Repositório do GitHub (pasta raiz) | Formulário público que o entrevistador acessa |
| pasta `assets/` | Repositório do GitHub (pasta raiz) | Pasta com as logomarcas |
| `logo-instituto-br.png` | Pasta `assets/` no GitHub | Logomarca do Instituto BR Arte no formulário |
| `logo-prospera-turismo.png` | Pasta `assets/` no GitHub | Logomarca do Prospera Turismo no formulário |
| `logo-prefeitura-rio.png` | Pasta `assets/` no GitHub | Logomarca da Prefeitura do Rio no formulário |
| `Code.gs` | Google Apps Script (arquivo principal `.gs`) | Backend: recebe dados, grava na planilha, gera relatórios |
| `TemplateEmpresarial.html` | Google Apps Script (arquivo HTML) | Template visual do Diagnóstico Empresarial em PDF |
| `TemplateTurismo.html` | Google Apps Script (arquivo HTML) | Template visual do Diagnóstico de Turismo em PDF |
| `TemplateUnico.html` | Google Apps Script (arquivo HTML) | Template visual do Diagnóstico Único Consolidado em PDF |

---

## 3. Antes de começar

Antes de iniciar a instalação, certifique-se de que você tem:

- Uma conta Google ativa (Gmail).
- Acesso ao Google Drive.
- Acesso ao Google Sheets.
- Acesso ao Google Apps Script (acessado pela planilha, em Extensões > Apps Script).
- Uma conta no GitHub (gratuita, em https://github.com).
- Os arquivos do sistema: `index.html`, `Code.gs`, `TemplateEmpresarial.html`, `TemplateTurismo.html`, `TemplateUnico.html`.
- As três logomarcas em formato PNG com os nomes exatos:
  - `logo-instituto-br.png`
  - `logo-prospera-turismo.png`
  - `logo-prefeitura-rio.png`

---

## 4. Passo 1 — Criar a planilha Google Sheets

A planilha é onde todos os diagnósticos preenchidos no formulário serão guardados.

1. Acesse https://sheets.google.com e faça login com sua conta Google.
2. Clique em "Em branco" para criar uma nova planilha.
3. No campo de nome no topo (onde aparece "Planilha sem título"), clique e digite o nome: `PROSPERA_TURISMO_DIAGNOSTICOS`.
4. Pressione Enter para salvar o nome.

**Como copiar o ID da planilha:**

Olhe para o endereço do seu navegador. Ele vai parecer com algo assim:

```
https://docs.google.com/spreadsheets/d/1aBcDeFgHiJkLmNoPqRsTuVwXyZ12345/edit
```

O ID da planilha é a sequência de letras e números que aparece entre `/d/` e `/edit`. No exemplo acima, o ID seria:

```
1aBcDeFgHiJkLmNoPqRsTuVwXyZ12345
```

5. Copie esse ID e guarde. Você vai precisar dele no Passo 4.

---

## 5. Passo 2 — Criar a pasta de relatórios no Google Drive

Os Google Docs e PDFs gerados pelo sistema serão salvos nessa pasta.

1. Acesse https://drive.google.com e faça login.
2. Clique em "Novo" (botão no canto superior esquerdo).
3. Selecione "Pasta".
4. Digite o nome: `RELATORIOS_PROSPERA_TURISMO`.
5. Clique em "Criar".
6. Clique duas vezes na pasta para abri-la.

**Como copiar o ID da pasta:**

Quando você abrir a pasta, o endereço do navegador vai parecer com algo assim:

```
https://drive.google.com/drive/folders/1XyZ9aBcDeFgHiJkLmNoPqRsTuVw
```

O ID da pasta é a sequência que aparece após `/folders/`. No exemplo acima, o ID seria:

```
1XyZ9aBcDeFgHiJkLmNoPqRsTuVw
```

7. Copie esse ID e guarde. Você vai precisar dele no Passo 4.

---

## 6. Passo 3 — Configurar o Google Apps Script

O Apps Script é onde ficam o código e os templates. Você vai acessá-lo pela planilha.

1. Abra a planilha `PROSPERA_TURISMO_DIAGNOSTICOS` que você criou.
2. No menu superior, clique em **Extensões**.
3. Clique em **Apps Script**.
4. Uma nova aba vai abrir com o editor do Apps Script.

**Apagar o código inicial:**

5. O editor já vai ter um código padrão chamado `function myFunction() {}`. Selecione tudo com Ctrl+A (ou Cmd+A no Mac) e apague.

**Colar o Code.gs:**

6. Abra o arquivo `Code.gs` no seu computador com qualquer editor de texto (Bloco de Notas, VS Code, TextEdit, etc.).
7. Selecione todo o conteúdo com Ctrl+A e copie com Ctrl+C.
8. Volte para o Apps Script e cole com Ctrl+V. O código vai aparecer no editor.
9. Clique no ícone de disquete (salvar) ou pressione Ctrl+S.

**Criar o arquivo do TemplateEmpresarial:**

10. No painel esquerdo do Apps Script, clique no sinal de "+" ao lado de "Arquivos".
11. Selecione "HTML".
12. No campo de nome, digite exatamente: `TemplateEmpresarial` (sem acentos, sem espaços, sem `.html` no final — o Apps Script adiciona `.html` automaticamente).
13. Pressione Enter.
14. O novo arquivo HTML vai abrir. Apague qualquer conteúdo inicial.
15. Abra o arquivo `TemplateEmpresarial.html` no seu computador, selecione tudo e copie.
16. Cole no editor do Apps Script.
17. Salve com Ctrl+S.

**Criar o arquivo do TemplateTurismo:**

18. Repita os passos 10 a 17, mas desta vez nomeie o arquivo como `TemplateTurismo` e cole o conteúdo do arquivo `TemplateTurismo.html`.

**Criar o arquivo do TemplateUnico:**

19. Repita os passos 10 a 17 novamente, nomeie o arquivo como `TemplateUnico` e cole o conteúdo do arquivo `TemplateUnico.html`.

Ao final, o painel esquerdo do Apps Script deve mostrar:
- `Code.gs`
- `TemplateEmpresarial.html`
- `TemplateTurismo.html`
- `TemplateUnico.html`

---

## 7. Passo 4 — Configurar as constantes no Code.gs

Agora você vai preencher as informações específicas do seu projeto dentro do `Code.gs`.

No Apps Script, clique no arquivo `Code.gs` para abri-lo. Procure as primeiras linhas do código. Elas vão parecer com isto:

```javascript
var SPREADSHEET_ID   = 'INSIRA_O_ID_DA_PLANILHA_AQUI';
var PASTA_RELATORIOS = 'INSIRA_O_ID_DA_PASTA_AQUI';
var LOGO_INSTITUTO   = 'https://raw.githubusercontent.com/SEU_USUARIO/SEU_REPOSITORIO/main/assets/logo-instituto-br.png';
var LOGO_PROSPERA    = 'https://raw.githubusercontent.com/SEU_USUARIO/SEU_REPOSITORIO/main/assets/logo-prospera-turismo.png';
var LOGO_PREFEITURA  = 'https://raw.githubusercontent.com/SEU_USUARIO/SEU_REPOSITORIO/main/assets/logo-prefeitura-rio.png';
```

**Substituir o ID da planilha:**

1. Apague o texto `INSIRA_O_ID_DA_PLANILHA_AQUI` (mantendo as aspas).
2. Cole o ID da planilha que você copiou no Passo 1.

Exemplo:
```javascript
var SPREADSHEET_ID = '1aBcDeFgHiJkLmNoPqRsTuVwXyZ12345';
```

**Substituir o ID da pasta de relatórios:**

3. Apague o texto `INSIRA_O_ID_DA_PASTA_AQUI` (mantendo as aspas).
4. Cole o ID da pasta que você copiou no Passo 2.

Exemplo:
```javascript
var PASTA_RELATORIOS = '1XyZ9aBcDeFgHiJkLmNoPqRsTuVw';
```

**Configurar as URLs das logomarcas:**

As logomarcas nos templates de PDF precisam de um endereço público completo na internet. A forma mais simples é usar a URL do GitHub Pages depois que você subir os arquivos.

A URL do GitHub Pages tem o formato:
```
https://SEU_USUARIO.github.io/prospera-turismo/assets/NOME_DO_ARQUIVO.png
```

Substitua `SEU_USUARIO` pelo seu nome de usuário do GitHub e `prospera-turismo` pelo nome do repositório que você vai criar.

Exemplo:
```javascript
var LOGO_INSTITUTO  = 'https://institutobr.github.io/prospera-turismo/assets/logo-instituto-br.png';
var LOGO_PROSPERA   = 'https://institutobr.github.io/prospera-turismo/assets/logo-prospera-turismo.png';
var LOGO_PREFEITURA = 'https://institutobr.github.io/prospera-turismo/assets/logo-prefeitura-rio.png';
```

**Atenção importante:** No arquivo `index.html` do GitHub, as logos usam caminho relativo como `assets/logo-prospera-turismo.png`. Isso funciona corretamente no formulário. Porém, nos templates do Apps Script que geram os PDFs visuais, o sistema roda nos servidores do Google e não consegue encontrar arquivos por caminho relativo. Por isso as URLs nas constantes do `Code.gs` devem ser endereços completos e públicos na internet. Se você tentar usar `assets/logo-prospera-turismo.png` dentro do `Code.gs`, a logomarca não vai aparecer nos PDFs visuais.

5. Salve o `Code.gs` com Ctrl+S.

---

## 8. Passo 5 — Inserir as funções dos PDFs visuais

O `Code.gs` já conta com as três funções de PDF Visual incorporadas. Você não precisa copiar e colar esses trechos manualmente.

As funções ja presentes no `Code.gs` sao:

- `gerarRelatorioEmpresarialTemplatePDFDaLinhaSelecionada`
- `gerarRelatorioTurismoTemplatePDFDaLinhaSelecionada`
- `gerarRelatorioUnicoTemplatePDFDaLinhaSelecionada`

O menu tambem ja inclui automaticamente os tres itens:

- "Diagnostico Empresarial - PDF Visual"
- "Diagnostico de Turismo - PDF Visual"
- "Diagnostico Unico - PDF Visual"

Voce nao precisa fazer nenhuma alteracao manual para isso. Basta colar o `Code.gs` conforme o Passo 3 e tudo estara pronto.

O passo a seguir descrito abaixo e apenas informativo, caso voce precise revisar ou recriar essas funcoes no futuro.

Texto de referência das funções (já incluídas no `Code.gs` entregue):

O `Code.gs` já contém funções para gerar Google Docs e PDFs via DocumentApp. As funções abaixo já foram incorporadas no arquivo final e servem como referência:

**Onde inserir as funções:**

No `Code.gs`, localize a função `gerarRelatorioUnicoPDFDaLinhaSelecionada()` (por volta da linha 646). Logo após o fechamento desta função (o `}` final dela), insira as três funções abaixo:

**Função 1 — PDF Visual do Diagnóstico Empresarial:**

```javascript
function gerarRelatorioEmpresarialTemplatePDFDaLinhaSelecionada() {
  var dados = montarDadosDaLinhaSelecionada();
  if (!dados) return;
  try {
    var template = HtmlService.createTemplateFromFile('TemplateEmpresarial');
    template.data = dados;
    template.logoInstituto  = LOGO_INSTITUTO;
    template.logoProspera   = LOGO_PROSPERA;
    template.logoPrefeitura = LOGO_PREFEITURA;
    var html = template.evaluate().getContent();
    var blob = Utilities.newBlob(html, 'text/html', 'temp_emp.html');
    var pdf  = blob.getAs('application/pdf')
                   .setName('DiagEmpresarial_' + (dados['SISTEMA_ID'] || 'sem_id') + '.pdf');
    var arquivo = (PASTA_RELATORIOS && PASTA_RELATORIOS !== 'INSIRA_O_ID_DA_PASTA_AQUI')
      ? DriveApp.getFolderById(PASTA_RELATORIOS).createFile(pdf)
      : DriveApp.createFile(pdf);
    registrarRelatorioGerado('Empresarial - PDF Visual', '', arquivo.getUrl(), dados['SISTEMA_ID']);
    mostrarLinkRelatorio(arquivo.getUrl(), 'Diagnostico Empresarial - PDF Visual');
  } catch (e) {
    SpreadsheetApp.getUi().alert('Erro ao gerar PDF via template: ' + e.toString());
    registrarLog('templatePDFEmpresarial', 'ERRO', e.toString());
  }
}
```

**Função 2 — PDF Visual do Diagnóstico de Turismo:**

```javascript
function gerarRelatorioTurismoTemplatePDFDaLinhaSelecionada() {
  var dados = montarDadosDaLinhaSelecionada();
  if (!dados) return;
  try {
    var template = HtmlService.createTemplateFromFile('TemplateTurismo');
    template.data = dados;
    template.logoInstituto  = LOGO_INSTITUTO;
    template.logoProspera   = LOGO_PROSPERA;
    template.logoPrefeitura = LOGO_PREFEITURA;
    var html = template.evaluate().getContent();
    var blob = Utilities.newBlob(html, 'text/html', 'temp_tur.html');
    var pdf  = blob.getAs('application/pdf')
                   .setName('DiagTurismo_' + (dados['SISTEMA_ID'] || 'sem_id') + '.pdf');
    var arquivo = (PASTA_RELATORIOS && PASTA_RELATORIOS !== 'INSIRA_O_ID_DA_PASTA_AQUI')
      ? DriveApp.getFolderById(PASTA_RELATORIOS).createFile(pdf)
      : DriveApp.createFile(pdf);
    registrarRelatorioGerado('Turismo - PDF Visual', '', arquivo.getUrl(), dados['SISTEMA_ID']);
    mostrarLinkRelatorio(arquivo.getUrl(), 'Diagnostico de Turismo - PDF Visual');
  } catch (e) {
    SpreadsheetApp.getUi().alert('Erro ao gerar PDF via template: ' + e.toString());
    registrarLog('templatePDFTurismo', 'ERRO', e.toString());
  }
}
```

**Função 3 — PDF Visual do Diagnóstico Único:**

```javascript
function gerarRelatorioUnicoTemplatePDFDaLinhaSelecionada() {
  var dados = montarDadosDaLinhaSelecionada();
  if (!dados) return;
  try {
    var template = HtmlService.createTemplateFromFile('TemplateUnico');
    template.data = dados;
    template.logoInstituto  = LOGO_INSTITUTO;
    template.logoProspera   = LOGO_PROSPERA;
    template.logoPrefeitura = LOGO_PREFEITURA;
    var html = template.evaluate().getContent();
    var blob = Utilities.newBlob(html, 'text/html', 'temp_unico.html');
    var pdf  = blob.getAs('application/pdf')
                   .setName('DiagUnico_' + (dados['SISTEMA_ID'] || 'sem_id') + '.pdf');
    var arquivo = (PASTA_RELATORIOS && PASTA_RELATORIOS !== 'INSIRA_O_ID_DA_PASTA_AQUI')
      ? DriveApp.getFolderById(PASTA_RELATORIOS).createFile(pdf)
      : DriveApp.createFile(pdf);
    registrarRelatorioGerado('Unico - PDF Visual', '', arquivo.getUrl(), dados['SISTEMA_ID']);
    mostrarLinkRelatorio(arquivo.getUrl(), 'Diagnostico Unico - PDF Visual');
  } catch (e) {
    SpreadsheetApp.getUi().alert('Erro ao gerar PDF via template: ' + e.toString());
    registrarLog('templatePDFUnico', 'ERRO', e.toString());
  }
}
```

**Adicionar os itens de menu:**

Ainda no `Code.gs`, localize a função `criarMenu()`. Dentro dela, após a linha:

```javascript
.addItem('Diagnostico Unico Consolidado - PDF', 'gerarRelatorioUnicoPDFDaLinhaSelecionada')
```

Adicione os três novos itens:

```javascript
.addSeparator()
.addItem('Diagnostico Empresarial - PDF Visual', 'gerarRelatorioEmpresarialTemplatePDFDaLinhaSelecionada')
.addItem('Diagnostico de Turismo - PDF Visual', 'gerarRelatorioTurismoTemplatePDFDaLinhaSelecionada')
.addItem('Diagnostico Unico - PDF Visual', 'gerarRelatorioUnicoTemplatePDFDaLinhaSelecionada')
```

Salve com Ctrl+S.

---

## 9. Passo 6 — Executar a instalação do sistema

Este passo cria todas as abas, cabeçalhos e estruturas necessárias na planilha automaticamente.

1. No Apps Script, clique no menu suspenso de funções (geralmente aparece "myFunction" ou o nome da última função usada).
2. Clique nesse menu e selecione `instalarSistemaProsperaRioTurismo`.
3. Clique no botão **Executar** (o triângulo de "play").
4. Uma janela de autorização vai aparecer. Clique em **Revisar permissões**.
5. Escolha a sua conta Google.
6. O Google vai exibir um aviso dizendo que o aplicativo não foi verificado. Isso é normal para scripts pessoais. Clique em **Avançado**.
7. Clique em **Acessar Prospera Turismo (não seguro)** ou texto similar.
8. Clique em **Permitir**.
9. O script vai executar. Aguarde alguns segundos.
10. Volte para a planilha (clique na aba do navegador com a planilha).
11. Atualize a página com F5 ou Ctrl+R.
12. Verifique que o menu **Prospera Turismo** apareceu na barra de menus da planilha, ao lado de "Ajuda".
13. Verifique que as abas foram criadas na parte inferior da planilha:
    - `00_CONFIG`
    - `01_RESPOSTAS_DIAGNOSTICO_UNICO`
    - `02_TERRITORIOS`
    - `03_LISTAS_VALIDACAO`
    - `04_DICIONARIO_COLUNAS`
    - `05_LOGS`
    - `06_RELATORIOS_GERADOS`
    - `07_PAINEL_RESUMO`
    - `08_PLANO_ACAO`

---

## 10. Passo 7 — Publicar como Web App

O Web App é o endereço do Google que vai receber os dados do formulário. Sem isso, o formulário não consegue enviar nada.

1. No Apps Script, clique em **Implantar** (canto superior direito).
2. Selecione **Nova implantação**.
3. Clique no ícone de engrenagem ao lado de "Selecionar tipo".
4. Selecione **App da Web**.
5. No campo **Descrição**, escreva algo como: `Prospera Turismo v1`.
6. Em **Executar como**, selecione: **Eu mesmo (seu e-mail)**.
7. Em **Quem pode acessar**, selecione: **Qualquer pessoa**.

    Atenção: este campo deve ser "Qualquer pessoa" para que o formulário do GitHub Pages consiga enviar dados sem precisar de login.

8. Clique em **Implantar**.
9. Se aparecer pedido de autorização novamente, repita os passos de autorização do Passo 6.
10. Após a implantação, uma janela vai mostrar a URL do Web App. Ela tem o formato:

```
https://script.google.com/macros/s/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/exec
```

11. Copie essa URL completa e guarde. Você vai precisar dela no próximo passo.

---

## 11. Passo 8 — Configurar o index.html com a URL do Web App

1. Abra o arquivo `index.html` no seu computador com qualquer editor de texto.
2. Procure a seguinte linha (ela fica perto do final do arquivo, dentro da tag `<script>`):

```javascript
var WEB_APP_URL = 'COLE_AQUI_A_URL_DO_WEB_APP';
```

3. Apague o texto `COLE_AQUI_A_URL_DO_WEB_APP` (mantendo as aspas simples).
4. Cole a URL do Web App que você copiou no Passo 7.

Exemplo de como deve ficar:
```javascript
var WEB_APP_URL = 'https://script.google.com/macros/s/AKfycbxXXXXXXXXX/exec';
```

5. Salve o arquivo.

---

## 12. Passo 9 — Subir os arquivos no GitHub e ativar o GitHub Pages

**Criar o repositório:**

1. Acesse https://github.com e faça login.
2. Clique no botão "+" no canto superior direito e selecione "New repository".
3. Em "Repository name", digite: `prospera-turismo`.
4. Selecione "Public" (público).
5. Não marque nenhuma outra opção.
6. Clique em **Create repository**.

**Enviar os arquivos:**

7. Na página do repositório criado, clique em **uploading an existing file** ou **Add file > Upload files**.
8. Arraste ou selecione o arquivo `index.html`.
9. Clique em **Commit changes**.
10. Para criar a pasta `assets`, clique em **Add file > Create new file**.
11. No campo do nome, digite: `assets/logo-instituto-br.png` — isso cria automaticamente a pasta `assets`.
12. Porém, como o GitHub não aceita imagens por esse caminho, use a opção **Add file > Upload files** e, antes de soltar os arquivos, clique na pasta `assets` se ela aparecer, ou suba as imagens e depois mova-as.

    A forma mais simples: clique em **Add file > Upload files**, depois clique na área de upload e selecione as três imagens de uma vez. Após o upload, você vai poder mover para a pasta `assets` ou fazer o upload diretamente dentro dela clicando na pasta primeiro.

    Se preferir, use o GitHub Desktop ou o Git por linha de comando para organizar os arquivos localmente e enviar tudo de uma vez.

13. Confirme que o repositório contém:
    - `index.html` (na raiz)
    - `assets/logo-instituto-br.png`
    - `assets/logo-prospera-turismo.png`
    - `assets/logo-prefeitura-rio.png`

**Ativar o GitHub Pages:**

14. No repositório, clique em **Settings** (Configurações).
15. No menu lateral esquerdo, clique em **Pages**.
16. Em "Branch", selecione a branch principal (geralmente `main` ou `master`).
17. Em "Folder", selecione `/ (root)`.
18. Clique em **Save**.
19. Aguarde de 1 a 3 minutos.
20. Recarregue a página de Settings > Pages.
21. Vai aparecer uma mensagem com a URL pública do formulário, no formato:
    `https://SEU_USUARIO.github.io/prospera-turismo/`
22. Clique nessa URL para abrir o formulário no navegador.

**Atenção:** Após obter a URL do GitHub Pages, volte ao `Code.gs` no Apps Script e atualize as constantes `LOGO_INSTITUTO`, `LOGO_PROSPERA` e `LOGO_PREFEITURA` com os endereços completos das imagens, conforme explicado no Passo 4.

---

## 13. Passo 10 — Teste obrigatório do formulário

Antes de usar em campo, teste o sistema completo com um diagnóstico fictício.

1. Abra o formulário pela URL do GitHub Pages.
2. Confira se as três logomarcas aparecem no cabeçalho.
3. Preencha a **Seção 1** (Dados da coleta): data, tipo, território, nome do entrevistador, horário de início.
4. Preencha a **Seção 2** (Perfil do empreendedor): nome fictício, gênero, **data de nascimento**.
5. Verifique se o campo **Idade** foi preenchido automaticamente após digitar a data de nascimento.
6. Continue preenchendo as demais seções. Use dados fictícios para o teste.
7. Na **Seção 5** (Caracterização do negócio), preencha o campo de **CPF ou CNPJ**.
8. Na **Seção 4** (Contato e endereço), preencha o **e-mail**.
9. Clique em **Enviar Diagnóstico**.
10. A mensagem de confirmação "Formulário enviado com sucesso" deve aparecer no topo da página.

**Conferir a planilha:**

11. Abra a planilha `PROSPERA_TURISMO_DIAGNOSTICOS` no Google Sheets.
12. Clique na aba `01_RESPOSTAS_DIAGNOSTICO_UNICO`.
13. Verifique se a linha com os dados do teste apareceu.
14. Procure as colunas `NEGOCIO_CPF` ou `NEGOCIO_CNPJ` e confirme que o valor está preenchido.
15. Procure a coluna `CONTATO_EMAIL` e confirme que o e-mail aparece.
16. Procure a coluna `PESSOA_DATA_NASCIMENTO` e confirme que a data de nascimento está correta.
17. Procure a coluna `PESSOA_IDADE` e confirme que a idade calculada está correta.

---

## 14. Passo 11 — Gerar relatórios pela planilha

1. Na planilha, abra a aba `01_RESPOSTAS_DIAGNOSTICO_UNICO`.
2. Clique em qualquer célula da linha do diagnóstico que você quer transformar em relatório. Pode ser qualquer célula dessa linha, não precisa ser a primeira.
3. No menu superior, clique em **Prospera Turismo**.
4. Escolha o tipo de relatório desejado:

**Relatórios via Google Docs (editável + PDF):**
- "Diagnostico Empresarial - Google Docs" — cria um documento editável no Google Drive.
- "Diagnostico Empresarial - PDF" — converte o documento em PDF e salva no Drive.
- "Diagnostico de Turismo - Google Docs"
- "Diagnostico de Turismo - PDF"
- "Diagnostico Unico Consolidado - Google Docs"
- "Diagnostico Unico Consolidado - PDF"

**Relatórios visuais (PDF com layout institucional):**
- "Diagnostico Empresarial - PDF Visual"
- "Diagnostico de Turismo - PDF Visual"
- "Diagnostico Unico - PDF Visual"

5. Após a geração, uma janela vai aparecer com o link para abrir o arquivo. Clique em "Abrir arquivo".
6. Verifique se o relatório contém os dados corretos.
7. Abra a aba `06_RELATORIOS_GERADOS` na planilha para confirmar que o registro do relatório foi salvo com o link.
8. Acesse a pasta `RELATORIOS_PROSPERA_TURISMO` no Google Drive para verificar os arquivos gerados.

---

## 15. Erros comuns e como resolver

| Problema | Causa provável | Como resolver |
|---|---|---|
| Formulário envia, mas a resposta não aparece na planilha | O Web App não foi publicado ou a URL está errada no `index.html` | Confirme que o Web App foi publicado como "qualquer pessoa pode acessar" e que a URL correta está no `WEB_APP_URL` do `index.html` |
| Formulário envia sem erro, mas planilha vazia | A planilha está em modo de visualização ou o `SPREADSHEET_ID` está errado | Verifique se o ID da planilha no `Code.gs` é exatamente o mesmo que aparece na URL da planilha |
| Web App retorna erro 404 ou página em branco | O Web App não foi publicado ou foi despublicado | Vá em Implantar > Gerenciar implantações e confirme se o Web App está ativo |
| URL do Web App não foi colada no `index.html` | O texto `COLE_AQUI_A_URL_DO_WEB_APP` ainda está no código | Abra o `index.html`, localize `WEB_APP_URL` e substitua pelo endereço real do Web App |
| `SPREADSHEET_ID` errado | ID copiado incorretamente | Abra a planilha, copie a sequência completa entre `/d/` e `/edit` na URL e cole no `Code.gs` |
| `PASTA_RELATORIOS` errado | ID da pasta copiado incorretamente ou pasta não existe | Abra a pasta no Google Drive, copie a sequência após `/folders/` na URL |
| Menu "Prospera Turismo" não aparece | A função de instalação não foi executada ou a planilha não foi recarregada | Execute a função `instalarSistemaProsperaRioTurismo` no Apps Script e recarregue a planilha com F5 |
| Função não autorizada no Apps Script | As permissões não foram aceitas | Execute qualquer função no Apps Script e complete o fluxo de autorização do Google |
| Campos novos não aparecem na planilha | A instalação foi feita antes de adicionar os campos | Execute `criarOuAtualizarEstrutura()` no Apps Script para adicionar os novos cabeçalhos sem apagar dados |
| `PESSOA_DATA_NASCIMENTO` não aparece na planilha | Campo novo adicionado após a primeira instalação | Execute `criarOuAtualizarEstrutura()` no Apps Script. O sistema vai adicionar a coluna sem apagar registros |
| Logomarca aparece no formulário (GitHub), mas não aparece no PDF visual | A URL da logo no `Code.gs` ainda usa caminho relativo (`assets/...`) | Substitua pelo endereço completo do GitHub Pages: `https://SEU_USUARIO.github.io/prospera-turismo/assets/logo-...png` |
| PDF visual não gera — erro ao gerar PDF via template | Template não encontrado ou URL da logo inválida | Confirme que os arquivos `TemplateEmpresarial.html`, `TemplateTurismo.html` e `TemplateUnico.html` estão criados no Apps Script com esses nomes exatos |
| Template não encontrado | Nome do arquivo HTML no Apps Script está errado | Confirme que os arquivos foram criados sem espaços e sem acentos: `TemplateEmpresarial`, `TemplateTurismo`, `TemplateUnico` |
| Nome do arquivo HTML no Apps Script está errado | Arquivo criado com nome diferente do esperado pelo código | Delete o arquivo incorreto e crie novamente com o nome exato indicado no Passo 3 |
| Relatório gera, mas campos vêm vazios | A linha selecionada na planilha não é a linha de dados | Certifique-se de clicar em uma célula da linha com dados antes de acionar o menu. O sistema usa a linha onde está o cursor |
| A linha correta não foi selecionada | Cursor estava na linha 1 (cabeçalho) | Clique em qualquer célula de uma linha com número 2 ou superior (abaixo do cabeçalho) |
| GitHub Pages abre página 404 | GitHub Pages não foi ativado ou o `index.html` não está na raiz | Confirme que o arquivo se chama exatamente `index.html` (minúsculo) e está na pasta raiz do repositório, não dentro de subpastas |
| Assets (logomarcas) não carregam no GitHub | Arquivos com nomes diferentes ou pasta com nome errado | Confirme que os arquivos estão na pasta `assets` (minúsculo) e com os nomes exatos: `logo-instituto-br.png`, `logo-prospera-turismo.png`, `logo-prefeitura-rio.png` |

---

## 16. Checklist final de instalação

Use este checklist para garantir que tudo foi feito corretamente:

- [ ] Planilha `PROSPERA_TURISMO_DIAGNOSTICOS` criada no Google Sheets
- [ ] ID da planilha copiado e colado no `SPREADSHEET_ID` do `Code.gs`
- [ ] Pasta `RELATORIOS_PROSPERA_TURISMO` criada no Google Drive
- [ ] ID da pasta copiado e colado no `PASTA_RELATORIOS` do `Code.gs`
- [ ] `Code.gs` colado no Apps Script
- [ ] Arquivo `TemplateEmpresarial` criado no Apps Script com o conteúdo correto
- [ ] Arquivo `TemplateTurismo` criado no Apps Script com o conteúdo correto
- [ ] Arquivo `TemplateUnico` criado no Apps Script com o conteúdo correto
- [ ] Três funções de PDF Visual adicionadas ao `Code.gs`
- [ ] Três itens de menu de PDF Visual adicionados à função `criarMenu()`
- [ ] Função `instalarSistemaProsperaRioTurismo` executada com sucesso
- [ ] Permissões do Google autorizadas
- [ ] Menu "Prospera Turismo" aparece na planilha
- [ ] Abas criadas na planilha (de `00_CONFIG` a `08_PLANO_ACAO`)
- [ ] Web App publicado com acesso "qualquer pessoa"
- [ ] URL do Web App copiada
- [ ] URL do Web App colada no `WEB_APP_URL` do `index.html`
- [ ] GitHub Pages publicado e URL gerada
- [ ] URLs completas das logos configuradas no `Code.gs` (apontando para o GitHub Pages)
- [ ] Formulário testado com diagnóstico fictício
- [ ] Resposta do teste apareceu na aba `01_RESPOSTAS_DIAGNOSTICO_UNICO`
- [ ] CPF ou CNPJ aparece corretamente na planilha
- [ ] E-mail aparece corretamente na planilha
- [ ] Data de nascimento aparece corretamente na planilha
- [ ] Idade calculada automaticamente no formulário
- [ ] Diagnóstico Empresarial — Google Docs gerado com sucesso
- [ ] Diagnóstico Empresarial — PDF gerado com sucesso
- [ ] Diagnóstico Empresarial — PDF Visual gerado com sucesso
- [ ] Diagnóstico de Turismo — Google Docs gerado com sucesso
- [ ] Diagnóstico de Turismo — PDF gerado com sucesso
- [ ] Diagnóstico de Turismo — PDF Visual gerado com sucesso
- [ ] Diagnóstico Único Consolidado — Google Docs gerado com sucesso
- [ ] Diagnóstico Único Consolidado — PDF gerado com sucesso
- [ ] Diagnóstico Único — PDF Visual gerado com sucesso
- [ ] Logos aparecem corretamente no formulário (GitHub Pages)
- [ ] Logos aparecem corretamente nos PDFs visuais

---

## 17. Observação importante sobre o GitHub

O GitHub Pages exibe o formulário para o entrevistador preencher. Mas o GitHub não armazena nenhum dado. Ele não tem banco de dados, não grava respostas e não gera documentos.

Tudo que o GitHub faz neste sistema é mostrar o arquivo `index.html` no navegador.

Quem realmente trabalha com os dados é o Google Apps Script. Quando o entrevistador clica em "Enviar Diagnóstico", o formulário do GitHub manda os dados para o endereço do Web App no Google. O Apps Script recebe, organiza e grava tudo no Google Sheets.

Por isso, se você mudar o `index.html` mas não configurar o Apps Script corretamente, o formulário vai aparecer normalmente no GitHub, mas nada vai ser gravado.

E se o Apps Script estiver configurado corretamente mas o GitHub Pages não estiver no ar, o entrevistador não vai conseguir acessar o formulário.

As duas partes precisam funcionar juntas para o sistema operar.


---

## 18. Observação sobre logomarcas — pré-visualização e publicação

As logomarcas podem não aparecer durante testes locais, em pré-visualizações de editores de texto ou antes de o repositório estar publicado corretamente no GitHub Pages. Isso não significa que há um erro definitivo no sistema. Existem três situações diferentes que precisam ser compreendidas:

**Situação 1 — Formulário no GitHub Pages:**

Quando o `index.html` está publicado no GitHub Pages, as logos são carregadas com caminho relativo:

```
assets/logo-instituto-br.png
assets/logo-prospera-turismo.png
assets/logo-prefeitura-rio.png
```

Para que as logos apareçam corretamente no formulário, os arquivos devem estar:
- na pasta `assets/` do repositório (não na raiz);
- com os nomes exatamente iguais aos indicados acima (letras minúsculas, hífens, extensão `.png`);
- no mesmo repositório que o `index.html`.

Se uma das condições não for atendida, a logo não aparece no formulário mesmo que o GitHub Pages esteja ativo. Para confirmar, abra o formulário pelo GitHub Pages e inspecione se as imagens estão carregando (no navegador, clique com o botão direito > Inspecionar > aba Network e recarregue a página).

**Situação 2 — PDFs visuais gerados pelo Apps Script:**

Os templates `TemplateEmpresarial.html`, `TemplateTurismo.html` e `TemplateUnico.html` são executados nos servidores do Google, não no seu computador. Por isso, os caminhos relativos como `assets/logo-instituto-br.png` não funcionam dentro do Apps Script.

Para que as logos apareçam nos PDFs visuais, as constantes `LOGO_INSTITUTO`, `LOGO_PROSPERA` e `LOGO_PREFEITURA` no `Code.gs` devem apontar para endereços públicos e completos na internet.

A opção mais simples é usar as URLs do GitHub Pages:

```javascript
var LOGO_INSTITUTO  = 'https://SEU_USUARIO.github.io/prospera-turismo/assets/logo-instituto-br.png';
var LOGO_PROSPERA   = 'https://SEU_USUARIO.github.io/prospera-turismo/assets/logo-prospera-turismo.png';
var LOGO_PREFEITURA = 'https://SEU_USUARIO.github.io/prospera-turismo/assets/logo-prefeitura-rio.png';
```

Substitua `SEU_USUARIO` pelo seu nome de usuário real do GitHub e `prospera-turismo` pelo nome exato do repositório.

Caso o GitHub Pages ainda não esteja publicado no momento em que você for testar os PDFs visuais, as logos vão aparecer como espaços em branco nos PDFs. Isso é esperado. Assim que o GitHub Pages estiver ativo e as URLs forem configuradas, os PDFs passarão a exibir as logos corretamente.

**Situação 3 — Pré-visualização no navegador ou em editores:**

Se você abrir o `index.html` diretamente pelo seu computador (clicando duas vezes no arquivo), o caminho `assets/logo-instituto-br.png` pode funcionar se a pasta `assets/` estiver na mesma pasta que o `index.html`. Mas isso é apenas uma simulação local. O comportamento definitivo só pode ser avaliado com os arquivos publicados no GitHub Pages.

**Resumo prático:**

| Onde as logos devem aparecer | O que precisa estar correto |
|---|---|
| No formulário (GitHub Pages) | Arquivos na pasta `assets/` do repositório com nomes exatos |
| Nos PDFs visuais (Apps Script) | URLs completas e públicas nas constantes do `Code.gs` |
| Em teste local no computador | Pasta `assets/` junto com o `index.html` na mesma pasta |

---

## 19. Estrutura final esperada do repositório no GitHub

Ao final da instalação, o repositório no GitHub deve ter exatamente esta estrutura:

```
prospera-turismo/
├── index.html
└── assets/
    ├── logo-instituto-br.png
    ├── logo-prospera-turismo.png
    └── logo-prefeitura-rio.png
```

O repositório não deve conter `Code.gs`, `TemplateEmpresarial.html`, `TemplateTurismo.html` ou `TemplateUnico.html`. Esses arquivos ficam exclusivamente no Google Apps Script.

---

## 20. Estrutura final esperada do Google Apps Script

No editor do Apps Script, o painel de arquivos deve mostrar exatamente:

```
Code.gs
TemplateEmpresarial.html
TemplateTurismo.html
TemplateUnico.html
```

Se algum desses arquivos estiver ausente ou com nome diferente, os relatórios correspondentes não vão funcionar.

---

## 21. Referência rápida das abas criadas na planilha

Após executar `instalarSistemaProsperaRioTurismo()`, a planilha vai ter as seguintes abas:

| Aba | O que contém |
|---|---|
| `00_CONFIG` | Configurações do sistema: IDs, URLs, versão |
| `01_RESPOSTAS_DIAGNOSTICO_UNICO` | Todos os diagnósticos recebidos pelo formulário |
| `02_TERRITORIOS` | Lista dos territórios cadastrados |
| `03_LISTAS_VALIDACAO` | Listas de opções usadas pelo sistema |
| `04_DICIONARIO_COLUNAS` | Descrição técnica de cada coluna da aba principal |
| `05_LOGS` | Registro de ações e erros do sistema |
| `06_RELATORIOS_GERADOS` | Links de todos os relatórios gerados com data e tipo |
| `07_PAINEL_RESUMO` | Painel com totais e contagens por território, formalização e maturidade |
| `08_PLANO_ACAO` | Aba reservada para registros de planos de ação |

A aba `01_RESPOSTAS_DIAGNOSTICO_UNICO` nunca deve ser limpa manualmente. Ela é a base de dados do sistema. Executar `criarOuAtualizarEstrutura()` pelo menu jamais apaga os dados desta aba — apenas adiciona cabeçalhos faltantes.

---

## 22. Referência rápida dos itens do menu na planilha

Após a instalação, o menu **Prospera Turismo** vai ter os seguintes itens:

| Item do menu | Função chamada | O que faz |
|---|---|---|
| Gerar todos os relatórios da linha selecionada | `gerarTodosRelatoriosDaLinhaSelecionada` | Gera Docs e PDFs dos três diagnósticos de uma vez |
| Diagnóstico Empresarial - Google Docs | `gerarRelatorioEmpresarialDocsDaLinhaSelecionada` | Cria Google Doc editável do diagnóstico empresarial |
| Diagnóstico Empresarial - PDF | `gerarRelatorioEmpresarialPDFDaLinhaSelecionada` | Gera PDF a partir do Google Doc empresarial |
| Diagnóstico de Turismo - Google Docs | `gerarRelatorioTurismoDocsDaLinhaSelecionada` | Cria Google Doc editável do diagnóstico de turismo |
| Diagnóstico de Turismo - PDF | `gerarRelatorioTurismoPDFDaLinhaSelecionada` | Gera PDF a partir do Google Doc de turismo |
| Diagnóstico Único Consolidado - Google Docs | `gerarRelatorioUnicoDocsDaLinhaSelecionada` | Cria Google Doc editável do diagnóstico consolidado |
| Diagnóstico Único Consolidado - PDF | `gerarRelatorioUnicoPDFDaLinhaSelecionada` | Gera PDF a partir do Google Doc consolidado |
| Diagnóstico Empresarial - PDF Visual | `gerarRelatorioEmpresarialTemplatePDFDaLinhaSelecionada` | Gera PDF visual com layout institucional usando template HTML |
| Diagnóstico de Turismo - PDF Visual | `gerarRelatorioTurismoTemplatePDFDaLinhaSelecionada` | Gera PDF visual do turismo com layout institucional |
| Diagnóstico Único - PDF Visual | `gerarRelatorioUnicoTemplatePDFDaLinhaSelecionada` | Gera PDF visual consolidado com layout institucional |
| Atualizar estrutura da planilha | `criarOuAtualizarEstrutura` | Atualiza abas e cabeçalhos sem apagar dados |
| Atualizar painel resumo | `atualizarPainelResumo` | Recalcula os totais do painel na aba `07_PAINEL_RESUMO` |
| Formatar planilha | `formatarAbas` | Aplica formatação visual nas linhas de dados |
| Instalar sistema | `instalarSistemaProsperaRioTurismo` | Executa a instalação completa do sistema |

---

**Sistema Prospera Turismo — Instituto BR Arte**
Diagnóstico Único: Diagnóstico Empresarial e Diagnóstico de Turismo Comunitário
