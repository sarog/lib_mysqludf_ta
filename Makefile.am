TESTS=test/02_load_data.sh test/03_test.sh
ACLOCAL_AMFLAGS=-I config
lib_LTLIBRARIES = lib_mysqludf_ta.la
AUTOMAKE_OPTIONS = subdir-objects
lib_mysqludf_ta_la_SOURCES = src/ta_libmysqludf_ta.h \
                src/ta_ema.c \
				src/ta_max.c \
				src/ta_min.c \
				src/ta_previous.c \
				src/ta_rsi.c \
				src/ta_sma.c \
				src/ta_stddevp.c \
				src/ta_sum.c \
				src/ta_tr.c
lib_mysqludf_ta_la_CFLAGS = -Wall -c @MYSQL_CFLAGS@
lib_mysqludf_ta_la_LDFLAGS = -shared

EXTRA_DIST = *.h test *.sql API

.PHONY : test

mrproper:
	make clean
	make test-clean
	make maintainer-clean
	rm -rf config.guess config.h.* config.status configure missing config.sub ltmain.sh depcomp aclocal.m4 install.sh config.log compile Makefile.in doc/html *.loT mkinstalldirs install-sh *~

installdb: 
	$(MYSQL) < ./installdb.sql

uninstalldb: 
	$(MYSQL) < ./uninstalldb.sql
	
dist-hook:
	find $(distdir) -name .svn -o -name .DS_Store -o -name .cdtproject -o -name .project -o -name .settings -o *~ | xargs --no-run-if-empty rm -rf

test: 
	for i in `ls test/*.result | awk -F "." '{print $$1}'`; do echo "$$i"; $(MYSQLTEST) --result-file=$$i.result < $$i.test; done

test-results: 
	for i in `ls test/*.test | awk -F "." '{print $$1}'`; do echo "$$i"; $(MYSQLTEST) --result-file=$$i.result --record < $$i.test; done

test-clean:
	rm -f test/*.log test/*.reject

