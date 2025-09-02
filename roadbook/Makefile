# Makefile

# Eingaben und Ziele
SOURCES := $(filter-out README.md,$(wildcard *.md))
PDFDIR  := pdf
PDFS    := $(patsubst %.md,$(PDFDIR)/%.pdf,$(SOURCES))
MERGED  := $(PDFDIR)/_ALL.pdf
NUP4    := $(PDFDIR)/all-4up-a4.pdf

# Standardziel
.DEFAULT_GOAL := all

.PHONY: all clean 4up merge

# Baue alle PDFs
all: 4up

# Zielordner anlegen, falls nicht vorhanden
$(PDFDIR):
	@mkdir -p $(PDFDIR)

# Regel: Markdown -> PDF (Pandoc mit roadbook.yaml)
# --pdf-engine-opt leitet LaTeX-Hilfsdateien in pdf/ um
$(PDFDIR)/%.pdf: %.md | $(PDFDIR)
	pandoc $< --defaults=roadbook.yaml -o $@ --pdf-engine-opt=-output-directory=$(PDFDIR)
	@rm -f $(PDFDIR)/input.*

# Bei Änderungen an YAML (und optional preamble.tex) neu bauen
EXTRA_DEPS := roadbook.yaml preamble.tex
$(PDFS): $(EXTRA_DEPS)

# Aufräumen: Zielordner löschen
clean:
	@rm -rf $(PDFDIR)

# Für Druck alle zusammenfassen
merge: $(MERGED)
$(MERGED): $(PDFS) | $(PDFDIR)
	@pdfunite $(PDFS) $(MERGED)

# und auf A4 verteilen
4up: $(NUP4)
$(NUP4): $(PDFS)
	pdfjam $< --nup 2x2 --paper a4paper --outfile $@
