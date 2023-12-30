#!/usr/bin/make -f
#
# make -h
# man make
# info make
# /usr/share/doc/make-doc/make.pdf.gz
# file:///usr/share/doc/make-doc/make.html/index.html
#
# http://ctan.org
# http://latex-project.org
# http://wikibooks.org/wiki/LaTeX
# http://wikibooks.org/wiki/LaTeX/Bibliography_Management
# http://wikibooks.org/wiki/LaTeX/Bibliographies_with_biblatex_and_biber
# http://www.ctan.org/tex-archive/info/bibtex/tamethebeast/ttb_en.pdf
# http://mirrors.ctan.org/macros/latex/contrib/biblatex/doc/biblatex.pdf
# http://mirrors.ctan.org/macros/latex/contrib/jurabib/docs/english/jbendoc.pdf
# http://mirrors.ctan.org/macros/latex/contrib/natbib/natbib.pdf
#
# aux	AUXliary file for temporary data
# bcf	Biber Control File
# blg	Bibtex LoG file
# bbl	Bibtex BibLiography
# bbx	Biblatex Bibliography style eXtension?
# bib	BIBliography database file
# bst	Bibliography STyle file
# cbx	Citation style BibteX
# cfg	latex ConFiGuration file
# dvi	DeVice Independent file
# lbx	Language definition file BibteX
# idx	InDeX entrys
# ilg	makeIndex LoG
# lat	spreadscheet calculator default extension for LATex export file
# lof	List Of Figures
# log	latex LOG file
# out	Bookmark file
# pdf	Postscript Document File
# pfb	Postscript Font Binary
# run.xml logreq request file /usr/share/doc/texlive-doc/latex/logreq/README
# sc	Spreadsheet Calculator
# tex	TeX, LaTeX TEXt file
# tfm	Tex Font Metric Data
# toc	babel Table Of Content
# vf	tex Virtual Font data

# Find out what original files you have on directory and
# create needed temporary files for typesetting program.
scfiles  = $(wildcard *.sc)
sc2lat   = $(patsubst %.sc,%.lat,$(scfiles))
lat2tex  = $(patsubst %.lat,%.tex,$(sc2lat))
texfiles = $(wildcard *.tex) $(lat2tex)

# Definitions:
DOC = general
SUBM = fig
#LATEX = latex
#LATEX = xelatex
LATEX = lualatex # latex
SC2LAT = echo "set tblstyle=latex\ntbl \"\"" | sc -q
BIBTEX = bibtex # nbibtex, bibtexu aux->bbl
BIBER = biber # bcf->bbl
CLEAN =	lintex -o -k -v && rm -f clean all $(sc2lat) $(lat2tex) *.xml
MAKEINDEX = makeindex
#LATEXMK = latexmk

# Clear suffixlist
.SUFFIXES:
# Add suffixrule supported suffix to suffixes list.
#.SUFFIXES: .sc .lat .pdf
.SUFFIXES: .sc .lat

# Define implicit rule for added suffix:
#%.pdf: %.tex $(texfiles)
#	make -C $(SUBM)
#	$(LATEXMK) $< $@
%.lat: %.sc
	$(SC2LAT) $<
%.tex: %.lat
	cp $< $@

# Targets:
.PHONY: all
all: $(DOC).pdf 

$(DOC).aux $(DOC).bcf $(DOC).idx : $(DOC).tex $(texfiles)
	make -C $(SUBM)
	$(LATEX) $(DOC).tex

#$(DOC).bbl : $(DOC).aux ; $(BIBTEX) $(DOC).aux

$(DOC).bbl : $(DOC).bcf ; $(BIBER) $(DOC).bcf

$(DOC).ind : $(DOC).idx ; $(MAKEINDEX) $(DOC).idx

$(DOC).pdf : $(DOC).bbl $(DOC).ind $(DOC).tex ; $(LATEX) $(DOC).tex

#$(DOC).dvi : $(DOC).bbl $(DOC).ind $(DOC).tex ; $(LATEX) $(DOC).tex

#$(DOC).ps : $(DOC).dvi ; $(DVIPS) $(DOC).ps $(DOC).dvi

#$(DOC).pdf : $(DOC).ps ; $(PS2PDF)$(DOC).pdf $(DOC).ps

# Clean: Remove temporary files.
.PHONY: clean 
clean :
	touch $(DOC).pdf
	mv -f $(DOC).pdf ..
	$(CLEAN)
	make -C $(SUBM) clean

