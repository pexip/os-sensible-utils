# part of this makefile are copyied from dpkg
AUTOMAKE_OPTIONS = foreign

man_MANS = sensible-editor.1 sensible-browser.1 sensible-pager.1 select-editor.1
EXTRA_DIST = sensible-editor.man sensible-browser.man sensible-pager.man select-editor.man

# Extract the list of languages from the po4a config file.
LINGUAS_DIST = `sed -ne 's/^.*\[po4a_langs\] \(.*\)$$/\1/p' $(srcdir)/po4a/po4a.cfg`

# If the user has not defined it let's use the default.
LINGUAS ?= $(LINGUAS_DIST)


update-po:
	$(PO4A_V) $(PO4A) $(PO4A_OPTS) --force $(srcdir)/po4a/po4a.cfg

PO4A_V = $(PO4A_V_@AM_V@)
PO4A_V_ = $(PO4A_V_@AM_DEFAULT_V@)
PO4A_V_0 = @echo "  PO4A   $@";

PO4A_OPTS = --previous --srcdir $(srcdir) --destdir $(builddir) --no-backups \
            --porefs noline,wrap --msgmerge-opt=--add-location=file \
            --package-name $(PACKAGE_NAME) #--package-version $(PACKAGE_VERSION) #\
            #--copyright-holder "$(PACKAGE_COPYRIGHT_HOLDER)"# \
	    #--msgid-bugs-address $(PACKAGE_BUGREPORT)

all-local: gen-man

clean-local: clean-local-yes

clean-local-no:
clean-local-yes:
	rm -rf $(LINGUAS_DIST)
	$(AM_V_at) rm -f man.stamp

CLEANFILES = $(man_MANS) messages.mo man.stamp

# We use the man-once target to guarantee there is never an empty call in
# gen-man, that starts recursing infinitely.
.PHONY: gen-man man-once

gen-man: man.stamp
	for lang in $(LINGUAS); do \
	  files=""; \
	  for man in $(man_MANS); do \
            trans="$${man%%.[0-9]}.man"; \
	    if [ -f $(builddir)/$$lang/$$trans ]; then \
	      files="$$files $(builddir)/$$lang/$$man"; \
	    elif [ -f $(srcdir)/$$lang/$$trans ]; then \
	      files="$$files $(srcdir)/$$lang/$$man"; \
	    fi; \
	  done; \
	  $(MAKE) mandir="$(mandir)/$$lang" man_MANS="" man-once $$files; \
	done

# FIXME: Use a stamp file until po4a supports them internally.
man.stamp:
	$(PO4A_V) $(PO4A) $(PO4A_OPTS) $(srcdir)/po4a/po4a.cfg
	$(AM_V_at) touch $@

SUFFIXES = .man

MANGEN_V = $(MANGEN_V_@AM_V@)
MANGEN_V_ = $(MANGEN_V_@AM_DEFAULT_V@)
MANGEN_V_0 = @echo "  MANGEN $@";

MANGEN = $(MANGEN_V) sed \
	-f $(srcdir)/utf8toman.sed \
	-e 's/%PACKAGE%/$(PACKAGE)/g' \
	-e 's/%VERSION%/$(PACKAGE_VERSION)/g' \
	-e 's/%RELEASE_DATE%/$(PACKAGE_RELEASE_DATE)/g' \
	-e 's:%CONFDIR%:$(sysconfdir):g' \
	-e 's:%LOGDIR%:$(logdir):g' \
	-e 's:%ADMINDIR%:$(admindir):g' \
	-e 's:%PKGDOCDIR%:$(docdir):g' \
	-e 's:%PKGCONFDIR%:$(pkgconfdir):g' \
	-e 's:%PKGLIBDIR%:$(pkglibexecdir):g' \
	-e 's:%PKGDATADIR%:$(pkgdatadir):g'
	$(nil)

.man.1: Makefile
	$(MANGEN) $< >$@
.man.5: Makefile
	$(MANGEN) $< >$@
.man.8: Makefile
	$(MANGEN) $< >$@

install-data-local: install-data-local-yes

install-data-local-no:
install-data-local-yes:
	for lang in $(LINGUAS); do \
	  files=""; \
	  for man in $(man_MANS); do \
            trans="$${man%%.[0-9]}.man"; \
	    if [ -f $(builddir)/$$lang/$$trans ]; then \
	      files="$$files $(builddir)/$$lang/$$man"; \
	    elif [ -f $(srcdir)/$$lang/$$trans ]; then \
	      files="$$files $(srcdir)/$$lang/$$man"; \
	    fi; \
	  done; \
	  $(MAKE) install-man mandir="$(mandir)/$$lang" man_MANS="$$files"; \
	done

uninstall-local: uninstall-local-yes

uninstall-local-no:
uninstall-local-yes:
	for lang in $(LINGUAS); do \
	  files=""; \
	  for man in $(man_MANS); do \
	    trans="$${man%%.[0-9]}.man"; \
	    if [ -f $(builddir)/$$lang/$$trans ]; then \
	      files="$$files $(builddir)/$$lang/$$man"; \
	    elif [ -f $(srcdir)/$$lang/$$trans ]; then \
	      files="$$files $(srcdir)/$$lang/$$man"; \
	    fi; \
	  done; \
	  $(MAKE) uninstall-man mandir="$(mandir)/$$lang" man_MANS="$$files"; \
	done

#dist-hook: man.stamp
#	cp $(srcdir)/man.stamp $(distdir)/
#	for lang in $(LINGUAS_DIST); do \
#	  cp $(srcdir)/po/$$lang.po $(distdir)/po; \
#	  cp $(srcdir)/po/$$lang.add $(distdir)/po; \
#	  $(MKDIR_P) $(distdir)/$$lang; \
#	  cp -R $(srcdir)/$$lang $(distdir)/; \
#	done
