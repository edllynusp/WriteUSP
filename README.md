# **Ferramentas para Escrita Acadêmica**

Este repositório contém informações sobre ferramentas úteis para a escrita de documentos científicos, incluindo revisão ortográfica, versionamento, diferenciação de versões de documentos LaTeX, gráficos. Foi inspirado após o minicurso "mão na massa" realizado pelo Prof. Dr. Daniel Macedo Batista no IME-USP.

## **1. Templates para Tese, Projeto de Pesquisa FAPESP e Outros**
- [Modelo de Tese/Dissertação (USP)](https://gitlab.com/ccsl-usp/modelo-latex)
- [Modelo de Projeto de Pesquisa FAPESP](https://pt.overleaf.com/latex/templates/projeto-fapesp-iniciacao-cientifica/wrjcqxbypyzk)
- [Manuscript Templates for Conference Proceedings - IEEE](https://pt.overleaf.com/latex/templates/projeto-fapesp-iniciacao-cientifica/wrjcqxbypyzk)

## **2. Pré-requisitos: ambiente recomendado e pacotes**
### **Linux (Ubuntu/Debian)**
```sh
sudo apt update && sudo apt install -y \
  latexdiff make diffpdf aspell aspell-en aspell-pt-br gnuplot \
  texlive-latex-base texlive-latex-extra texlive-latex-recommended \
  ghostscript git vim
```
## **3. Edição e Formatação**
- **vim** – Editor de texto poderoso para escrever e editar arquivos LaTeX.
- **Visual Studio Code** – Utilizar a IDE com suporte das extensões:
  - [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)
  - [LaTeX](https://marketplace.visualstudio.com/items?itemName=mathematic.vscode-latex)
  - [latex-formatter](https://marketplace.visualstudio.com/items?itemName=nickfode.latex-formatter)
  - [LaTeX Utilities](https://marketplace.visualstudio.com/items?itemName=tecosaur.latex-utilities).
- **[Overleaf](https://www.overleaf.com/)** – Plataforma colaborativa para edição de documentos LaTeX online.
- **[Turnitin](https://www.turnitin.com/)** – Verificação de similaridade para detecção de plágio.

## **4. Geração e Compilação de Documentos LaTeX**
- **make** – Automatiza a compilação de projetos LaTeX.
- **texlive-latex-base** – Pacote essencial para compilar documentos LaTeX.
- **texlive-latex-extra** – Conjunto adicional de pacotes LaTeX úteis.
- **texlive-latex-recommended** – Pacotes recomendados para documentos científicos e técnicos.
- **ghostscript** – Manipulação e conversão de arquivos PDF e PostScript.

## **5. Gráficos e Visualização de Dados**
- **gnuplot** – Ferramenta para geração de gráficos científicos.
  - Exemplos disponíveis em: [Gnuplot Demos](http://gnuplot.info/demo/)

## **6. Revisão Ortográfica e Gramatical**
- **aspell** – Verificador ortográfico para o terminal.
  - **aspell-en** – Dicionário para inglês.
  - **aspell-pt-br** – Dicionário para português do Brasil.
- **[Grammarly](https://app.grammarly.com/)** – Ferramenta online para correção gramatical e sugestões de estilo em inglês.
- **[Maritaca AI](https://chat.maritaca.ai/)** – IA especializada na revisão de textos acadêmicos em português.
- **[DeepL](https://www.deepl.com)** – Tradutor avançado, útil para textos técnicos e acadêmicos.
- **[QuillBot](https://quillbot.com/pt/corretor-ortografico)** – Ferramenta de parafraseamento alimentada por AI que melhora sua escrita.


## **7. Comparação e Controle de Versão para Documentos LaTeX**
- **latexdiff** – Gera um documento LaTeX destacando as diferenças entre duas versões.
- **latexdiff-vc** – Integra `latexdiff` com sistemas de controle de versão como `git`, facilitando a comparação de versões armazenadas no Overleaf + Git.
- **git** – Controle de versão para rastrear alterações no código-fonte do documento.
- **diffpdf** – Comparação visual de arquivos PDF.
