# This is a makefile that builds the XXX morphological parser
# *****************************************************************
# This is a dummy file. XXX refers to the name of the language, xxx to the 
# ISO code of the language, for use in file names.

# This is a preliminary file that builds a XXX parser based upon
# xfst and not twolc.

# Here we build the final generator , an inverted transducer of the analyzer.
# It is dependent upon xxx.save

ixxx.fst: ../bin/ixxx.fst
../bin/ixxx.fst: ../bin/xxx.fst
	@echo
	@echo "*** Building the inverse ixxx.fst ***"
	@echo
	@printf "load ../bin/xxx.save \n\
	invert net \n\
	save stack ../bin/ixxx.fst \n\
	quit \n" > ../../tmp/ixxx-fst-script
	@xfst < ../../tmp/ixxx-fst-script
	@rm -f ../../tmp/ixxx-fst-script


# This goal is to build the final analyser. It depends on all the files.

xxx.fst: ../bin/xxx.fst
../bin/xxx.fst: ../bin/xxx.save ../bin/caseconv.fst ../bin/tok.fst 
	@echo
	@echo "*** Building xxx.fst ***" ;
	@echo
	@printf "read regex [[@\"../bin/xxx.save\"] .o. \
	[@\"../bin/caseconv.fst\"]] ; \n\
	save stack ../bin/xxx.fst \n\
	quit \n" > ../../tmp/xxx-fst-script
	@xfst < ../../tmp/xxx-fst-script
	@rm -f ../../tmp/xxx-fst-script


# The second goal is to build the caseconv.fst file
# This goal depends on case.regex

caseconv.fst: ../bin/caseconv.fst
../bin/caseconv.fst: case.regex
	@echo
	@echo "*** Building caseconv.fst ***" ;
	@echo
	@printf "read regex < case.regex \n\
	save stack ../bin/caseconv.fst \n\
	quit \n" > ../../tmp/caseconv-script
	@xfst < ../../tmp/caseconv-script
	@rm -f ../../tmp/caseconv-script


# Another goal is to build a preprocessor.This goal depends on tok.txt

tok.fst: ../bin/tok.fst
../bin/tok.fst: tok.txt
	@echo
	@echo "*** Building the tokenizer tok.fst ***" ;
	@echo
	@printf "source tok.txt \n\
	save stack ../bin/tok.fst \n\
	quit \n" > ../../tmp/tok-script
	@xfst < ../../tmp/tok-script
	@rm -f ../../tmp/tok-script


xxx.save: ../bin/xxx.save
../bin/xxx.save: ../bin/xfst-xxx.bin ../bin/xxx-lex.save
	@echo
	@echo "*** Building the parser xxx.save ***"
	@echo
	@printf "read regex [[@\"../bin/xxx-lex.save\"] .o. \
	[@\"../bin/xfst-xxx.bin\"]] ; \n\
	save stack ../bin/xxx.save \n\
	quit \n" > ../../tmp/xxx-save-script
	@xfst < ../../tmp/xxx-save-script
	@rm -f ../../tmp/xxx-save-script

# The first goal is to build xxx-lex.save
# This goal depends on a bunch of lexicon files

xxx-lex.save: ../bin/xxx-lex.save
../bin/xxx-lex.save: xxx-lex.txt noun-xxx-morph.txt noun-xxx-lex.txt \
        adj-xxx-lex.txt \
	adv-xxx-lex.txt verb-xxx-lex.txt pron-xxx-lex.txt punct-xxx-lex.txt \
	pp-xxx-lex.txt
	@echo
	@echo "*** Building xxx-lex.save ***" ;
	@echo
	printf "compile-source xxx-lex.txt noun-xxx-morph.txt noun-xxx-lex.txt \
	adj-xxx-lex.txt verb-xxx-lex.txt verb-xxx-morph.txt pron-xxx-lex.txt \
	adv-xxx-lex.txt pp-xxx-lex.txt punct-xxx-lex.txt \n\
	save-source ../bin/xxx-lex.save \n\
	quit \n" > ../../tmp/xxx-lex-save-script
	@lexc < ../../tmp/xxx-lex-save-script
	rm -f ../../tmp/xxx-lex-save-script

# The second goal is to build xfst-xxx.bin
# This goal depends on xfst-xxx.txt

xfst-xxx.bin: ../bin/xfst-xxx.bin
../bin/xfst-xxx.bin: xfst-xxx.txt
	@echo
	@echo "*** Building xfst-xxx.bin ***" ;
	@echo
	@printf "source xfst-xxx.txt \n\
	save stack ../bin/xfst-xxx.bin \n\
	quit \n" > ../../tmp/xfst-xxx-script
	@xfst < ../../tmp/xfst-xxx-script
	@rm -f ../../tmp/xfst-xxx-script

clean:
	@rm -f ../bin/*.bin ../bin/*.fst ../bin/*.save


