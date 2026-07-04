# Preamble draft (working)

This is a working draft of the cathach preamble: the readable opening that teaches a
reader to decode the book by hand, before any machine. It is the sparsest and most
protected part of the book, and the one part that must survive with nothing else.

The final preamble is trilingual, so the same meaning is carried in three languages
chosen by literate-speaker population: English, Mandarin, and Spanish (see
[call/0007](../../call/0007-preamble-languages.md)). This draft is the English text plus the shared
figures, which are language-independent and do the real teaching. The figures are the
spine; the prose only points at them.

**Assumed-knowledge floor.** This draft assumes the least it can: that the reader sees
that marks carry meaning, and that the reader can count. It then teaches the digits,
the places, the checks, and the repair. Lowering the floor further, or fixing it firmly,
is the deepest open question of the whole format ([poc-findings.md](poc-findings.md)).

## Order of teaching

1. What this book is, and that it can be read back.
2. Two kinds of mark: this writing, and the dense fields.
3. Counting, and the ten digits.
4. Places, so digits make larger numbers.
5. The rows: data digits and two checks, S and W.
6. Repair: recovering one lost digit by subtraction.
7. The pages, and the table of contents.
8. The machine, and the printed program that rebuilds it.

Sections 3 to 6 are the core: with them a patient reader recovers any single page by
hand. The rest is navigation.

## 1. What this book is

This book holds information. Every page can be turned back into the exact marks it was
made from, so nothing is lost. You do not need a machine to begin: the next pages teach
you to read the information by hand, using only counting, addition, and subtraction. A
machine reads it faster, and a printed program later in the book rebuilds that machine,
but the hand method never depends on it.

## 2. Two kinds of mark

There are two kinds of mark in this book. The first is writing like this, meant for a
person to read. The second is dense fields of small squares, meant for a machine, or for
a patient person following these instructions. The writing teaches; the fields carry the
bulk. This preamble is all writing.

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

## 4. Places

A number is a row of digits. The rightmost digit counts ones. The next digit to its left
counts groups of ten. The next counts groups of ten tens, and so on, each place ten times
the place to its right:

```host-lint:ignore
   2 0 3   means   (2 groups of ten tens) + (0 groups of ten) + (3 ones)  =  two hundred and three
```

This is all the number-reading the preamble needs. The dense data uses the same digits,
but only the first few of them (a small base), written in short rows.

## 5. The rows: data and two checks

The recoverable data is written in rows. Each row is one group: a few data digits,
followed by two check digits we call S and W. The checks are computed from the data, and
they let you find and repair damage.

For a worked example we use base ten and a row of five data digits, so the arithmetic is
familiar. (The real book uses a smaller base; the method is identical.)

```host-lint:ignore
   place    1   2   3   4   5
   data     3   7   1   9   4

   S = the sum of the data,            kept to one digit by casting out tens:
       3 + 7 + 1 + 9 + 4  =  24    ->   S = 4
   W = the sum of (place times digit), kept the same way:
       1*3 + 2*7 + 3*1 + 4*9 + 5*4  =  76    ->   W = 6
```

S is a plain sum; it repairs a missing digit. W weights each digit by its place; it
catches a digit that was changed for another. Neither uses division, so both stay inside
a budget of addition and multiplication.

## 6. Repair: one lost digit

Suppose damage destroys one digit, but you can still see which place is now empty. That is
the easy case: the place is known, only the value is gone. Add the data digits you can
still read, and subtract from S:

```host-lint:ignore
   damaged  3   7   ?   9   4      with  S = 4

   known sum = 3 + 7 + 9 + 4  =  23    ->   cast out tens  ->  3
   lost digit = S - 3 = 4 - 3 = 1

   the row was  3 7 1 9 4  again — recovered exactly, by subtraction alone.
```

If two digits in a row are lost at once, or a digit was changed with no empty place to
mark it, the hand method stops and the machine tier takes over. After a repair, recompute
W to confirm the row is whole.

## 7. The pages and the table of contents

- These readable pages teach you and hold the **table of contents**: a list of the files
  in the book, and for each file the pages that carry it and a check value to confirm it.
- The dense pages carry the files. Each dense page has a short readable header naming its
  place in the sequence, so the pages can be put back in order.
- To recover a file, find it in the table of contents, gather its pages in order, and read
  them. A page lost entirely is rebuilt from a nearby repair page.

## 8. The machine and its program

The dense fields are read quickly by a small program. Its complete source is printed later
in this book, in two computer languages, so a reader who can build a computer can type it
back in and run it. The hand method in sections 3 to 6 is enough to recover that program
if everything else is gone, because the program too is stored as recoverable rows.

## Open, for this draft

- The assumed-knowledge floor: how much a far-future reader is presumed to know. This
  gates how sections 3 and 4 are written, and it is unresolved.
- The three-column trilingual layout (the languages are locked to English, Mandarin, and
  Spanish; see [call/0007](../../call/0007-preamble-languages.md)).
- The figures: the strokes-to-digits chart and the worked rows become the real, drawn
  figures once the glyph alphabet is fixed.
