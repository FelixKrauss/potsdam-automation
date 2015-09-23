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
	

