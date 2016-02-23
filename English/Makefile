DOCUMENT=poly
# vide implique que le fichier est $(DOCUMENT) s'il existe:
BIBFILES=
VIEWER=open
TYPEVIEWER=ps
IMAGESFOLDER=figures
FILETOSAVE=$(DOCUMENT).pdf
DEPENDS_DOCUMENT= $(wildcard *.tex)
USEDPACKAGES=

# Les programmes externes:

DVIVIEWER=dvi
PSVIEWER=kghostview
PDFVIEWER=open

GRAPHICS=$(wildcard $(IMAGESFOLDER)/*.eps $(IMAGESFOLDER)/*.jpg $(IMAGESFOLDER)/*.gif)

BIBFILES_TMP=$(if $(BIBFILES),$(BIBFILES),$(DOCUMENT))
# Obtain the list of bibfile existing in the FS :
BIBFILES_EXISTS= $(wildcard $(BIBFILES_TMP:%=%.bib))

# Fichier généré par défaut (ps ou dvi généralement)

all: dvi

#
# Les raccourcis vers les types de documents :
#
ps: $(DOCUMENT).ps

ps-4: $(DOCUMENT)-4.ps

pdf: $(DOCUMENT).pdf

pdf-4: $(DOCUMENT)-4.pdf

dvi: $(DOCUMENT).dvi

#
# Fichiers PDF :
#
$(DOCUMENT).pdf: $(DOCUMENT).ps
	ps2pdf $(DOCUMENT).ps

$(DOCUMENT)-4.pdf: $(DOCUMENT)-4.ps
	ps2pdf $(DOCUMENT)-4.ps

#
# Fichiers PS :
#
$(DOCUMENT).ps:$(DOCUMENT).dvi
	dvips -o $(DOCUMENT).ps $(DOCUMENT).dvi

$(DOCUMENT)-4.ps: $(DOCUMENT).ps
	a2ps -4 $(DOCUMENT).ps -o $(DOCUMENT)-4.ps

EFRAME= '********************************************************************************'
ELATEX= '************************************ LATEX *************************************'
EBIBTEX= '*********************************** BIBTEX *************************************'

#
# Fichiers DVI BBL :
#
$(DOCUMENT).dvi: $(DOCUMENT).tex $(BIBFILES_EXISTS) $(GRAPHICS) $(DEPENDS_DOCUMENT)
	make -f Makefile release

beta:
	@latex $(DOCUMENT).tex

release:
	@latex $(DOCUMENT).tex
	@if [ -n '$(BIBFILES_EXISTS)' ]; \
	then \
		echo  -e $(EFRAME)'\n'$(EBIBTEX)'\n'$(EFRAME) && \
		bibtex $(DOCUMENT) ; \
	fi
	@echo $(EFRAME)
	@echo $(ELATEX)
	@echo $(EFRAME)
	@latex $(DOCUMENT).tex
	@echo $(EFRAME)
	@echo $(ELATEX)
	@echo $(EFRAME)
	@latex $(DOCUMENT).tex

releasepdf:
	@pdflatex $(DOCUMENT).tex
	@if [ -n '$(BIBFILES_EXISTS)' ]; \
	then \
		echo  -e $(EFRAME)'\n'$(EBIBTEX)'\n'$(EFRAME) && \
		bibtex $(DOCUMENT) ; \
	fi
	@echo $(EFRAME)
	@echo $(ELATEX)
	@echo $(EFRAME)
	@pdflatex $(DOCUMENT).tex
	@echo $(EFRAME)
	@echo $(ELATEX)
	@echo $(EFRAME)
	@pdflatex $(DOCUMENT).tex

$(DOCUMENT).bbl:$(DOCUMENT).bib
	bibtex $(DOCUMENT)

#
# sauvegarde sur clef USB ou fichier tar.gz :
#
save:
	tar hczvf $(DOCUMENT)-`date +%d-%m-%Y_%H-%M`.tar.gz $(wildcard $(DOCUMENT).tex $(DOCUMENT).bib $(USEDPACKAGES) $(IMAGESFOLDER) Makefile $(FILETOSAVE)) $(DEPENDS_DOCUMENT)

save_usb: save
	cp -u $(DOCUMENT)-*-*-*.tar.gz /mnt/clef/

#
# Nettoyage du dossier :
#
clean:
	rm -f $(DOCUMENT).aux $(DOCUMENT).bbl $(DOCUMENT).blg $(DOCUMENT).log $(DOCUMENT).toc *.aux $(DOCUMENT).brf $(DOCUMENT).out $(DOCUMENT).lof

cleanall: clean
	rm -f $(DOCUMENT).ps $(DOCUMENT).pdf $(DOCUMENT)-4.ps $(DOCUMENT)-4.pdf $(DOCUMENT).dvi

#
# prévisualisation du fichier :
#

vie vi view: $(DOCUMENT).$(TYPEVIEWER)
	$(VIEWER) $(DOCUMENT).$(TYPEVIEWER)

view-dvi: $(DOCUMENT).dvi
	$(DVIVIEWER) $(DOCUMENT).dvi

view-ps: $(DOCUMENT).ps
	$(PSVIEWER) $(DOCUMENT).ps

view-pdf: $(DOCUMENT).pdf
	$(PDFVIEWER) $(DOCUMENT).pdf

.PRECIOUS: $(DOCUMENT).tex $(DOCUMENT).bib Makefile $(USEDPACKAGES)
.PHONY: all view view-dvi view-ps view-pdf pdf pdf-4 ps ps-4 dvi clean cleanall save save_usb
.DEFAULT_GOAL := all
.DEFAULT:
	$(error $<: je ne sais pas comment construire ce fichier.)

