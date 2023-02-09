SOURCES ?= $(wildcard *.pdf)

%.png: %.pdf
	convert $< $@

all: $(SOURCES:%.pdf=%.png)
