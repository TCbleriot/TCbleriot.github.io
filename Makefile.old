DIR_HOST = TCbleriot
DIR_SRC = ./src
DIR_OUT = ./docs
DIR_SITE = ./site

MAIN_RMD = $(shell find $(DIR_SRC) -maxdepth 1 -name "*.Rmd")
MAIN_FILE = $(shell basename --suffix=.Rmd -a $(MAIN_RMD) "")
MAIN_MD := $(patsubst %, $(DIR_OUT)/%.md, $(MAIN_FILE))
SRC_FILE = $(shell find $(DIR_SRC) -not -path '*/\.*')

.PHONY: all clean 

all: $(DIR_SITE)/index.html

$(DIR_SITE)/index.html: $(MAIN_MD) $(SRC_FILE) 
	mkdir -p $(DIR_OUT)
	cp -rf $(DIR_SRC)/* $(DIR_OUT)/
	if [ -d figure ]; then rm -rf $(DIR_OUT)/figure && mv figure $(DIR_OUT)/; fi
	mkdocs build --clean
	
$(DIR_OUT)/%.md: $(DIR_SRC)/%.Rmd
	mkdir -p $(DIR_OUT)
	Rscript -e 'knitr::knit("$<", output="$@")'

clean:
	rm -fR $(DIR_OUT)
	rm -fR $(DIR_SITE)

publish:
	rsync --exclude=".*" --chown="dymaphy:shinyUsers" --chmod=775 -Pe  ssh -av --delete-after "$(DIR_SITE)/" mawenzi:/data/www/shiny/$(DIR_HOST)
