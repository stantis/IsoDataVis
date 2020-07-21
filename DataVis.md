Stable Isotopes for Archaeology: Data Visualization
================
C Stantis
7/21/2020

  - [The Background](#the-background)
  - [Axis Text and Ticks](#axis-text-and-ticks)
      - [Values](#values)
      - [Ticks and value range](#ticks-and-value-range)
      - [Tick Units](#tick-units)
      - [Axis Titles](#axis-titles)
      - [Labels](#labels)
  - [The Data Points](#the-data-points)
      - [Size](#size)
      - [Bar Graphs For Strontium?](#bar-graphs-for-strontium)

Good data visualization helps you tell your data’s story with minimal
misunderstanding.

Here I’m going to explain the datavis rules that I follow to create
stable isotopes graphs, along with the associated R code. I’m going to
use bone collagen data (carbon, nitrogen, sulphur, and strontium) from
my [PhD Thesis](https://ourarchive.otago.ac.nz/handle/10523/6019). These
are bone samples from individuals excavated from two Polynesian sites,
Bourewa (Fiji) and \`Atele (Tonga).

I’ve cleaned the data and put it in this repository as ThesisData.xlsx.
Maybe later I’ll share some R tips for cleaning data, but today let’s
focus on datavis. Let’s focus on one component at a time, thinking about
**why** it might be better to go in one art direction rather than
another, and the underlying R code for making your data follow this
datavis rule.

Let’s build the graph, one piece at a time.

# The Background

I love dark themes. I’m writing in one of RStudio’s right now. But,
unless you’re creating graphs for a dark-themed presentation or web
output, graphs with black backgrounds aren’t going to work well in
publication and **no one** will like you if they’re printing out your
paper.

Some people like grid lines but I prefer a clean slate. The first image
below is the carbon and nitrogen data plotted using ggplot, with no
theme. The second image is the same data, but using the
`theme_classic()`
option.

<img src="LotsaGraphs_files/figure-gfm/Background Example-1.png" width="50%" /><img src="LotsaGraphs_files/figure-gfm/Background Example-2.png" width="50%" />

# Axis Text and Ticks

## Values

When displaying any interval data, don’t display any more decimal places
than needed. Otherwise, the graph becomes increasingly difficult to
read. Yes, I know that many stable isotopes data are presented to the
first or even second decimal, you don’t need decimal points on your axes
for isotope data other than strontium, save that richness of data for
tables and text. This might be my biggest pet peeve for the smallest
reason. It’s hard to add extraneous decimal places in R, so I’m mostly
looking at you, Excel users.

![don’t do this.](figures/excel_decimal.jpg "Awful Axes")

Make sure the values text is large enough to read easily for most. Err
on the side of caution, some publishers like to shove your figure into
one column of a two-column article
layout.

<img src="LotsaGraphs_files/figure-gfm/Axis-1.png" width="50%" /><img src="LotsaGraphs_files/figure-gfm/Axis-2.png" width="50%" />
I sometimes angle the x-axis values because I think that makes me look
fancy. ![](LotsaGraphs_files/figure-gfm/AxisAngle-1.png)<!-- -->

## Ticks and value range

Be mindful of the value range you use. R and Excel try to be helpful,
but there’s two major ways they can be wrong.

### Starting at zero

Again, looking at you Excel.

![don’t do this either.](figures/excel_zero.jpg "Awful Axes, pt II")

### Axis range too narrow

This one’s trickier, and I often see researchers still learning about
stable isotopes make this mistake. A population eating only from a C3
environment with no C4 or marine input might show a range in C-values of
only 2 per mill. That doesn’t mean your x-axis should only be 2 per mill
across\! This encourages you and your readers to think about differences
in values on too small of a scale, and see patterns that aren’t
biologically meanful. Even if values are tight, widen out to include the
expected environmental range.

    ## Warning: Removed 31 rows containing missing values (geom_point).

![](LotsaGraphs_files/figure-gfm/AxisRange-1.png)<!-- -->

## Tick Units

Adjust your major units on the axes accordingly to the range you have.
Too few tick marks, and it’s hard to estimate the values using the graph
and the reader has to delve into your raw data. Too many tick marks, and
it’s visually really busy. R likes to do it’s best, but don’t be afraid
to be specific, especially for strontium data.

    ## Warning: Removed 62 rows containing missing values (geom_point).

![](LotsaGraphs_files/figure-gfm/Tick%20Units-1.png)<!-- -->

    ## Warning: Removed 62 rows containing missing values (geom_point).

![](LotsaGraphs_files/figure-gfm/Tick%20Units-2.png)<!-- -->

## Axis Titles

For each axis, you’ll want: \* what’s being measured
(e.g. \(\delta ^{13}\)C, \(\delta ^{15}\)N) \* the unit (i.e. per mill)
\* the international reference used

## Labels

`ggrepel` let’s you make sure your text labels don’t overlap. My example
is a little harsh, as I could have used `hjust = 0, nudge_x = 0.05` to
move the text off of the points, but `geom_text_repel` is quick and
easy.

<img src="LotsaGraphs_files/figure-gfm/Text Overlap-1.png" width="50%" /><img src="LotsaGraphs_files/figure-gfm/Text Overlap-2.png" width="50%" />

If you want to only label a few points on the graph, you can either
`geom_text_repel` using the `data(subset = ...)` or `annotate('text')`

![](LotsaGraphs_files/figure-gfm/Annotation-1.png)<!-- -->

# The Data Points

This is my favorite part, where the graph really starts to shine.

## Size

I like to make the points a little bigger than the base size, so long as
they’re not
overlapping.

<img src="LotsaGraphs_files/figure-gfm/Size Queen-1.png" width="50%" /><img src="LotsaGraphs_files/figure-gfm/Size Queen-2.png" width="50%" />
\#\# Color I love color. I often try to create my own color palettes for
data presentation, keeping in mind that they should be color-blind and
printer friendly. When I don’t want to create a bespoke palette, I use
the package
`viridis`.

<img src="LotsaGraphs_files/figure-gfm/Viridis Showoff-1.png" width="30%" /><img src="LotsaGraphs_files/figure-gfm/Viridis Showoff-2.png" width="30%" /><img src="LotsaGraphs_files/figure-gfm/Viridis Showoff-3.png" width="30%" />

The Viridis package isn’t great for comparing two groups, so you’re best
off coming up with your own color scheme at that
point.

<img src="LotsaGraphs_files/figure-gfm/Color-1.png" width="50%" /><img src="LotsaGraphs_files/figure-gfm/Color-2.png" width="50%" />

## Bar Graphs For Strontium?

No. Never.
