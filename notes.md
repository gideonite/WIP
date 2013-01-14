# THOUGHTS

Two projects currently in progress.

* CN Pipeline
* oncoprint

## CN Pipeline

## oncoprint

refactoring of current code.  What this means is that now, instead of the
server returning strings of html and code which is then evaluated in the
browser, it returns code that makes an asynchronus call back to the server
(AJAX), grabs data and then renders it.

What this means is that now the data for the oncoprint is returned as, and
persists as, a single data structure in the browser.

This data structure can be accessed and manipulated by different functions, for
example sorting, and rendered into different visualizations, the oncoprint
being one of them.  This opens the way for fast, dynamic interaction with the
data in the browser, without having to make another request to the server and
waiting for a request.

d3 is a javascript library that allows for highly customizable and interactive
data visualizations.  Generally speaking, it is based on a notion of structural
correspondance (isomorphism) between data and visualization.  A piece of data
has a certain structure, which is mirrored in the visualization of that piece
of data.


# OUTLINE

## CN Pipeline

### Goal
CBS --> GISTIC

broken down:

1. get the implementations of CBS and GISTIC working
2. get them to work together, i.e. merge CBS files and run them through GISTIC.
3. put it up onto a Galaxy server (Lowe's lab?)

(what is Galaxy again? It's a very flexible system but for us it is a server
that runs, and creates pipelines)

### Difficulties

...stuck on step 1.  Mostly the problems have been with markers files.

CBS takes input like this:
...

TCGA `Level_2` data looks like this:
...

So you need to map between them

Stuck on

### In Progress

## Oncoprint

### What Was Done

* backend: refactoring
* frontend: rewrote using [d3.js](www.d3js.org)

### Check it out
Can you get an oncoprint into one of these slides?  You could first try just
copying it, and then maybe try actually making an AJAX call.

### Big Picture
* backend refactoring: separation of data and data visualization

* data transformation in the browser, e.g. oncoprint sorting

### Future Oncoprint
What does this mean for the future?  A much more interactive portal, where data
manipulation happens dynamically and users can navigate high dimensional data
by using various incarnations of filtering and sorting.

For example,

### Clinical Data in Oncoprint
(can you make a quick sample with a row of clinical data?)

Can sort, unsort, resort based on different clinical attributes.

In progress, incorporating TCGA Clinical Data into our system...

#QUESTIONS?

# THANKS
put this at the beginning?

cBio Portal Team:
* alphabetical order

Giovanni who worked with me on the Pipeline.
Yupu and others at the Bioinformatics Core.



Need marker files, like this:<pre>
#name       chromosome  position
CN_473963   1           61735
CN_473964   1           61808
CN_473965   1           61823
</pre>

