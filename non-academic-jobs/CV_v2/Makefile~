
#COMPONENTS = cv tech-profile refs
COMPONENTS = cv
LANGUAGES = en

RST2HTML = rst2html.py
RST2HTML_OPTS = --tab-width=2 --initial-header-level=2 --link-stylesheet
RST2HTML_OPTS := $(RST2HTML_OPTS) --cloak-email-addresses

SUFFIX = html

.PHONY: all
#all: $(addsuffix .$(SUFFIX), $(foreach l,$(LANGUAGES),$(addsuffix .$(l),$(COMPONENTS))))
#	for l in $(LANGUAGES); do \
#		for c in $(COMPONENTS); do \
#			ln -sf $$c.$$l.$(SUFFIX) $$c.$(SUFFIX).$$l; \
#		done; \
#	done
#	chmod -R u=rwX,og=rX .
#	find . -type d -exec chmod g+s {} \;
#	find . -type d -name .svn -exec chmod og= {} \;
#	chown -R --reference=. .
#	chgrp -R --reference=. .
#	$(MAKE) cv_martin-f-krafft.zip

#cv_martin-f-krafft.zip: DIR = $(patsubst %.zip,%,$@)
#cv_martin-f-krafft.zip: $(wildcard *.$(SUFFIX) *.css *.png *.pdf)
#	mkdir --parent $(DIR)
#	ln -f $^ $(DIR)
#	zip -r $@ $(DIR)
#	rm -r $(DIR)
#	chmod 644 $@

.PHONY: pdfs
pdfs: $(patsubst %.ps,%.pdf,$(wildcard *.ps))

.PHONY: refs
refs: $(patsubst %.rst,%.$(SUFFIX),$(wildcard ref-*.rst))

%.pdf: %.ps
	ps2pdf -dEmbedAllFonts=true -dCompressPages=true -dUseFlatCompression=true \
		-dAutoFilterColorImages=false -sColorImageFilter=FlateEncode $< $@

cv.en.% refs.en.%: RST2HTML_OPTS := $(RST2HTML_OPTS) --stylesheet=cv.css --language=en
tech-profile.en.%: RST2HTML_OPTS := $(RST2HTML_OPTS) --stylesheet=tech-profile.css --language=en
refs.en.%: RST2HTML_OPTS := $(RST2HTML_OPTS) --stylesheet=refs.css --language=en
ref-%: RST2HTML_OPTS := $(RST2HTML_OPTS) --stylesheet=ref.css --language=en

%.$(SUFFIX): %.rst
	$(RST2HTML) $(RST2HTML_OPTS) $< > $@
	sed -i -e 's,text/html,application/xhtml+xml,' \
				 -e 's,XHTML 1\.0 Transitional,XHTML 1.0 Strict,' \
				 -e 's,xhtml1-transitional,xhtml1-strict,' \
				 -e 's,<title>Curriculum Vitae</title>,<title>CV Martin F. Krafft</title>,' \
				 -e 's,<title>Technical Profile</title>,<title>Tech Profile Martin F. Krafft</title>,' \
				 -e 's,<title>List of References</title>,<title>References Martin F. Krafft</title>,' $@

.PHONY: push
push: DEST_SERVER = lotus
push: DEST_DIR = /srv/apache2/martin-krafft.net/htdocs/cv
push: pdfs
	git push
	ssh $(DEST_SERVER) make -C $(DEST_DIR) update

.PHONY: clean
clean:
	rm -f $(wildcard *.$(SUFFIX))
	rm -f $(wildcard *.zip)
	find . -type l -exec rm {} \;

.PHONY: update
update:
	git pull
	$(MAKE)
