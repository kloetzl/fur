SRC1 =   cleanSeq.tex fur.tex madis.tex makeFurDb.tex stream.tex tutorial.tex util.tex
SRC2 =   header.tex intro.tex furDoc.tex

date = $(shell git log | grep Date | head -n 1 | sed -r 's/Date: +[A-Z][a-z]+ ([A-Z][a-z]+) ([0-9]+) [^ ]+ ([0-9]+) .+/\2_\1_\3/')
version = $(shell git describe)

all: furDoc.pdf 
furDoc.pdf: furDoc.tex $(SRC1) $(SRC2) version.txt date.txt ../stream/time_*.ps ../stream/memory.ps ../makeFurDb/mlAlg.tex
	latex furDoc.tex
	bibtex furDoc
	latex furDoc.tex
	latex furDoc.tex
	dvips furDoc -o -q
	ps2pdf -dALLOWPSTRANSPARENCY furDoc.ps
%.tex: ../*/%.org furDoc.tex
	bash ../scripts/org2nw $< | awk -f ../scripts/preWeave.awk | noweave -n -x | sed 's/_test/\\_test/g' > $@
version.txt: ../.git/COMMIT_EDITMSG
	echo $(version) | tr '-' ' ' | awk '{printf "%s", $$1; if ($$2) printf "-%s", $$2; printf "\n"}' > version.txt
date.txt: ../.git/COMMIT_EDITMSG
	echo $(date) | tr '_' ' ' > date.txt
../makeFurDb/time.ps:
	make -C makeFurDb
../makeFurDb/memory.ps:
	make -C makeFurDb
clean:
	rm -f furDoc.pdf furDoc.ps furDoc.dvi furDoc.aux furDoc.bbl  furDoc.blg  furDoc.dvi  furDoc.log $(SRC1) version.txt date.txt
