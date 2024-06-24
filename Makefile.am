AUTOMAKE_OPTIONS = foreign
CLOCAL_AMFLAGS = -I m4/

SUBDIRS = man

TESTS = test/ed-as-sensible-editor test/echo-as-browser test/echo-as-terminal test/ed-as-visual test/ed-as-fallback test/error-out test/fork-bomb test/shell-script test/failure-propagation test/echo-as-pager test/empty test/whitespace-preserved test/shellcheck
AM_TESTS_ENVIRONMENT=\
    PATH="$(top_srcdir):$$PATH"; export PATH;

binpkgdatadir = $(pkgdatadir)/bin

bin_SCRIPTS = sensible-browser sensible-editor sensible-pager select-editor sensible-terminal
