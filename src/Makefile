# This is a makefile that builds the GRN morphological parser
# *****************************************************************
# This is a dummy file. GRN refers to the name of the language, grn to the 
# ISO code of the language, for use in file names.

# This is a preliminary file that builds a GRN parser based upon
# xfst and not twolc.

# Here we build the final generator , an inverted transducer of the analyzer.
# It is dependent upon grn.save

XFST = xfst #-utf8
LEXC = lexc #-utf8

SRCS = grn-lex.txt noun-grn-morph.txt noun-grn-lex.txt \
	adj-grn-lex.txt adv-grn-lex.txt \
	verb-grn-lex.txt verb-grn-morph.txt \
	pron-grn-lex.txt \
	pp-grn-lex.txt \
	num-grn-lex.txt \
	cc-grn-lex.txt \
	interj-grn-lex.txt \
	cs-grn-lex.txt \
	punct-grn-lex.txt

# This is the ultimate goal, the inverse transducer

igrn.fst: ../bin/igrn.fst
../bin/igrn.fst: ../bin/grn.fst
	@echo
	@echo "*** Building the inverse igrn.fst ***"
	@echo
	@printf "load ../bin/grn.save \n\
	invert net \n\
	save stack ../bin/igrn.fst \n\
	quit \n" > ../../tmp/igrn-fst-script
	$(XFST) < ../../tmp/igrn-fst-script
	@rm -f ../../tmp/igrn-fst-script


# This goal is to build the final analyser. It depends on all the files.

grn.fst: ../bin/grn.fst
../bin/grn.fst: ../bin/grn.save ../bin/inituppercase.fst ../bin/tok.fst 
	@echo
	@echo "*** Building grn.fst ***" ;
	@echo
	@printf "read regex [[@\"../bin/grn.save\"] .o. \
	[@\"../bin/inituppercase.fst\"]] ; \n\
	save stack ../bin/grn.fst \n\
	quit \n" > ../../tmp/grn-fst-script
	$(XFST) < ../../tmp/grn-fst-script
	@rm -f ../../tmp/grn-fst-script


# The second goal is to build the inituppercase.fst file
# This goal depends on inituppercase.regex

inituppercase.fst: ../bin/inituppercase.fst
../bin/inituppercase.fst: inituppercase.regex
	@echo
	@echo "*** Building inituppercase.fst ***" ;
	@echo
	@printf "read regex < inituppercase.regex \n\
	save stack ../bin/inituppercase.fst \n\
	quit \n" > ../../tmp/inituppercase-script
	$(XFST) < ../../tmp/inituppercase-script
	@rm -f ../../tmp/inituppercase-script


# Another goal is to build a preprocessor.This goal depends on tok.txt

tok.fst: ../bin/tok.fst
../bin/tok.fst: tok.txt
	@echo
	@echo "*** Building the tokenizer tok.fst ***" ;
	@echo
	@printf "source tok.txt \n\
	save stack ../bin/tok.fst \n\
	quit \n" > ../../tmp/tok-script
	$(XFST) < ../../tmp/tok-script
	@rm -f ../../tmp/tok-script


grn.save: ../bin/grn.save
../bin/grn.save: ../bin/xfst-grn.bin ../bin/grn-lex.save
	@echo
	@echo "*** Building the parser grn.save ***"
	@echo
	@printf "read regex [[@\"../bin/grn-lex.save\"] .o. \
	[@\"../bin/xfst-grn.bin\"]] ; \n\
	save stack ../bin/grn.save \n\
	quit \n" > ../../tmp/grn-save-script
	$(XFST) < ../../tmp/grn-save-script
	@rm -f ../../tmp/grn-save-script

# The first goal is to build grn-lex.save
# This goal depends on a bunch of lexicon files

../tmp/grn-lex-all.tmp: 
	cat $(SRCS) > $@	
#	cat $(SRCS) > tmpsrc.tmp

grn-lex.save: ../bin/grn-lex.save
../bin/grn-lex.save: ../tmp/grn-lex-all.tmp
	@echo
	@echo "*** Building grn-lex.save ***" ;
	@echo
	printf "read lexc ../tmp/grn-lex-all.tmp \n\
	save stack ../bin/grn-lex.save \n\
	quit \n" > ../../tmp/grn-lex-save-script
	$(XFST) < ../../tmp/grn-lex-save-script
	rm -f ../../tmp/grn-lex-save-script

# The second goal is to build xfst-grn.bin
# This goal depends on xfst-grn.txt

xfst-grn.bin: ../bin/xfst-grn.bin
../bin/xfst-grn.bin: xfst-grn.txt
	@echo
	@echo "*** Building xfst-grn.bin ***" ;
	@echo
	@printf "source xfst-grn.txt \n\
	save stack ../bin/xfst-grn.bin \n\
	quit \n" > ../../tmp/xfst-grn-script
	$(XFST) < ../../tmp/xfst-grn-script
	@rm -f ../../tmp/xfst-grn-script

clean:
	@rm -f ../bin/*.bin ../bin/*.fst ../bin/*.save ../tmp/grn-lex-all.tmp


