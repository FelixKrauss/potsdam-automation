# Automation and make

## Olav Vahtras

### Software Carpentry Workshop Potsdam 2015-09-24


---

layout: false

## Initialize

- Download example files
```bash
$ wget http://swcarpentry.github.io/make-novice/make-lesson.tar.gz
```
- unpack the compressed archive
```bash
$ tar xvfz make-lesson.tar.gz
```
- step into the new directory
```bash
$ cd make-lesson
```

---

## Initial examination

- Files in `make-lesson`

<PRE>
    make-lesson
    ├── books
    │   ├── abyss.txt
    │   ├── isles.txt
    │   ├── last.txt
    │   ├── LICENSE_TEXTS.md
    │   └── sierra.txt
    ├── plotcount.py
    └── wordcount.py
</PRE>

- Use the `wordcount.py` script to generate words statistics

```
$ python wordcount.py books/abyss.txt abyss.dat
```

--
- What about many files?

--
- How can we automate analysis so that only change files are analyzed?

--
- `make`
	

---

### The Makefile
```
# Count words.
.PHONY : dats
dats : isles.dat abyss.dat

isles.dat : books/isles.txt
        python wordcount.py books/isles.txt isles.dat

abyss.dat : books/abyss.txt
        python wordcount.py books/abyss.txt abyss.dat

.PHONY : clean
clean :
        rm -f *.dat
```

### Exercise

1. Update `Makefile` to handle `last.txt`
2. Create a new rule that creates `analysis.tar.gz` from the `dat` files
```
tar cvfz analysis.tar.gz isles.dat abyss.dat last.dat
```
3. Update clean

---

### Automatic variables

The general form of a make rule is

```
target: dependencies
<tab> action
```

In the action part automatic variables can be used

* `$@` for target
* `$^` for all dependencies
* `$<` for the first dependency

In the dependencies

* Unix wildcard `*` can be used

### Exercise

1. Rewrite the .dat rules using automatic variables
2. Add the script to the dependencies

---

## More generalizations

### Makefile wildcards

* The `dat` rules follow a pattern
* The Makefile wildcard character `%` can be used to summarize all three rules

```
%.dat: books/%.txt wordcount.py
        python wordcount.py $< $@
```

### Variables

* Variables can be assigned
```
SCRIPT=wordcount.py
```
* and are referenced in the Makefile with e.g.
```
python $(SCRIPT)
```

---

## Functions

Built in functions in make


* `wildcard`: list of files mathing a pattern

* `patsubst`: pattern substitution

* `include`: include another Makefile source into the current Makefile

---

### Final version

```
include config.mk

TXT_FILES=$(wildcard books/*.txt)
DAT_FILES=$(patsubst books/%.txt, %.dat, $(TXT_FILES))

.PHONY: variables
variables:
    @echo TXT_FILES: $(TXT_FILES)
    @echo DAT_FILES: $(DAT_FILES)

# Count words.
.PHONY : dats
dats : $(DAT_FILES)

%.dat : books/%.txt $(COUNT_SRC)
    $(COUNT_EXE) $< $*.dat

# Generate archive file.
analysis.tar.gz : $(DAT_FILES) $(COUNT_SRC)
    tar -czf $@ $^

.PHONY : clean
clean :
    rm -f $(DAT_FILES)
    rm -f analysis.tar.gz

```
```
#config.mk
COUNT_SRC=wordcount.py
COUNT_EXE=python $(COUNT_SRC)
```

---

### Exercise

* Complement the Makefile with arule for generating `.jpg` from `.dat` with `plotcount.py`
* Include `.jpg` files in the archive and remove them for `make clean`
