PROJECT=pfc

# You want latexmk to *always* run, because make does not have all the info.
# Also, include non-file targets in .PHONY so they are run regardless of any
# file of the given name existing.
.PHONY: $(PROJECT).pdf all clean dist-clean tarball $(PROJECT).tgz

# The first rule in a Makefile is the one executed by default ("make"). It
# should always be the "all" rule, so that "make" and "make all" are identical.
all: $(PROJECT).pdf

# CUSTOM BUILD RULES
# In case you didn't know, '$@' is a variable holding the name of the target,
# and '$<' is a variable holding the (first) dependency of a rule.
# "raw2tex" is just placeholder for whatever custom steps
# you might have.
# %.tex: %.raw
#  	./raw2tex $< > $@

$(PROJECT).gls:
	makeglossaries $(PROJECT)

spell: *.tex
	for file in $?; do aspell --mode=tex -c $$file; done

# make can't know all the sourcefiles.  some file may not have
# sourcefiles, e.g. eps's taken from other documents. 
$(PROJECT).tgz: eps images *.tex *.bib *.glossary *.sty Makefile
	tar -czvf $@ $^

tarball: $(PROJECT).tgz

clean:
	latexmk -C

dist-clean: clean
	rm -f *.acn *.acr *.alg *.bbl *.brf *.glg *.glo *.gls *.ist *.maf *.mtc*

# MAIN LATEXMK RULE
# -pdf tells latexmk to generate PDF directly (instead of DVI).
# -pdflatex="" tells latexmk to call a specific backend with specific options.
# -use-make tells latexmk to call make for generating missing files.
#
# -interactive=nonstopmode keeps the pdflatex backend from stopping at a
# missing file reference and interactively asking you for an alternative.

$(PROJECT).pdf:
	latexmk -pdf -pdflatex="pdflatex" -use-make $(PROJECT).tex

