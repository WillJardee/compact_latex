LATEX = latexmk

# Directory containing SVG files
IMG_DIR := images

# Find all SVG files in the images directory
SVG_FILES := $(wildcard $(IMG_DIR)/*.svg)

# Define the corresponding PDF files
PDF_FILES := $(patsubst %.svg,%.pdf,$(SVG_FILES))

# Check if ImageMagick is installed
IMAGEMAGICK_INSTALLED := $(shell command -v convert >/dev/null 2>&1 && echo yes || echo no)

default: check-imagemagick convert-pdfs pdf

.PHONY: all clean check-imagemagick convert-pdfs

clean:
	${LATEX} -c
	rm -f *.vtc *.bbl *.snm *.nav *.run.xml $(IMG_DIR)/*.pdf

veryclean: clean
	${LATEX} -C

# Rule to check if ImageMagick is installed
check-imagemagick:
ifeq ($(IMAGEMAGICK_INSTALLED),yes)
	@echo "ImageMagick is installed."
else
	@echo "Warning: ImageMagick is not installed. SVG to PDF conversion will be skipped."
endif

# Phony target to convert all SVG files to PDF every time
convert-pdfs:
ifeq ($(IMAGEMAGICK_INSTALLED),yes)
	@echo "Converting SVG files to PDF..."
	@for svg in $(SVG_FILES); do \
		pdf=$${svg%.svg}.pdf; \
		echo "Converting $$svg to $$pdf"; \
		convert $$svg $$pdf; \
	done
else
	@echo "Skipping SVG to PDF conversion because ImageMagick is not installed."
endif

pdf:
	${LATEX} -pdf -f

pdf-watcher:
	${LATEX} -pvc -pdf

