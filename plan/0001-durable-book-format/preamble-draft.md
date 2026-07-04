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

**Tightness.** Every step is small and built from the step before it, so the distance between
counting and the full system is crossed without a leap. The teaching stays in base ten, the
base a reader learns first, and only at the end notes that the dense rows use a smaller base
by the same method. Each guard against damage is motivated before it is introduced.

## Order of teaching

1. What this book is.
2. How to read the marks.
3. Counting, and the ten digits.
4. Places, by grouping.
5. Rows, and what can go wrong.
6. The first guard: the sum, and repair by subtraction.
7. The second guard: the weighted sum.
8. Mending a whole page.
9. The smaller base.
10. A guard, worked in full.
11. The pages, with their headers and the table of contents.
12. Integrity: confirming the whole book.
13. The machine, and its printed program.
14. What you can now do.

## 1. What this book is

This book holds information. Every page can be turned back into the exact marks it was made
from, so nothing is lost. You do not need a machine to begin: the pages that follow teach you
to read the information by hand, using only counting, addition, and subtraction. A machine
reads it faster, and a printed program later in the book rebuilds that machine, but the hand
method never depends on it.

## 2. How to read the marks

The marks are read in an order the figures fix: along a line in one direction, then the next
line below it, and the pages in the order of their numbers. Every page carries the same small
marks in its corners; they fix which way is up and where a page begins, for a reader and for a
machine alike.

There are two kinds of mark. The first is writing like this, meant for a person to read. The
second is dense fields of small squares, meant for a machine, or for a very patient person
following these instructions. The writing teaches; the fields carry the bulk. This preamble is
all writing.

## 3. Counting, and the ten digits

We write counts with ten marks. Each mark stands for a count, shown here beside that many
strokes, so the meaning is fixed without words. Zero is the empty row, before one:

```host-lint:ignore
   0
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
into bundles of ten. The count is then some bundles of ten with some ones left over. Write the
number of ones with a digit on the right, and the number of ten-bundles with a digit to its
left. If the ten-bundles are themselves many, bundle those into tens as well, and write that
count one place further to the left. Each place counts ten times the place to its right:

```host-lint:ignore
   |||||||||| |||||||||| |||   =   2 bundles of ten and 3 ones   ->   2 3

   2 0 3   =   2 bundles-of-ten-bundles,  0 ten-bundles,  3 ones   =   two hundred and three
```

Two habits from this section carry the rest: reading a place from its position, and taking the
ones left over after complete bundles are set aside. Both come back at once.

## 5. Rows, and what can go wrong

The information in this book sits in short rows of digits, like the numbers you have just
learned to read. Paper is fragile. A mark can be smudged until its place is blank, or it can be
changed so that one digit looks like another. To catch such damage, and to mend it, we add a
little to each row, computed from the row itself, with nothing more than counting, adding, and
the grouping of the last section.

## 6. The first guard: the sum

Add all the data digits in a row. The total may be large, but we keep only its ones left over:
set aside complete tens, exactly as you bundled strokes into tens, and keep what remains. That
single digit is the first guard. Call it S, and write it just after the data.

```host-lint:ignore
   place    1   2   3   4   5
   data     3   7   1   9   4

   sum  =  3 + 7 + 1 + 9 + 4  =  24  =  two tens and 4   ->   S = 4
```

S mends a smudged digit. If one digit is later lost, but you can see which place has gone
blank, add the digits you can still read, keep the ones left over, and subtract from S. The
missing digit reappears.

```host-lint:ignore
   damaged  3   7   ?   9   4      with  S = 4

   digits you can read = 3 + 7 + 9 + 4  =  23  =  two tens and 3   ->   3
   lost digit  =  S - 3  =  4 - 3  =  1

   the row was  3 7 1 9 4  again — mended exactly, by subtraction alone.
```

This is repair by subtraction, and it asks no more than you already know.

## 7. The second guard: the weighted sum

The sum alone cannot notice a digit swapped for another that leaves the total unchanged. So we
add a second guard. Number the places from the left: one, two, three, and on. Multiply each
data digit by its place, add the results, and keep the ones left over as before. That digit is
the second guard. Call it W, and write it after S.

```host-lint:ignore
   place    1   2   3   4   5
   data     3   7   1   9   4

   weighted  =  1×3 + 2×7 + 3×1 + 4×9 + 5×4  =  76  =  seven tens and 6   ->   W = 6
