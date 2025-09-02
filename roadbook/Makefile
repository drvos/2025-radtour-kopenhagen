# Makefile

# Eingaben und Ziele
SOURCES := $(filter-out README.md,$(wildcard *.md))
PDFDIR  := pdf
PDFS    := $(patsubst %.md,$(PDFDIR)/%.pdf,$(SOURCES))

# Standardziel
.DEFAULT_GOAL := all

.PHONY: all clean

# Baue alle PDFs
all: $(PDFS)

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

