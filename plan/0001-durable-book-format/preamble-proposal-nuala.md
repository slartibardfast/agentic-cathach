# Preamble proposal (Nuala's alternative draft)

This is an alternative draft of the cathach preamble, "How to read this book",
designed from the viewpoint of [Nuala](../../cast/nuala-numeral-linguist.md), the
comparative linguist of number. It is a fresh proposal, not an edit of
[preamble-draft.md](preamble-draft.md). It covers the same ground, but it moves the
teaching off the sentence and onto the figure wherever a sentence would only work
for a reader whose own language already thinks in base ten, reads in one direction,
or has a grammar of number-words.

The floor, the budget, the three languages, and the reserved base six are unchanged
(see [call/0007](../../call/0007-preamble-languages.md) and
[call/0008](../../call/0008-assumed-knowledge-floor.md)). What changes is where the
meaning lives.

## How this draft differs in principle

The figures are the language-independent spine, so any meaning that a figure can
carry is taken out of the prose and put into the figure. The prose only points.
These meanings were shifted onto the figure, out of the prose:

- **Reading direction and place order.** No sentence says "from the left". Direction
  is fixed once, physically, by the corner marks that open every page, and called "onward".
  Every figure then lays places out in that same onward order, so the reader learns
  the order by seeing it repeated, not by a direction-word from one language.
- **The weights of the second guard.** The current draft says "number the places from
  the left". This draft prints the weight of each column as a small staircase of
  strokes directly beneath that column, so the multiplier is attached to the cell and
  no counting-direction is assumed. A reader who reads the other way still reads the
  same weight off the same column.
- **What a base is.** This is the deepest cross-linguistic point, so it is made
  explicit and figure-driven. One count of strokes is shown grouped two different
  ways, so the same strokes take two different numerals. The reader sees that the
  size of a group is a free choice, that ten is only the size this book teaches with,
  and that the strokes are the real quantity. A reader who counts in scores or by
  body-parts sees their own habit as one more group-size.
- **Keeping the ones left over.** No "remainder" and no division. Because the ones
  place of a total is exactly the loose count after full groups are set aside, each
  guard is simply read off as the ones place of a total already written by places.
  This is shown once with strokes, then reused.
- **Damage.** The book never prints a loss mark. Damage is shown as a smudge that
  falls on a known cell, drawn as a solid block over that column. There is no "?" and
  no loss glyph, because a fresh page never carries one.
- **Zero.** The mark for an empty place is introduced at the exact step where an empty
  place is first born, out of the grouping, not asserted up front. It is a positive
  mark, never a blank, because a blank cannot be told from damage. Its shape is left
  open and shown here by a stand-in.

English words are kept out of the figures. Where the current draft writes number-words
("nine", "ten", "twelve") or labels ("hundreds place", "data", "sum", "weighted")
inside a figure, this draft carries the same meaning by marks, columns, and layout, so
the one figure serves all three languages unchanged.

The teaching stays in base ten. Base six is reserved for the dense rows near the end,
where it arrives as one more group-size by the method already shown.

---

# How to read this book

## 1. What this book is

This book holds information, kept so that every page can be turned back into the exact
marks it was made from. Nothing is lost. You do not need a machine to begin. The pages
that follow teach you to read the information by hand, with counting, addition,
multiplication, and subtraction, and nothing harder. A machine reads it faster, and a
program printed later in the book rebuilds that machine, but the hand method never
depends on the machine.

## 2. How to read the marks

Every page carries a mark in each corner. The four marks differ, so they fix which way
up the page is held and which corner a reading begins from. From that corner the marks
are read along a line in one direction. Call that direction onward. At the end of a
line the next line below is read, again onward, and the pages are read in the order of
their printed numbers.

```host-lint:ignore
   +--------------------+
   | #                . |
   |                    |
   |                    |
   | o                : |
   +--------------------+
     ^ begins here, reads onward ->
```

There are two kinds of mark. One is writing like this, set for a person. The other is
dense fields of small squares, set for a machine or for a very patient person following
these instructions. The writing teaches. The fields carry the bulk. This preamble is
all writing.

## 3. Counting, and the single marks

You can count marks. Beside each count below stands a single mark that means that count.
The single mark and the strokes share a line, so the meaning is fixed without a word.

```host-lint:ignore
   1   |
   2   ||
   3   |||
   4   ||||
   5   |||||
   6   ||||||
   7   |||||||
   8   ||||||||
   9   |||||||||
```

There is a single mark for each count up to nine strokes. Nine strokes is the last count
with a single mark of its own. For more, we do not invent new marks. We group.

## 4. Places, by grouping

Take one more stroke than the last single mark. There is no single mark for it. So gather
a full group into one enclosure. The enclosure counts as one thing in a new place, opened
onward of the ones. The ones place is now empty, and an empty place still has to be
marked, so it is held by a mark of its own, the absence mark, shown here by the stand-in
Z. The absence mark is a real mark, never a blank, because a blank could not be told from
a place rubbed away.

