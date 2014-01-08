COMPONENTS = marfin_cv # marfin_publications  marfin_research_software_projects
LANGUAGES = en.2


dorst2pdf=yes

RST2HTML = rst2html.py
RST2HTML_OPTS = --strict --tab-width=2 --initial-header-level=2 --link-stylesheet
RST2HTML_OPTS := $(RST2HTML_OPTS) --cloak-email-addresses

SUFFIX = html



.PHONY: all
all: $(addsuffix .$(SUFFIX), $(foreach l,$(LANGUAGES),$(addsuffix .$(l),$(COMPONENTS))))

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
#	$(MAKE) cv_igor_marfin.zip

#$(COMPONENTS).en.%: RST2HTML_OPTS := $(RST2HTML_OPTS) --stylesheet=cv.css --language=en
#$(COMPONENTS).en.%: RST2HTML_OPTS := $(RST2HTML_OPTS) --stylesheet=cv.css.1 --language=en
%.en.%: RST2HTML_OPTS := $(RST2HTML_OPTS)  --language=en

%.$(SUFFIX): %.rst
	$(RST2HTML) $(RST2HTML_OPTS) $< > $@
#	sed -i -e 's,text/html,application/xhtml+xml,' \
#				 -e 's,XHTML 1\.0 Transitional,XHTML 1.0 Strict,' \
#				 -e 's,xhtml1-transitional,xhtml1-strict,' \
#				 -e 's,<title>Curriculum Vitae</title>,<title>CV Igor Marfin</title>,' \
#				 -e 's,<title>Technical Profile</title>,<title>Tech Profile Igor Marfin</title>,' \
#				 -e 's,<title>List of References</title>,<title>References Igor Marfin</title>,' $@

%.pdf: %.rst
ifdef dorst2pdf
	rst2pdf -s cv.pdfstyle $<
else
	rst2latex.py tmp.rst > tmp.tex
	pdflatex tmp.tex
	mv tmp.pdf $@
	rm tmp*
endif


.PHONY: clean
clean:
	rm -f $(wildcard *.$(SUFFIX))
	rm -f $(wildcard *.zip)
	rm -f $(wildcard *.pdf)
	find . -type l -exec rm {} \;
