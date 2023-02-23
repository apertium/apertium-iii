###############################################################################
## Makefile for apertium-iii
###############################################################################

LANG1=iii
SCRIPT1=Lat
SCRIPT2=Yi
LANG1SCRIPT1=$(LANG1)@$(SCRIPT1)
LANG1SCRIPT2=$(LANG1)@$(SCRIPT2)
BASENAME=apertium-$(LANG1)

TARGETS_COMMON =			\
	$(LANG1).automorf.hfst		\
	$(LANG1).automorf.bin		\
	$(LANG1).automorf.att.gz	\
	$(LANG1SCRIPT1).autogen.hfst		\
	$(LANG1SCRIPT1).autogen.bin		\
	$(LANG1SCRIPT1).autogen.att.gz		\
	$(LANG1SCRIPT2).automorf.hfst		\
   $(LANG1SCRIPT2).autotok.hfst     \
	$(LANG1SCRIPT2).autogen.hfst		\
	$(LANG1SCRIPT2).autogen.bin		\
	$(LANG1SCRIPT2).autogen.att.gz		\
	$(LANG1).autopgen.bin                  \
	$(LANG1).rlx.bin		\
	.deps/$(SCRIPT2)-$(SCRIPT1).hfst

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

LEXCFLAGS=-v --Werror

###############################################################################
## Sichuan Yi transducer
###############################################################################

.deps/$(LANG1).twol.hfst: $(BASENAME).$(LANG1).twol .deps/.d
	hfst-twolc $< -o $@

.deps/$(LANG1).RL.lexd: $(BASENAME).$(LANG1).lexd .deps/.d
	cat $< | grep -v 'Dir/LR' > $@

.deps/$(LANG1).LR.lexd: $(BASENAME).$(LANG1).lexd .deps/.d
	cat $< | grep -v 'Dir/RL' > $@

.deps/$(LANG1).RL.lexd.hfst: .deps/$(LANG1).RL.lexd
	lexd $< .deps/$(LANG1).RL.lexd.att
	hfst-txt2fst .deps/$(LANG1).RL.lexd.att -o $@

.deps/$(LANG1).LR.lexd.hfst: .deps/$(LANG1).LR.lexd
	lexd $< .deps/$(LANG1).LR.lexd.att
	hfst-txt2fst .deps/$(LANG1).LR.lexd.att -o $@


.deps/$(LANG1).RL.hfst: .deps/$(LANG1).RL.lexd.hfst .deps/$(LANG1).twol.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).RL.lexd.hfst -2 .deps/$(LANG1).twol.hfst -o $@

.deps/$(LANG1).LR.hfst: .deps/$(LANG1).LR.lexd.hfst .deps/$(LANG1).twol.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).LR.lexd.hfst -2 .deps/$(LANG1).twol.hfst -o $@

$(LANG1SCRIPT1).autogen.hfst: .deps/$(LANG1).RL.hfst
	hfst-fst2fst -O $< -o $@

$(LANG1SCRIPT2).autogen.hfst: .deps/$(LANG1).RL.hfst .deps/$(SCRIPT1)-$(SCRIPT2).hfst
	hfst-compose -1 $< -2 .deps/$(SCRIPT1)-$(SCRIPT2).hfst | hfst-fst2fst -O -o $@

.deps/$(LANG1SCRIPT1).spellrelax.hfst: $(BASENAME).$(LANG1SCRIPT1).spellrelax .deps/.d
	hfst-regexp2fst -S -o $@ $<

.deps/$(LANG1SCRIPT2).spellrelax.hfst: $(BASENAME).$(LANG1SCRIPT2).spellrelax .deps/.d
	hfst-regexp2fst -S -o $@ $<

.deps/$(LANG1SCRIPT1).automorf.hfst: .deps/$(LANG1).LR.hfst .deps/$(LANG1SCRIPT1).spellrelax.hfst .deps/.d
	hfst-compose -1 $< -2 .deps/$(LANG1SCRIPT1).spellrelax.hfst | hfst-invert -o $@

