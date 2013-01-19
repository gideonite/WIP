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

### Why do we do CBS and GISTIC?

#### Why do we do CBS?

We get a list of probe level measurements.
<a href=http://www.chem.agilent.com/Library/posters/Public/Copy_Number_Variant_Detection_Using_Agilent_CGH_Platforms.pdf>platform info</a>

But, there is noise in those measurments.  Two probes on the same segment will
not necessarily have the same signal due to the noise.  So given all of these
measurements, we need to find the segments.  (example .png)

(genome location vs. log ratio)

**What is CBS?**

Circular Binary Segmentation:

*Binary Segmentation* has existed since 1975
(<http://projecteuclid.org/euclid.aos/1176343001>).  Basically, the way it
works is by using MLE to find where a particular mean in a list of means
differs from a reference.  This is a search for signle break points.

*Circular* is Olshen and Venkratraman's extension in which you search, not for
single break points, but for pairs of breakpoints. "Circle" is a nice metaphor
because if you are travelling around in signal strength space, then pairs of
breakpoints should have the same value and should therefore form a "circle"
virtue of the fact that they should have the same signal.  This is said to
improve the "detection of of a small change buried in the middle of a large
segment."

#### Why do we do GISTIC?

* recurrence.
* bonus: gene calls.

Technically, the only thing that GISTIC should be doing is finding the ROIs
(Regions of Interest), the geometrical segments of the genome that are reccurent
in population.  But, GISTIC also provides the bonus of trying to figure out the
most likely genes (so says Niki). <http://genomebiology.com/2011/12/4/R41>

What's the word for this?  Gene call?

GISTIC works if you get the input files "just right."  Even on TCGA Level_3
data (CBS segments), there may be a probe or two which is off and will cause
GISTIC to fail with an unhelpful error message.

#### GISTIC supplementary materials

GISTIC2.0 is an iterative MLE algorithm (whatever that is, I suppose you might
want to think about Expectation Maximization):

You are given a set of breakpoints, you're job is to estimate the CN events
that took place.  Say you have just 3 breakpoints for example.  What could have
happened?  It could have been just one event (the middle one), or it could have
been two events.  Of course, you have more than just breakpoints, you also have
amplitudes help you start to think about this...


1. *base case:* Construct the base background rate by using a simplified
   deconstruction with the following two assumptions:

   a. break points represent only a single CN event
   b. CN gains are not followed by CN losses and visa-versa

Given these two assumptions, it should be clear that the higher the amplitude
of the CNA, the later it must have occured in the SCNA history.  And thus, we
can easily construct an initial deconstruction (SCNA history), and from that
bin to compute the background rate of SCNA.

This basically boostrapping the ping pong between background rate and
deconstruction by making the assumptions necessarily to determine
(deterministically!) a deconstruction (history).

2. *induction step:* create a new background rate of SCNA from the current
   history, which here amounts to simply a list of CN events.  So take
   those SCNAs and bin them according by length and amplitude, and then use a
   frequency from that.  Construct a new history using the new background
   rate.

Keep doing this until there is convergence (not proven that it always
converges, but apparently it does "in practice").

*Always assuming that SCNAs are independent*

##### Gene GISTIC:

In addition to doing the stuff above (actually, question -- what do you get out
of all of this??), there is also Gene GISTIC.  We are going to score individual
markers, by taking the log likelihood of observing focal events over i

Say we have a marker `i`, we can compute a score on `i` by taking the log
likelihood of all the focal SCNAs over `i`, given the arm-level SCNAs over `i`.
We get the probability function from the steps above.  So the goal of the steps
above is to generate that background rate of SCNA given length and amplitude.

You can extend this to genes by taking the min of the above score on all the
markers in a gene.

**And then, and this is something I don't really understand, you sum this min
score across all of your samples.**

And that's GISTIC scores genes.

**Still need to understand targeted regions of CNA.**

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
Liang Yupu and others at the Bioinformatics Core.



Need marker files, like this:<pre>
#name       chromosome  position
CN_473963   1           61735
CN_473964   1           61808
CN_473965   1           61823
</pre>

