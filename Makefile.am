AUTOMAKE_OPTIONS = foreign
CLOCAL_AMFLAGS = -I m4/

SUBDIRS = man

TESTS = test/ed-as-sensible-editor test/echo-as-browser
AM_TESTS_ENVIRONMENT=\
    PATH="$(topsrcdir):$$PATH"; export PATH;

binpkgdatadir = $(pkgdatadir)/bin

edit = sed \
             -e 's|@binpkgdatadir[@]|$(binpkgdatadir)|g' \
             -e 's|@prefix[@]|$(prefix)|g'


select-editor: Makefile
	rm -f $@ $@.tmp
	$(edit) '$(srcdir)/$@.in' >$@.tmp
	chmod +x $@.tmp
	chmod a-w $@.tmp
	mv $@.tmp $@

select-editor: $(srcdir)/select-editor.in


bin_SCRIPTS = sensible-browser sensible-editor sensible-pager select-editor
binpkgdata_SCRIPTS = tools/gettext
