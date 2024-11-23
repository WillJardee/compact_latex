# Default LaTeX engine (can be overridden with 'make LATEX_ENGINE=lualatex')
LATEX_ENGINE ?= pdflatex
BUILD_DIR ?= build/
LATEX_FLAGS := --no-shell-escape --output-directory=$(BUILD_DIR) --use-make --f 

# Find all .tex files in the current directory
TEX_FILES := $(wildcard *.tex)
DVI_FILES := $(patsubst %.tex,%.dvi,$(TEX_FILES))
PDF_FILES := $(patsubst %.tex,%.pdf,$(TEX_FILES))
BUILD_PDF_FILES := $(patsubst %.pdf,$(BUILD_DIR)/%.pdf,$(PDF_FILES))
OPT_PDF_FILES := $(patsubst %.pdf,%_optimized.pdf,$(BUILD_PDF_FILES))
# HTML_FILES := $(patsubst %.tex,%.html,$(TEX_FILES))
# EPUB_FILES := $(patsubst %.tex,%.epub,$(TEX_FILES))

# Directory containing image files
IMG_DIR ?= images/

SRC_DIRS := $(shell find ./* -type d | grep -v -e "^./$(BUILD_DIR)" -e "build")
# TEXINPUTS_VAR := $(patsubst  ./%,$(BUILD_DIR)/%:,$(SRC_DIRS))$(BUILD_DIR):
# TEXINPUTS_VAR := $(shell echo $(TEXINPUTS_VAR) | sed 's/[[:space:]]//g')
TEXINPUTS_VAR:=.:$(BUILD_DIR)//:

# Find all SVG and EPS files in the images directory
SVG_FILES := $(wildcard $(IMG_DIR)/*.svg)
SVG_PDF_FILES := $(patsubst %.svg,%.pdf,$(SVG_FILES))
EPS_FILES := $(wildcard $(IMG_DIR)/*.eps)
EPS_PDF_FILES := $(patsubst %.eps,%.pdf,$(EPS_FILES))

# Check if required tools are installed
IMAGEMAGICK_INSTALLED := $(shell command -v convert >/dev/null 2>&1 && echo yes || echo no)
INKSCAPE_INSTALLED := $(shell command -v inkscape >/dev/null 2>&1 && echo yes || echo no)
EPSTOPDF_INSTALLED := $(shell command -v epspdf >/dev/null 2>&1 && echo yes || echo no)
GS_INSTALLED := $(shell command -v gs >/dev/null 2>&1 && echo yes || echo no)

TEMP_EXT := aux log out toc lof lot fls fdb_latexmk synctex.gz nav snm vrb bbl blg bcf xdv run.xml acn acr alg idx ilg ind tmp makefile SAVE-ERROR

.PHONY: all clean veryclean check-dependencies pdf

# Main targets
all:
	@echo -e "Running default build (dev). Run 'make help' for options.\n"
	@$(MAKE) dev

# Help target to display information about available targets

# Function to display help message
help:
	@echo "Usage: make [target] [options]"
	@echo ""
	@echo "Available targets:"
	@echo "  dev               - Build the project with development settings"
	@echo "  prod              - Build the project with production settings"
	@echo "  clean             - Remove temporary build files"
	@echo "  veryclean         - Remove all output files and the build directory"
	@echo "  check-dependencies - Check if all required tools are installed"
	@echo ""
	@echo "Options:"
	@echo "  LATEX_ENGINE      - Specify the LaTeX engine (pdflatex, lualatex, xelatex)"
	@echo "  BUILD_DIR         - Specify the directory for build output (default: build/)"
	@echo "  LATEX_FLAGS       - Custom LaTeX flags to pass to the LaTeX engine"

# -draftmode option might be on interest to add a togglable
dev: LATEX_FLAGS += -interaction=nonstopmode -synctex=1 -file-line-error -verbose
dev: check-dependencies $(PDF_FILES)

prod: LATEX_FLAGS += --halt-on-error -interaction=batchmode -synctex=0 -quiet  
prod: check-dependencies $(PDF_FILES) $(OPT_PDF_FILES)

# get these guys running
html: prod $(HTML_FILES)
epub: prod $(EPUB_FILES)


clean:
	@if [ -d "$(BUILD_DIR)" ]; then \
		for ext in $(TEMP_EXT); do \
			find $(BUILD_DIR) -type f -name "*$$ext" -delete; \
		done; \
		echo "Build files cleaned."; \
	else \
		echo "Error: $(BUILD_DIR) does not exist. Skipping cleaning."; \
	fi

veryclean: clean
	@echo "Are you sure you want to delete all output files in the build directory (${BUILD_DIR})? [y/N] "
	@read -p "WARNING: This action can be destructive." confirm && \
	if [ "$$confirm" = "y" ]; then \
		if [ -d "$(BUILD_DIR)" ]; then \
			find $(BUILD_DIR) -type f -name "*.pdf" -delete; \
			find $(BUILD_DIR) -type f -name "*.html" -delete; \
			find $(BUILD_DIR) -type f -name "*.epub" -delete; \
			find $(BUILD_DIR) -type f -name "*.dvi" -delete; \
			echo "Output files cleaned."; \
		else \
			echo "Error: $(BUILD_DIR) does not exist. Skipping veryclean."; \
		fi; \
	else \
		echo "veryclean aborted."; \
	fi

# Rule to check if required tools are installed
check-dependencies:
	@echo "Checking dependencies..."
	$(call check_tool,INKSCAPE_INSTALLED,"Inkscape",\
		"SVG to PDF conversion.",\
		"ImageMagick found for SVG to PDF conversion.",\
		"SVG to PDF conversion not supported.")
	$(call check_tool,EPSTOPDF_INSTALLED,"epspdf",\
		"EPS to PDF conversion.",\
		"ImageMagick found for EPS to PDF conversion.",\
		"EPS to PDF conversion not supported.")
	$(call check_tool_simple,GS_INSTALLED,"Ghostscript",\
		"PDF optimization.",\
		"PDF optimization not supported.")

# Function to check a tool with a fallback
define check_tool
	@if [ "$($1)" = "no" ]; then \
		echo "Warning: $2 is not installed. $3"; \
		if [ "$(IMAGEMAGICK_INSTALLED)" = "no" ]; then \
			echo "Warning: $4"; \
		else \
			echo "$5"; \
		fi \
	else \
		echo "$2 found for$3"; \
	fi
endef

# Function to check a tool without a fallback
define check_tool_simple
	@if [ "$($1)" = "no" ]; then \
		echo "Warning: $2 is not installed. $3"; \
	else \
		echo "$2 found for$3."; \
	fi
endef


# Function: Sync directories into BUILD_DIR
sync-dirs:
	@echo "Syncing directory structure to $(BUILD_DIR)..."
	@$(foreach dir,$(SRC_DIRS),mkdir -p $(BUILD_DIR)/$(dir);)
	@echo "Directory structure synced."
	@echo "TEXINPUTS=$(TEXINPUTS_VAR)"
	@echo "SRC_DIRS=$(SRC_DIRS)"


# Rule for SVG to PDF conversion
%.pdf: $(IMG_DIR)/%.svg
	@echo "Converting $< to $@"
	@if [ "$(INKSCAPE_INSTALLED)" = "yes" ]; then \
		inkscape --export-filename=$(BUILD_DIR)/$(IMG_DIR)/$@ $< ; \
	elif [ "$(IMAGEMAGICK_INSTALLED)" = "yes" ]; then \
		magick $< $(BUILD_DIR)/$(IMG_DIR)/$@ ; \
	else \
		echo "Error: Neither Inkscape nor ImageMagick is available for SVG conversion." ; \
		exit 1 ; \
	fi

# Rule for EPS to PDF conversion
%.pdf: %.eps
	@echo "Converting $< to $@"
	@if [ "$(EPSTOPDF_INSTALLED)" = "yes" ]; then \
		epspdf $< $(BUILD_DIR)/$(IMG_DIR)/$@; \
	elif [ "$(IMAGEMAGICK_INSTALLED)" = "yes" ]; then \
		magick $< $(BUILD_DIR)/$(IMG_DIR)/$@ ; \
	else \
		echo "Error: Neither epspdf or ImageMagick is available for EPS conversion." ; \
		exit 1 ; \
	fi

# Rule for TEX to PDF conversion
%.pdf: %.tex sync-dirs
	# export TEXINPUTS=$(TEXINPUTS_VAR)
	@if [ "$(LATEX_ENGINE)" = "latexmk" ]; then \
		TEXINPUTS=$(TEXINPUTS_VAR) latexmk $(LATEX_FLAGS) $< ; \
	elif [ "$(LATEX_ENGINE)" = "pdflatex" ]; then \
		TEXINPUTS=$(TEXINPUTS_VAR) latexmk -pdflatex $(LATEX_FLAGS) $< ; \
	elif [ "$(LATEX_ENGINE)" = "lualatex" ]; then \
		latexmk -lualatex $(LATEX_FLAGS) $< ; \
	elif [ "$(LATEX_ENGINE)" = "xelatex" ]; then \
		latexmk -xelatex $(LATEX_FLAGS) $< ; \
	else \
		echo "Error: Unsupported LATEX_COMPILER value: $(LATEX_COMPILER)" ; \
		exit 1 ; \
	fi

# PDF optimization
%_optimized.pdf: %.pdf
	@echo "Optimizing PDFs..."
	@if [ "$(GS_INSTALLED)" = "yes" ]; then \
		gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/printer -dNOPAUSE -dQUIET -dBATCH \
			-sOutputFile=$@ $< ; \
		echo "Optimized PDF created: $@" ; \
	else \
		echo "Ghostscript not found. Skipping PDF optimization." ; \
	fi


# # Rule for HTML to PDF conversion
# %.html: %.tex
# 	@if [ "$(PANDOC_INSTALLED)" = "yes" ]; then \
# 		pandoc $< -o $@; \
# 		echo "HTML version created: $@"; \
# 	else \
# 		echo "Pandoc not found. Skipping HTML conversion."; \
# 	fi
#
# # Rule for EPUB to PDF conversion
# %.epub: %.tex
# 	@if [ "$(PANDOC_INSTALLED)" = "yes" ]; then \
# 		pandoc $< -o $@ ; \
# 		echo "EPUB version created: $@" ; \
# 	else \
# 		echo "Pandoc not found. Skipping EPUB conversion." ; \
# 	fi

# Watcher for continuous compilation
watch: LATEX_FLAGS += --pvc 
watch: dev