```host-lint:ignore
   |||||||||                     1     (the last single mark)

   (||||||||||)                  1 Z   (one enclosure, no loose ones)
```

Two more strokes than the enclosure make two loose ones beside it.

```host-lint:ignore
   (||||||||||) | |              1 2
```

### A group-size is a choice

Here is the same count of strokes, grouped two different ways. The strokes do not change.
The numeral changes with the size of the enclosure.

```host-lint:ignore
   | | | | | | | | | | | | | | |

   (| | | | | | | | | |) | | | | |             1 5

   (| | | |)(| | | |)(| | | |) | | |           3 3
```

The first enclosure holds ten strokes, and the count is written 1 5. The second holds
four strokes, and the very same strokes are written 3 3. The size of the enclosure is a
free choice. That size is called the base. This book teaches with ten strokes to an
enclosure. What the writing records is the strokes, whatever base a reader's own tongue
prefers.

### Places above the first

When enclosures themselves reach a full group, gather them into one larger enclosure,
opened one place further onward. Each place onward is the base times the place before it.
An empty place between filled ones is held by the absence mark, so no place is ever lost.

```host-lint:ignore
   (( ))(( ))                              | | | | |
       2                 Z                     5

   2 Z 5
```

Two large enclosures, no single enclosures, and five loose ones are written 2 Z 5. The
empty middle place carries Z, not a gap.

Two habits carry the rest. A place is read by where it sits. Complete enclosures are set
aside so the loose ones left over can be kept. Both return in the guards.

## 5. Rows, and what can go wrong

The information sits in short rows of digits, the digits you have just learned. Paper is
fragile. A smudge can fall on a cell and take its mark away. The cell's place is still
known, but its value is gone.

```host-lint:ignore
   3 7 1 9 4

   3 7 █ 9 4
```

The block is not a printed mark. It is damage, shown falling on the third cell. To catch
and to mend such damage, a little is added to each row, worked from the row itself, with
counting, adding, multiplying, and the grouping of the last section.

## 6. The first guard: the sum

Add the data digits of the row. The total is written by places, as you now write any
count. Its ones place is exactly the loose ones left after full tens are set aside, so
the ones place of the total is the first guard. Call it S. Write it just after the data.

```host-lint:ignore
   24  =  (||||||||||)(||||||||||) | | | |
```

Twenty-four gathers into two full tens and four loose. The four loose is the ones place
of 2 4. So the guard is read straight off the ones place, with no division.

```host-lint:ignore
   3 7 1 9 4

   3 + 7 + 1 + 9 + 4  =  2 4              S = 4
```

### Repair by subtraction

If a smudge later takes one cell, and you can see which place has gone, add the digits you
can still read, take the ones place of that total, and subtract it from S. The lost digit
returns, exactly.

```host-lint:ignore
   3 7 █ 9 4          S = 4

   3 + 7 + 9 + 4  =  2 3
   S - 3  =  4 - 3  =  1

   3 7 1 9 4
```

If the ones place you must subtract is larger than S, add one enclosure of ten to S first,
then subtract.

```host-lint:ignore
   S = 1     take away 4

   1 + (||||||||||) - 4  =  1 1 - 4  =  7
```

## 7. The second guard: the weighted sum

The sum alone cannot notice one digit swapped for another when the total is unchanged. So a
second guard weights each cell by its column. The weight of each column is drawn beneath it
as a staircase of strokes, one stroke for the first cell onward and one more for each cell
after. The weight rides with the column, so it does not depend on which way you read.

```host-lint:ignore
   3       7       1       9       4
   |       ||      |||     ||||    |||||

   3×1  +  7×2  +  1×3  +  9×4  +  4×5   =  7 6      W = 6
```

Multiply each digit by the strokes beneath it, add the results, and take the ones place of
the total. That digit is the second guard. Call it W and write it after S. Because each
column carries its own weight, a change in any single cell shifts W, so the change is
caught. After you mend a row, work W again to confirm the row is whole.

Two lost cells at once, or a change with no smudged place to point to, are past the hand
method. There the machine tier takes over.

## 8. Mending a whole page

The same idea works one step up. A repair page stands for a group of pages. In each place
it holds the sum of that place across the group, ones left over kept. If one page is wholly
lost, take the repair page and subtract the surviving pages, place by place, adding an
enclosure of ten where a place will not subtract, exactly as in a row. A smudge is mended
within a page. A lost page is mended across its group.

## 9. The smaller base

The dense rows do not group by ten. They group by six, the digits 0 1 2 3 4 5, in short
groups. Fewer marks, drawn further apart, survive damage better and pack more tightly. This
is only another group-size, as you already saw the strokes take. Nothing in the method
changes. Wherever you set aside full tens, set aside full sixes instead, and read the ones
place in base six. The same shape of row, worked in base six:

