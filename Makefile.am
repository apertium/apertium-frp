###############################################################################
## Makefile for apertium-frp
###############################################################################

LANG1=frp
BASENAME=apertium-$(LANG1)

TARGETS_COMMON =			\
	$(LANG1).automorf.bin		\
	$(LANG1).automorf.att.gz	\
	$(LANG1).autogen.bin		\
	$(LANG1).autogen.att.gz		\
	$(LANG1).autopgen.bin		\
	$(LANG1).rlx.bin
#	$(LANG1).prob                   

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## Arpitan transducer
###############################################################################

$(LANG1).xsl: $(BASENAME).$(LANG1).metadix buscaPar.xsl
	xsltproc buscaPar.xsl $< |uniq >$@.tmp
	mv $@.tmp $@

.deps/$(LANG1).dix: $(LANG1).xsl $(BASENAME).$(LANG1).metadix .deps/.d
	xsltproc $< $(BASENAME).$(LANG1).metadix > $@.tmp
	mv $@.tmp $@

$(LANG1).autogen.bin: .deps/$(LANG1).dix $(BASENAME).$(LANG1).acx
	apertium-validate-dictionary $<
	lt-comp rl $< $@ $(BASENAME).$(LANG1).acx

$(LANG1).automorf.bin: .deps/$(LANG1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@ $(BASENAME).$(LANG1).acx

$(LANG1).autogen.att.gz: $(LANG1).autogen.bin
	lt-print $< | gzip -9 -c -n > $@

$(LANG1).automorf.att.gz: $(LANG1).automorf.bin
	lt-print $< | gzip -9 -c -n > $@

$(LANG1).autopgen.bin: $(BASENAME).post-$(LANG1).dix
	lt-comp lr $< $@

###############################################################################
## Disambiguation rules
###############################################################################

$(LANG1).rlx.bin: $(BASENAME).$(LANG1).rlx $(CGCOMP)
	$(CGCOMP) $< $@

###############################################################################
## Unigram tagger
###############################################################################

#$(LANG1).prob: corpus/$(LANG1).tagged
#	apertium-tagger -s 0 -u 2 $@ $<

###############################################################################
## Distribution
###############################################################################
EXTRA_DIST=$(BASENAME).$(LANG1).metadix		\
		$(BASENAME).$(LANG1).acx	\
		$(BASENAME).$(LANG1).rlx	\
		$(BASENAME).post-$(LANG1).dix	\
		$(LANG1).prob			\
		modes.xml \
		alt.xsl \
		buscaPar.xsl \
		principal.xsl

###############################################################################
## Installation stuff
###############################################################################
#
#   apertium_frp_dir: This is where the compiled binaries go
#   apertium_frp_srcdir: This is where the source files go

apertium_frpdir=$(prefix)/share/apertium/$(BASENAME)/
apertium_frp_srcdir=$(prefix)/share/apertium/$(BASENAME)/

apertium_frp_DATA=$(TARGETS_COMMON)

pkgconfigdir = $(prefix)/share/pkgconfig
pkgconfig_DATA = $(BASENAME).pc

# Modes are not installed since they're all just "debug modes". All
# modes are created by this goal.
modes/$(LANG1)-morph.mode: modes.xml
	apertium-validate-modes modes.xml
	apertium-gen-modes modes.xml

noinst_DATA=modes/$(LANG1)-morph.mode

install-data-local: install-modes
	test -d $(DESTDIR)$(apertium_frp_srcdir) || mkdir -p $(DESTDIR)$(apertium_frp_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).rlx $(DESTDIR)$(apertium_frp_srcdir)
	$(INSTALL_DATA) $(BASENAME).post-$(LANG1).dix $(DESTDIR)$(apertium_frp_srcdir)
#	$(INSTALL_DATA) $(BASENAME).$(LANG1).dix $(DESTDIR)$(apertium_frp_srcdir)

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "TODO test sentence" | apertium -d . frp-morph | tee .test-mt
	@echo "TODO test analysis results" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt
