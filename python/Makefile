
all: build
#
#
build: copy libinjection/libinjection_wrap.c
	rm -f libinjection.py libinjection.pyc
	python setup.py --verbose build --force

install: build
	sudo python setup.py --verbose install

test-unit: build words.py
	python setup.py build_ext --inplace
	PYTHON_PATH='.' nosetests -v --with-xunit test_driver.py

.PHONY: test
test: test-unit

.PHONY: speed
speed:
	./speedtest.py


words.py: Makefile json2python.py ../c/sqlparse_data.json
	./json2python.py < ../c/sqlparse_data.json > words.py

libinjection/libinjection.h: ../c/libinjection.h
	cp ../c/libinjection.h libinjection/libinjection.h

libinjection/libinjection_sqli.c: ../c/libinjection_sqli.c
	cp ../c/libinjection_sqli.c libinjection/libinjection_sqli.c

libinjection/libinjection_sqli_data.h: ../c/libinjection_sqli_data.h
	cp ../c/libinjection_sqli_data.h libinjection/libinjection_sqli_data.h

libinjection/libinjection_wrap.c: libinjection/libinjection.i libinjection/libinjection.h
	swig -version
	swig -python -builtin -Wall -Wextra libinjection/libinjection.i

.PHONY:copy
copy: libinjection/libinjection.h libinjection/libinjection_sqli.c libinjection/libinjection_sqli_data.h

clean:
	@rm -rf build dist
	@rm -f *.pyc *~ _libinjection.so
	@rm -f nosetests.xml
	@rm -f words.py
	@rm -f libinjection/*~ libinjection/*.pyc
	@rm -f libinjection/libinjection.h libinjection/libinjection_sqli.c libinjection/libinjection_sqli_data.h
	@rm -f libinjection/libinjection_wrap.c libinjection/libinjection.py