.deps/$(SCRIPT1)-$(SCRIPT2).lexd.hfst: $(BASENAME).$(SCRIPT1)-$(SCRIPT2).lexd .deps/.d
	lexd -U $< .deps/$(SCRIPT1)-$(SCRIPT2).att
	hfst-txt2fst .deps/$(SCRIPT1)-$(SCRIPT2).att | hfst-invert -o $@

.deps/$(SCRIPT1)-$(SCRIPT2).twol.hfst: $(BASENAME).$(SCRIPT1)-$(SCRIPT2).twol .deps/.d
	hfst-twolc $< -o $@

.deps/$(SCRIPT1)-$(SCRIPT2).hfst: .deps/$(SCRIPT1)-$(SCRIPT2).lexd.hfst .deps/$(SCRIPT1)-$(SCRIPT2).twol.hfst .deps/.d
	hfst-compose-intersect -1 .deps/$(SCRIPT1)-$(SCRIPT2).lexd.hfst -2 .deps/$(SCRIPT1)-$(SCRIPT2).twol.hfst -o $@

.deps/$(SCRIPT2)-$(SCRIPT1).hfst: .deps/$(SCRIPT1)-$(SCRIPT2).hfst
	hfst-invert $< -o $@

.deps/$(LANG1SCRIPT2).LR.hfst: .deps/$(LANG1).LR.hfst .deps/$(SCRIPT1)-$(SCRIPT2).hfst .deps/.d
	hfst-compose -1 $< -2 .deps/$(SCRIPT1)-$(SCRIPT2).hfst -o $@

.deps/$(LANG1SCRIPT2).automorf.hfst: .deps/$(LANG1SCRIPT2).LR.hfst .deps/$(LANG1SCRIPT2).spellrelax.hfst .deps/.d
	hfst-compose -1 $< -2 .deps/$(LANG1SCRIPT2).spellrelax.hfst | hfst-invert -o $@

$(LANG1SCRIPT2).automorf.hfst: .deps/$(LANG1SCRIPT2).automorf.hfst
		cp $< $@

$(LANG1SCRIPT2).autotok.hfst: .deps/$(LANG1SCRIPT2).automorf.hfst
		hfst-fst2fst -F $< | hfst-project -p input | hfst-minimise -o $@

$(LANG1).automorf.hfst: .deps/$(LANG1SCRIPT1).automorf.hfst .deps/$(LANG1SCRIPT2).automorf.hfst
	hfst-union -1 .deps/$(LANG1SCRIPT1).automorf.hfst -2 .deps/$(LANG1SCRIPT2).automorf.hfst | hfst-fst2fst -O -o $@

$(LANG1SCRIPT1).autogen.att.gz: $(LANG1SCRIPT1).autogen.hfst
	hfst-fst2txt $< | gzip -9 -c -n > $@

$(LANG1SCRIPT2).autogen.att.gz: $(LANG1SCRIPT2).autogen.hfst
	hfst-fst2txt $< | gzip -9 -c -n > $@

$(LANG1).automorf.att.gz: $(LANG1).automorf.hfst
	hfst-fst2txt $< | gzip -9 -c -n > $@

$(LANG1SCRIPT1).autogen.bin: $(LANG1SCRIPT1).autogen.att.gz .deps/.d
	zcat < $< > .deps/$(LANG1SCRIPT1).autogen.att
	lt-comp lr .deps/$(LANG1SCRIPT1).autogen.att $@

$(LANG1SCRIPT2).autogen.bin: $(LANG1SCRIPT2).autogen.att.gz .deps/.d
	zcat < $< > .deps/$(LANG1SCRIPT2).autogen.att
	lt-comp lr .deps/$(LANG1SCRIPT2).autogen.att $@

$(LANG1).automorf.bin: $(LANG1).automorf.att.gz .deps/.d
	zcat < $< > .deps/$(LANG1).automorf.att
	lt-comp lr .deps/$(LANG1).automorf.att $@

$(LANG1).autopgen.bin: $(BASENAME).post-$(LANG1).dix
	lt-comp lr $< $@

###############################################################################
## Debugging transducers (for testvoc)
###############################################################################


debug: .deps/$(LANG1).LR-debug.hfst .deps/$(LANG1).lexd.hfst


