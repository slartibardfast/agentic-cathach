# Preamble draft (working)

This is a working draft of the cathach preamble: the readable opening that teaches a
reader to decode the book by hand, before any machine. It is the sparsest and most
protected part of the book, and the one part that must survive with nothing else.

The final preamble is trilingual, so the same meaning is carried in three languages
chosen by literate-speaker population: English, Mandarin, and Spanish (see
[call/0007](../../call/0007-preamble-languages.md)). This draft is the English text plus
the shared figures, which are language-independent and do the real teaching. The figures
are the spine; the prose only points at them.

**Assumed-knowledge floor.** The floor is fixed to tally strokes (see
[call/0008](../../call/0008-assumed-knowledge-floor.md)): a reader is presumed only to see
that marks carry meaning and to count marks one by one. Everything above that is taught,
from strokes up. It is the lowest floor above counting itself.

## Order of teaching

1. What this book is, and that it can be read back.
2. How to read the marks: orientation, order, and the two registers.
3. Counting, and the ten digits.
4. Places, by grouping.
5. The rows: data digits and two checks, S and W.
6. Repair: one lost digit, and one lost page.
7. The pages, with their headers and the table of contents.
8. Integrity: confirming the whole book.
9. The machine, and the printed program that rebuilds it.
10. What you can now do.

Sections 3 to 6 are the core: with them a patient reader recovers any single page of the
readable data tier by hand. The rest is orientation and navigation, with a final
confirmation of the whole.

## 1. What this book is

This book holds information. Every page can be turned back into the exact marks it was
made from, so nothing is lost. You do not need a machine to begin: the next pages teach
you to read the information by hand, using only counting, addition, and subtraction. A
machine reads it faster, and a printed program later in the book rebuilds that machine,
but the hand method never depends on it.

## 2. How to read the marks

The marks are read in an order the figures fix: along a line in one direction, then the
next line below it, and the pages in the order of their numbers. Every page carries the
same small marks in its corners; they fix which way is up and where a page begins, for a
reader and for a machine alike.

There are two kinds of mark. The first is writing like this, meant for a person to read.
The second is dense fields of small squares, meant for a machine, or for a very patient
person following these instructions. The writing teaches; the fields carry the bulk. This
preamble is all writing.

## 3. Counting, and the ten digits

We write counts with ten marks. Each mark stands for a count, shown here beside a row of
strokes so the meaning is fixed without words:

```host-lint:ignore
   0        (no strokes)
   1        |
   2        ||
   3        |||
   4        ||||
   5        |||||
   6        ||||| |
   7        ||||| ||
   8        ||||| |||
   9        ||||| ||||
```

Read the ten marks in that order. After nine, we do not invent a new mark; we use places.

## 4. Places, by grouping

Counting many strokes one at a time is slow and easy to lose, so we group. Tie the strokes
into bundles of ten. The count is then some bundles of ten with some ones left over. Write
the number of ones with a digit on the right, and the number of ten-bundles with a digit to
its left. If the ten-bundles are themselves many, bundle those into tens as well, and write
that count one place further to the left. Each place counts ten times the place to its right:

```host-lint:ignore
   |||||||||| |||||||||| |||   =   2 bundles of ten and 3 ones   ->   written   2 3

   2 0 3   =   2 bundles-of-ten-bundles,  0 ten-bundles,  3 ones   =   two hundred and three
```

This is all the number-reading the preamble needs.

## 5. The rows: data and two checks

The recoverable data is written in short rows. Each row is one group. These rows use base
six: the six digits 0 1 2 3 4 5, then five data digits, then the two checks called S and W.
A small base keeps the digits far apart, so damage is easier to spot and to mend. The two
checks are computed from the five data digits.

To keep a sum to a single digit, we cast out sixes: remove complete groups of six and keep
only what is left.

```host-lint:ignore
   place    1   2   3   4   5
   data     3   5   1   4   2

   S = the sum of the data, then cast out sixes:
       3 + 5 + 1 + 4 + 2  =  15  =  two sixes and 3   ->   S = 3
   W = the sum of (place times digit), then cast out sixes:
       1*3 + 2*5 + 3*1 + 4*4 + 5*2  =  42  =  seven sixes and 0   ->   W = 0
```

S is a plain sum; it mends a missing digit. W weights each digit by its place; it catches a
digit that was changed for another. Neither uses division, so both stay inside a budget of
addition and multiplication.

## 6. Repair: one lost digit, and one lost page

Suppose damage destroys one digit, but you can still see which place is now empty. That is
the easy case: the place is known, only the value is gone. Add the data digits you can still
read, cast out sixes, and subtract from S:

```host-lint:ignore
   damaged  3   5   ?   4   2      with  S = 3

   known sum = 3 + 5 + 4 + 2  =  14  =  two sixes and 2   ->   2
   lost digit = S - 2 = 3 - 2 = 1

   the row was  3 5 1 4 2  again — recovered exactly, by subtraction alone.
```

If two digits in a row are lost at once, or a digit was changed with no empty place to mark
it, the hand method stops and the machine tier takes over. After a repair, recompute W to
confirm the row is whole.

A whole lost page is mended the same way, one place up. A repair page stands for a group of
pages and holds, in each place, the sum of that place across the group. To rebuild a missing
page, take the repair page and subtract the pages you still have, place by place. So a smudge
is mended within a page, and a lost page is mended across the group.

## 7. The pages, their headers, and the table of contents

- Each dense page has a short readable header. It gives the page's number in the whole book
  (this page, of the total), and, for a file split across pages, which piece it is and where
  that piece begins. A reader uses the headers to put pages back in order.
- The readable **table of contents** lists every file: its name, the pages that hold it, its
  length, and a check value to confirm it. To recover a file, find it here, gather its pages
  in order by their headers, and read them.
- A file smaller than one page's worth sits on a single page. A larger file spans several
  pages in sequence; nothing about it is special except that there are more pages.

## 8. Integrity: confirming the whole book

Two checks confirm that nothing has changed. A person adds a running sum over the book, given
in the table of contents, and compares it; this needs only addition. A machine compares a
longer check value, printed as digits beside the running sum. A rebuilt program confirms it in
a moment, though it is too long for a person to add by hand. If either check matches, the book
is whole.

## 9. The machine and its printed program

The dense fields are read quickly by a small program. Its complete source is printed later in
this book, in two computer languages, so a reader who can build a computer can type it back in
and run it. The hand method taught above is enough to recover that program if everything else
is gone, because the program too is stored as recoverable rows. The program reads the dense
fields; the fields hold the same information as the rows, packed smaller.

## 10. What you can now do

With the counting and the two checks, you recover any single page of the readable tier by
hand. With the headers and the table of contents, you find any file and confirm the book is
whole. With the printed program, you rebuild the machine that reads the dense pages at speed.
Nothing in the book depends on anything outside the book, so a reader who has the book and a
pen can begin, with patience.

## Open, for this draft

- The three-column trilingual layout (the languages are locked to English, Mandarin, and
  Spanish; see [call/0007](../../call/0007-preamble-languages.md)).
- The figures: the strokes-to-digits chart and the worked rows become the real, drawn figures
  once the glyph alphabet is fixed.
