

# **Ferramentas para apoio  à  escrita acadêmica**
![Imagem](https://static1.makeuseofimages.com/wordpress/wp-content/uploads/2023/05/writing-on-windows.jpg?q=50&fit=crop&w=1140&h=&dpr=1.5)

Este repositório contém informações sobre ferramentas úteis para apoia à escrita de documentos científicos, incluindo revisão ortográfica, versionamento, diferenciação de versões de documentos LaTeX, gráficos e entre outros. Foi inspirado após o minicurso "mão na massa" ministrado pelo Prof. Dr. Daniel Macedo Batista no IME-USP.

## **1. Templates para tese, Projeto de pesquisa FAPESP e outros**
- [Modelo de Tese/Dissertação (USP)](https://gitlab.com/ccsl-usp/modelo-latex)
- [Modelo de Projeto de Pesquisa FAPESP](https://pt.overleaf.com/latex/templates/projeto-fapesp-iniciacao-cientifica/wrjcqxbypyzk)
- [Manuscript Templates for Conference Proceedings - IEEE](https://pt.overleaf.com/latex/templates/projeto-fapesp-iniciacao-cientifica/wrjcqxbypyzk)

## **2. Pré-requisitos: ambiente recomendado e pacotes**
Recomendado utilizar o sistema operacional **Linux (Ubuntu/Debian)**. 
Através do único comando abaixo é realizado a atualização do SO e instalação dos pacotes necessários:
```sh
sudo apt update && sudo apt install -y \
  latexdiff make diffpdf aspell aspell-en aspell-pt-br gnuplot \
  texlive-latex-base texlive-latex-extra texlive-latex-recommended \
  ghostscript git vim
```
## **3. Escrita**
A escrita de documentos científicos podem ser realizadas em diferentes ambientes, cada um oferecendo vantagens específicas. 
- **[Vim](https://www.vim.org/download.php)** – Editor de texto tradicional e poderoso, amplamente utilizado no meio acadêmico.
- **[Visual Studio Code](https://code.visualstudio.com/)** – A IDE proporciona um ambiente moderno e interativo para edição de documentos LaTeX, com suporte de extensões que auxiliam na formatação, como:
  - [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)
  - [LaTeX](https://marketplace.visualstudio.com/items?itemName=mathematic.vscode-latex)
  - [latex-formatter](https://marketplace.visualstudio.com/items?itemName=nickfode.latex-formatter)
  - [LaTeX Utilities](https://marketplace.visualstudio.com/items?itemName=tecosaur.latex-utilities).
- **[Overleaf](https://www.overleaf.com/)** – Uma excelente plataforma online para edição colaborativa de documentos LaTeX, de fácil uso e sem necessidade de configuração local. Além disso, é possível integrar o Overleaf com editores via Git, permitindo um fluxo de trabalho híbrido entre edição online e local.



## **4. Gráficos e visualização de dados**
Por meio do **[gnuplot](http://www.gnuplot.info/)** é possivel criar gráficos em duas ou três dimensões através de simples linhas de comando. 
Exemplos de gŕaficos disponíveis em: [Gnuplot Demos](http://gnuplot.info/demo/). 

O comando abaixo chama o programa gnuplot:
```sh
gnuplot configuracao.gpi 
```
O arquivo `configuracao.gpi` define como o gráfico deve ser gerado (por exemplo, tipo de gráfico, eixos, rótulos, cores, saída gráfica etc.). 

Exemplo de um arquivo `configuracao.gpi`:

```sh
set ydata time
set timefmt "%M:%S"
set format y "%M:%S"

set xlabel 'Dados transferidos por cliente (MB)'
set ylabel 'Tempo para atender 100 clientes concorrentes'

set term epscairo
set output "resultados-tudo.eps"

set grid
set key top left

plot "resultados-tudo.data" using 1:4 with linespoints title "Sockets da Internet: Mux de E/S",\
     "resultados-tudo.data" using 1:3 with linespoints title "Sockets da Internet: Threads",\
     "resultados-tudo.data" using 1:2 with linespoints title "Sockets da Internet: Processos",\
     "resultados-tudo.data" using 1:5 with linespoints title "Sockets Unix: Threads"
```



## **5. Geração e compilação de documentos LaTeX**
Durante a edição local, é necessário produzir alguns arquivos para compilar, gerar bibliografia e imprimir o arquivo final com uma extensão desejada.

Para gerar um arquivo `.dvi` a partir do seu arquivo `.tex`, utilize o seguinte comando:  

```sh
latex artigo.tex
```
Esse comando compila o seu arquivo LaTeX e cria um arquivo `.dvi`, que é um formato de saída intermediário.

Para gerar um arquivo `.bbl` a partir do seu arquivo de bibliografia `.bib`, execute:

```sh
bibtex artigo

```
Esse comando processa as referências bibliográficas e cria um arquivo `.bbl`, que será utilizado na compilação final do seu documento.

```sh
dvips artigo.dvi
```
Esse comando converte o arquivo `.dvi` em um arquivo PostScript `.ps`, que é um formato de impressão amplamente utilizado.

Por fim, para gerar um arquivo `.pdf` a partir do arquivo `.ps`, execute:

```sh
ps2pdf artigo.ps
```

Através do único comando abaixo é realizado a compilação de um arquivo `.tex` em um `.pdf` utilizando resultados anteriores gerados por meio do `gnuplot` e os os arquivos intermediários:

```sh
gnuplot resultados-tudo.gpi && latex artigo.tex && bibtex artigo && latex artigo.tex && dvips artigo.dvi && ps2pdf artigo.ps && open artigo.pdf && rm artigo.aux artigo.bbl artigo.blg artigo.dvi artigo.ps artigo.log
```

Por meio do comando **make** é possivel otimizar toda a compilação de projetos LaTeX com um unico script, para o mesmo caso, compilar um arquivo `.tex` em um `.pdf`:

```sh
TEX=latex
BIB=bibtex
TRADUZBIB=traduz-bibtex.sh
PSVIEWER=gv
PDFVIEWER=okular
PS2PDF=ps2pdf
DVIVIEWER=xdvi
DVIPSLETTER=dvips -t letter -Ppdf
DVIPSA4=dvips -t a4 -Ppdf
DVIPS=dvips -Ppdf
BARGRAPH=/home/daniel/bin/bargraph.pl.orig
PYTHON=/usr/bin/python
DOT=dot
NEATO=neato

SOURCE=artigo

FIGURAS=

##################################

.SUFFIXES: .tex .pdf .dia .eps .gpi .ps .perf .py .dot .dotr

.dotr.ps:
	${NEATO} -Tps -o $@ $<

.dot.ps:
	dot -Tps -o $@ $<

.py.eps:
	$(PYTHON) $<

.perf.eps:
	${BARGRAPH} -eps $< > $@

.dia.eps:
	dia --nosplash --export=$@ $<

.gpi.eps:
	gnuplot $<

.tex.dvi:
	($(TEX) $< && $(BIB) $(patsubst %.tex,%,$<) && $(TEX) $< && $(TEX) $<) || (rm -f $@ && false)

all: comst

$(SOURCE).dvi: comst.bib

comst: $(FIGURAS)  $(SOURCE).dvi
	$(DVIPSA4) $(SOURCE).dvi -o $(SOURCE).ps
	$(PS2PDF) $(SOURCE).ps $(SOURCE).pdf
	@rm -f *.aux *.dvi *.log *.bbl *.blg $(SOURCE).ps *.lof *.lot *.toc *.out *.bm
	$(PDFVIEWER) $(SOURCE).pdf &

clean:
	@rm -f *.dia~ *.aux *.dvi *.log *.bbl *.blg *.lof *.lot *.toc *.out *.bm ${SOURCE}.pdf
```

Além dos comandos mencionados, a seguir contém alguns comandos que também apresentam funcionalidades interessantes: 
- **texlive-latex-base** – Pacote essencial para compilar documentos LaTeX.
- **texlive-latex-extra** – Conjunto adicional de pacotes LaTeX úteis.
- **texlive-latex-recommended** – Pacotes recomendados para documentos científicos e técnicos.
- **ghostscript** – Manipulação e conversão de arquivos PDF e PostScript.


## **6. Revisão ortográfica e gramatical**
Durante a edição local, é possivel realizar verificação ortográfica e gramatical por meio do [Aspell](http://aspell.net/) que é um verificador ortográfico para terminal. 

Sendo **aspell-en** o dicionário para inglês e o **aspell-pt-br** dicionário para português do Brasil.

```sh
aspell -c -l en artigo.tex
```

Além disso, as ferramentas abaixo fornece funcionalidade de correção gramatical e sugestões:
- **[Grammarly](https://app.grammarly.com/)** – Ferramenta online para correção gramatical e sugestões de estilo em inglês.
- **[Maritaca AI](https://chat.maritaca.ai/)** – IA especializada na revisão de textos acadêmicos em português.
- **[DeepL](https://www.deepl.com)** – Tradutor avançado, útil para textos técnicos e acadêmicos.
- **[QuillBot](https://quillbot.com/pt/corretor-ortografico)** – Ferramenta de parafraseamento alimentada por AI que melhora sua escrita.

## **7. Verificação de plágio**

O website **[Turnitin](https://www.turnitin.com/)** possibilita a verificação de similaridade para detecção de plágio.


## **8. Comparação e controle de versão para documentos LaTeX**
Por meio do **latexdiff** se gera um documento LaTeX destacando as diferenças entre duas versões(adições ficam em azul e as remoções ficam em vermelho e cortadas).
```sh
latexdiff texto-anterior.tex texto-atual.tex > texto-diff.tex
```
Por meio do **diffpdf** é comparar dois pdfs similares destacando as diferenças a nível de pdf (para salvar essa diferença em outro pdf, tem que clicar no botão "Save as"):
```sh
diffpdf artigo.pdf artigo-antes-da-mudanca.pdf
```

Através do controle de versões do **git**, é possivel realizar Controle de versão para rastrear alterações no código-fonte do documento. Para a edição local por meio de um editor de texto de um projeto exitente do Overleaf, realizar um `git clone https://seu-projeto-no-overleaf.git` conforme as [instruções](https://www.overleaf.com/learn/how-to/Git_integration).
Para enviar mudanças locais para o Overleaf:
```sh
git add arquivos_modificados
git commit -m "Descrição das alterações"
git push
```
Para atualizar a cópia local com mudanças feitas no Overleaf:
```sh
git pull
```

Para comparar versões diretamente no Overleaf usando `latexdiff`. (Substitua `HEAD~1` pelo hash da versão desejada.):
```sh
latexdiff-vc -r HEAD~1 arquivo.tex
```

