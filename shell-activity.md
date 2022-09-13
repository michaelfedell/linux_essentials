# Activity

The following is adapted from the excellent (and more lengthy) tutorial from [Software Carpentry - The Unix Shell](https://swcarpentry.github.io/shell-novice/).

## Setup

### Download the Data

```shell
$ cd

$ pwd
/Users/mfedell

$ ls
... Desktop ...

$ cd Desktop

$ curl -Lo shell-data.zip https://swcarpentry.github.io/shell-novice/data/shell-lesson-data.zip
 
$ unzip -q shell-data.zip

$ cd shell-lesson-data

$ ls -l
total 56
drwxrwxr-x 2 ubuntu ubuntu  4096 Jul 30 01:41 creatures
drwxrwxr-x 5 ubuntu ubuntu  4096 Jul 30 01:41 data
drwxrwxr-x 2 ubuntu ubuntu  4096 Jul 30 01:41 molecules
drwxrwxr-x 3 ubuntu ubuntu  4096 Jul 30 01:41 north-pacific-gyre
-rw-rw-r-- 1 ubuntu ubuntu    86 Jul 30 01:41 notes.txt
-rw-rw-r-- 1 ubuntu ubuntu    13 Aug  3 23:34 numbers.txt
-rw-rw-r-- 1 ubuntu ubuntu    32 Jul 30 01:41 pizza.cfg
-rw-rw-r-- 1 ubuntu ubuntu 21583 Jul 30 01:41 solar.pdf
drwxrwxr-x 5 ubuntu ubuntu  4096 Jul 30 01:41 writing
```

### Insepct the files

```shell
$ cd data

$ ls
amino-acids.txt  animal-counts  animals.txt  elements  morse.txt  pdb  planets.txt  salmon.txt  sunspot.txt

$ ls -F
amino-acids.txt  animal-counts/  animals.txt  elements/  morse.txt  pdb/  planets.txt  salmon.txt  sunspot.txt

$ ls -F -a
./  ../  amino-acids.txt  animal-counts/  animals.txt  elements/  morse.txt  pdb/  planets.txt  salmon.txt  sunspot.txt

$ ls -Fa
./  ../  amino-acids.txt  animal-counts/  animals.txt  elements/  morse.txt  pdb/  planets.txt  salmon.txt  sunspot.txt

$ cd ..

$ ls
creatures  data  molecules  north-pacific-gyre  notes.txt  numbers.txt  pizza.cfg  solar.pdf  writing
```

### Add some content

Create folders

```shell
$ mkdir thesis

$ ls -F
creatures/  data/  molecules/  north-pacific-gyre/  notes.txt  pizza.cfg  solar.pdf  thesis/  writing/

$ mkdir -p project/data project/results

$ ls -FR project
project:
data/  results/

project/data:

project/results:
```

Edit and inspect files

```shell
$ cd thesis

$ nano draft.txt

$ ls
draft.txt

$ cd ..

$ ls
creatures  data  molecules  north-pacific-gyre  notes.txt  numbers.txt  pizza.cfg  project  solar.pdf  thesis  writing

$ ls thesis
draft.txt

$ cat thesis.txt
It's not "publish or perish" any more,
it's "share and thrive".
```

Rename / move files

```shell
$ mv thesis/draft.txt thesis/quotes.txt

$ ls thesis
quotes.txt

$ mv thesis/quotes.txt .

$ ls
creatures  data  molecules  north-pacific-gyre  notes.txt  numbers.txt  pizza.cfg  project  quotes.txt  solar.pdf  thesis  writing

$ ls thesis

$ ls thesis/quotes.txt
ls: cannot access 'thesis/quotes.txt': No such file or directory

$ ls quotes.txt
quotes.txt
```

Copy files / folders

```shell
$ cp quotes.txt thesis/quotations.txt

$ ls quotes.txt thesis/quotations.txt
quotes.txt  thesis/quotations.txt

$ cp -r thesis thesis_backup

$ ls thesis thesis_backup
thesis:
quotations.txt

thesis_backup:
quotations.txt
```

Remove files / folders

```shell
$ rm quotes.txt

$ ls quotes.txt
ls: cannot access 'quotes.txt': No such file or directory

$ rm thesis
rm: cannot remove 'thesis': Is a directory

$ rm -r thesis

$ ls thesis
ls: cannot access 'thesis': No such file or directory

$ ls -F
creatures/  molecules/           notes.txt    pizza.cfg  solar.pdf       writing/
data/       north-pacific-gyre/  numbers.txt  project/   thesis_backup/
```

More commands to inspect files

```shell
$ ls molecules
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb

$ cd molecules

$ wc cubane.pdb
  20  156 1158 cubane.pdb

$ wc --help
Usage: wc [OPTION]... [FILE]...
  or:  wc [OPTION]... --files0-from=F
Print newline, word, and byte counts for each FILE, and a total line if
more than one FILE is specified.  A word is a non-zero-length sequence of
characters delimited by white space.
...
```

Redirect output to another file

```shell
$ wc -l *.pdb
  20 cubane.pdb
  12 ethane.pdb
   9 methane.pdb
  30 octane.pdb
  21 pentane.pdb
  15 propane.pdb
 107 total

$ wc -l *.pdb > lengths.txt

$ ls lengths.txt
lengths.txt

$ cat lengths.txt
  20 cubane.pdb
  12 ethane.pdb
   9 methane.pdb
  30 octane.pdb
  21 pentane.pdb
  15 propane.pdb
 107 total
```

Redirect output to another command

```shell
$ sort -n lengths.txt
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total

$ sort -n lengths.txt > sorted-lengths.txt

$ head -n 1 sorted-lengths.txt
   9 methane.pdb

$ sort -n lengths.txt | head -n 3
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
```

More inspecting and piping

```shell
$ cd ~/Desktop/shell-lesson-data/data

$ ls
amino-acids.txt  animal-counts  animals.txt  elements  morse.txt  pdb  planets.txt  salmon.txt  sunspot.txt

$ cat animals.txt
2012-11-05,deer
2012-11-05,rabbit
2012-11-05,raccoon
2012-11-06,rabbit
2012-11-06,deer
2012-11-06,fox
2012-11-07,rabbit
2012-11-07,bear
```

Use `cut` to select fields in a delimited file

```shell
$ cut -d , -f 2 animals.txt
deer
rabbit
raccoon
rabbit
deer
fox
rabbit
bear

$ cut -d , -f 2 animals.txt | sort
bear
deer
deer
fox
rabbit
rabbit
rabbit
raccoon
```

Use `sort` and `uniq` to get the number of unique animals

```shell
$ cut -d , -f 2 animals.txt | sort | uniq -c
      1 bear
      2 deer
      1 fox
      3 rabbit
      1 raccoon

$ cut -d , -f 2 animals.txt | sort | uniq -c | wc -l
5
```

Find content in a file

```shell
$ cd data

$ head -5 sunspot.txt
(* Sunspot data collected by Robin McQuinn from *)
(* http://sidc.oma.be/html/sunspot.html         *)

(* Month: 1749 01 *) 58
(* Month: 1749 02 *) 63

$ tail +4 sunspot.txt | head -3
(* Month: 1749 01 *) 58
(* Month: 1749 02 *) 63
(* Month: 1749 03 *) 70
```

```shell
$ tail +4 sunspot.txt | cut -d " " -f 3 | sort -n | uniq -c | tail -3
     12 2003
     12 2004
      2 2005

$ grep 2005 sunspot.txt
(* Month: 2005 01 *) 31
(* Month: 2005 02 *) 29

$ grep -n 2005 sunspot.txt
3079:(* Month: 2005 01 *) 31
3080:(* Month: 2005 02 *) 29

$ grep 2005 sunspot.txt -B 2
(* Month: 2004 11 *) 44
(* Month: 2004 12 *) 18
(* Month: 2005 01 *) 31
(* Month: 2005 02 *) 29
```