```

Because each place has its own weight, a change in any place shifts W, so the change is caught.
After you mend a row, work W again to confirm the row is whole. Two lost digits at once, or a
change with no blank place to point at, are past the hand method, and there the machine tier
takes over.

## 8. Mending a whole page

The same idea works one step up. A repair page stands for a group of pages and holds, in each
place, the sum of that place across the group, ones left over kept. To rebuild a page that is
wholly lost, take the repair page and subtract the pages you still have, place by place. A
smudge is mended within a page; a lost page is mended across its group.

## 9. The smaller base

For the dense rows the book uses six digits, 0 1 2 3 4 5, rather than ten, and short groups.
Fewer marks, drawn further apart, survive damage better and pack more tightly. Nothing in the
method changes: wherever you grouped or set aside tens, group or set aside sixes instead. The
same row, worked in the smaller base:

```host-lint:ignore
   data     3   5   1   4   2

   sum       =  3 + 5 + 1 + 4 + 2  =  15  =  two sixes and 3   ->   S = 3
   weighted  =  1×3 + 2×5 + 3×1 + 4×4 + 5×2  =  42  =  seven sixes and 0   ->   W = 0
```

## 10. A guard, worked in full

You have the parts; here they are put together on real content, in the smaller base.

**A row of digits.** Take five data digits, work the two guards, and write the row as it stands
in the book: the five data digits, then S, then W.

```host-lint:ignore
   data                     3 5 1 4 2
   sum       ->  S = 3
   weighted  ->  W = 0

   the row, as written:     3 5 1 4 2  3 0
                            |-- data --| S W
```

To read a row, take its first five digits as the data and its last two as the guards.

**A piece of text.** The book stores a sequence of values, and a file of any kind is such a
sequence. Text becomes values by a code table the book carries, one number for each character.
Each value is written as four base-six digits, since four of them reach far past any single
value, so every value fits. The digits are then grouped by five and guarded as above.

```host-lint:ignore
   text             H       i
   value            72      105          (looked up in the code table)

   each value as four base-six digits:
       72   ->   0 2 0 0
       105  ->   0 2 5 3

   the digits, joined:      0 2 0 0 0 2 5 3
   grouped by five, the last group filled out with 0:
       0 2 0 0 0   ->   S = 2,  W = 4   ->   row   0 2 0 0 0  2 4
       2 5 3 0 0   ->   S = 4,  W = 3   ->   row   2 5 3 0 0  4 3
```

To read the text back, reverse the steps: join each row's five data digits into one stream, cut
the stream into fours, turn each four into its value, and look each value up in the code table.

## 11. The pages, their headers, and the table of contents

Each dense page has a short readable header. It gives the page's number in the whole book, and,
for a file split across pages, which piece it is and where that piece begins. A reader uses the
headers to put pages back in order. The readable table of contents lists every file: its name,
the pages that hold it, its length, and a check value to confirm it. To recover a file, find it
there, gather its pages in order by their headers, and read them. A file smaller than one page's
worth sits on a single page; a larger file spans several pages in sequence, and nothing about it
is special except that there are more pages.

## 12. Integrity: confirming the whole book

Two checks confirm that nothing has changed. A person adds a running sum over the book, given in
the table of contents, and compares it; this needs only addition. A machine compares a longer
check value, printed as digits beside the running sum. A rebuilt program confirms it in a moment,
though it is too long for a person to add by hand. If either check matches, the book is whole.

## 13. The machine and its printed program

The dense fields are read quickly by a small program. Its complete source is printed later in
this book, in two computer languages, so a reader who can build a computer can type it back in
and run it. The hand method taught above is enough to recover that program if everything else is
gone, because the program too is stored as recoverable rows. The program reads the dense fields;
the fields hold the same information as the rows, packed smaller.

## 14. What you can now do

With the counting and the two guards, you recover any single row, and any single page, by hand.
With the headers and the table of contents, you find any file and confirm the book is whole. With
the printed program, you rebuild the machine that reads the dense pages at speed. Nothing in the
book depends on anything outside the book, so a reader who has the book and a pen can begin, with
patience.

## Open, for this draft

- The three-column trilingual layout (the languages are locked to English, Mandarin, and
  Spanish; see [call/0007](../../call/0007-preamble-languages.md)).
- The figures: the strokes-to-digits chart and the worked rows become the real, drawn figures
  once the glyph alphabet is fixed.