```host-lint:ignore
   3 5 1 4 2

   3 + 5 + 1 + 4 + 2  =  (||||||)(||||||) | | |        S = 3

   3   5   1   4   2
   |   ||  |||  ||||  |||||

   3×1 + 5×2 + 1×3 + 4×4 + 2×5  =  42                  W = 0
```

Fifteen strokes gather into two full sixes and three loose, so S is 3. Forty-two gathers
into seven full sixes with nothing loose, so its ones place in base six is 0, and W is 0.

## 10. A guard, worked in full

Here the parts are put together on real content, in base six.

A row as it stands in the book is the five data digits, then S, then W.

```host-lint:ignore
   3 5 1 4 2                     data
   S = 3
   W = 0

   3 5 1 4 2  3 0                the row, as written
```

To read a row, take its first five digits as the data and its last two as the guards.

The book stores a sequence of values, and a file of any kind is such a sequence. A code
table the book carries pairs each character with a value. Each value is written as four
base-six digits, which reach far past any single value, so every value fits. The place
values of the four digits are one, six, six sixes, and six six-sixes, each place six times
the one before.

```host-lint:ignore
   place values, base six, four places:

   ((( )))     (( ))     ( )     |
   2 1 6       3 6       6       1
```

To read a value, multiply each of its four digits by its place value and add. Then look the
value up in the code table.

```host-lint:ignore
   code table:   H  <->  7 2          i  <->  1 0 5

   0 2 0 0   ->   0×216 + 2×36 + 0×6 + 0×1  =  7 2    ->   H
   0 2 5 3   ->   0×216 + 2×36 + 5×6 + 3×1  =  1 0 5  ->   i
```

The digits of the values run together into one stream, and the stream is cut into groups of
five, each group guarded as above. A short last group is filled out with the absence mark.

```host-lint:ignore
   values joined:      0 2 0 0 0 2 5 3
   cut into fives:     0 2 0 0 0        2 5 3 0 0

   0 2 0 0 0    S = 2   W = 4    ->    0 2 0 0 0  2 4
   2 5 3 0 0    S = 4   W = 3    ->    2 5 3 0 0  4 3
```

To read the text back, take each row's first five digits, join them into one stream, cut the
stream into fours, turn each four into its value by its place values, and look each value up
in the code table.

## 11. The pages, their headers, and the table of contents

Each dense page carries a short readable header, a fixed row of numbers in fixed positions.
It gives the page's number in the whole book and, for a file split across pages, which piece
sits here and where that piece begins. A reader uses the headers to put the pages back in
order by their numbers.

The readable table of contents lists every file: its name, the pages that hold it, its
length, and a check value to confirm it. To recover a file, find it there, gather its pages
in order by their headers, and read them. A file smaller than one page sits on a single
page. A larger file runs across several pages in sequence, and nothing about it is special
except that there are more pages.

## 12. Confirming the whole book

Two checks confirm that nothing has changed. A person adds a running sum across the book,
given in the table of contents, and compares it. This needs only addition. A machine
compares a longer check value, printed as digits beside the running sum, too long for a
person to add by hand but confirmed by a rebuilt program in a moment. If a check matches,
the book is whole.

## 13. The machine and its printed program

The dense fields are read quickly by a small program. Its full source is printed later in
this book, in two computer languages, so a reader who can build a computer can type it back
in and run it. The hand method taught above is enough to recover that program if all else is
gone, because the program too is stored as recoverable rows. The program reads the dense
fields, and the fields hold the same information as the rows, packed smaller.

## 14. What you can now do

With the counting and the two guards you recover any single row and any single page by hand.
With the headers and the table of contents you find any file and confirm the book is whole.
With the printed program you rebuild the machine that reads the dense pages at speed. Nothing
in the book depends on anything outside the book, so a reader with the book and a pen can
begin, given patience.

## Open

Left unfixed on purpose, to be settled later:

- **The glyph shapes.** The digit marks, and the absence mark shown here by the stand-in Z,
  are fixed by measurement of the real paper, toner, and printer, so that damage lands a
  mark in the cheap smudged-cell regime rather than turning it into another valid mark (see
  [#design-alphabet](README.md#design-alphabet)). The absence mark's shape is open, with one
  rule already settled: it is a positive mark, never a blank.
- **The base and the overhead.** The dense rows are taught here at six. The exact radix
  within four to eight, and the number of guard digits per group, are drawn against the
  measured silent-substitution rate ([poc-findings.md](poc-findings.md)).
- **The figures as drawn.** The stroke charts, the code table, and the worked rows become the
  real drawn figures once the glyph alphabet is fixed. The stand-in marks in this draft are
  placeholders for those glyphs.
- **The three-column layout.** The languages are locked to English, Mandarin, and Spanish
  (see [call/0007](../../call/0007-preamble-languages.md)). How the three run beside the
  shared figures on the page, in three columns or three passes, is a typesetting decision
  still open.