.deps/$(LANG1).LR-debug.hfst: $(BASENAME).$(LANG1).lexd .deps/.d
	cat $< | grep -v 'Dir/RL' | grep -v 'Use/Circ' > .deps/$(LANG1).LR-debug.lexd
	lexd .deps/$(LANG1).LR-debug.lexd .deps/$(LANG1).LR-debug.lexd.att
	hfst-txt2fst .deps/$(LANG1).LR-debug.lexd.att -o .deps/$(LANG1).LR-debug.lexd.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).LR-debug.lexd.hfst -2 .deps/$(LANG1).twol.hfst -o $@

.deps/$(LANG1).lexd.hfst: .deps/$(LANG1).RL.lexd.hfst
	hfst-fst2fst -O -i $< -o $@
###############################################################################
## Disambiguation rules
###############################################################################

$(LANG1).rlx.bin: $(BASENAME).$(LANG1).rlx
	cg-comp $< $@

###############################################################################
## Spell checker
###############################################################################

$(LANG1).zhfst: .deps/acceptor.default.hfst .deps/errmodel.default.hfst
	rm -f $@
	zip -j $@ .deps/acceptor.default.hfst .deps/errmodel.default.hfst speller/index.xml

.deps/errmodel.default.hfst: .deps/words.default.hfst .deps/strings.default.hfst
	python dev/editdist.py -v -s -d 1 -e '@0@' -i speller/editdist.default.txt -a .deps/acceptor.default.hfst \
	| hfst-txt2fst  -e '@0@' -o .deps/editdist.default.hfst
	hfst-disjunct .deps/strings.default.hfst .deps/editdist.default.hfst \
	| hfst-minimise | hfst-repeat -f 1 -t 2 -o .deps/editstrings.default.hfst
	hfst-disjunct .deps/words.default.hfst .deps/editstrings.default.hfst \
	| hfst-fst2fst  -f olw -o $@

.deps/words.default.hfst: speller/words.default.txt
	grep -v -e "^#" -e "^$$" $< | hfst-strings2fst  -j -o $@

.deps/strings.default.hfst: speller/strings.default.txt .deps/anystar.hfst
	grep -v -e "^#" -e "^$$" $< | hfst-strings2fst  -j | hfst-concatenate .deps/anystar.hfst - |\
	hfst-concatenate - .deps/anystar.hfst -o $@

.deps/anystar.hfst:
	echo "?*;" | hfst-regexp2fst -S -o $@

.deps/acceptor.default.hfst: $(LANG1).autogen.hfst
	cat $< | hfst-fst2fst -t | hfst-project  --project=lower | hfst-minimise |hfst-fst2fst  -f olw -o $@


###############################################################################
## Unigram tagger
###############################################################################

# $(LANG1).prob: corpus/$(LANG1).tagged
#	apertium-tagger -s 0 -u 2 $@ $<

###############################################################################
## Distribution
###############################################################################

EXTRA_DIST= \
		$(BASENAME).$(LANG1).lexd	\
		$(BASENAME).$(LANG1).twol	\
		$(BASENAME).$(LANG1).rlx	\
		$(BASENAME).post-$(LANG1).dix	\
		modes.xml

###############################################################################
## Installation stuff
###############################################################################
#
#   apertium_iii_dir: This is where the compiled binaries go
#   apertium_iii_srcdir: This is where the source files go

apertium_iiidir=$(prefix)/share/apertium/$(BASENAME)/
apertium_iii_srcdir=$(prefix)/share/apertium/$(BASENAME)/

apertium_iii_DATA=$(TARGETS_COMMON) \
	$(BASENAME).$(LANG1).lexd	\
	$(BASENAME).$(LANG1).twol	\
	$(BASENAME).$(LANG1).rlx	\
	$(BASENAME).post-$(LANG1).dix

pkgconfigdir = $(prefix)/share/pkgconfig
pkgconfig_DATA = $(BASENAME).pc

noinst_DATA=modes/$(LANG1)-morph.mode

install-data-local: install-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "TODO test sentence" | apertium -d . -f line iii-morph | tee .test-mt
	@echo "TODO test analysis results" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt
