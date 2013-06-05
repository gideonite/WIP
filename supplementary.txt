Notes
-----

Introductory remarks.  Hello everyone, for those of you who don't know me, my
name is Gideon.  I've been here for about 6 months and my official title is
"Software Engineer Associate," which Rayleen has dubbed "SEA," as in "at SEA,"
as in I'm always confused.

These are some people that I'd like to thank.  People I've had the pleasure of
working and exchanging ideas with.

I thought I'd talk about the things that I'm currently working on.  Basically
I'm going to be fleshing out two of the projects that Ben touched upon when
giving his WIP a few weeks ago.

# CN Pipeline

First, I'm going to talk a bit about the Copy Number pipeline.  Basically, this
is the idea: to link together the CBS and GISTIC algorithms to be able to mimic
the kind of analysis that is done regularly done at the Broad on TCGA data.

## Why?

Generally speaking, we are interested in segmentation because we get array data
that looks like this.  As always, we want to be able to formalize the finding
of segments for the sake of automation, as well as for the sake of detection.

There are other algorithms for segmentation which I know nothing about, but the
one that Broad uses and the one that we use is called CBS.

Of course, once we have segments from a set of samples, we want to know what
the recurrant segments are, if any.  And along with that, we want to know what
the recurrantly amplified or deleted genes are.

## How?

CBS looks for pairs of breakpoints.

GISTIC constructs a background "rate" of CNV given length and signal strength.
It does this by iteratively computing "CN histories" and their corresponding
distributions p(length, amplitude) until the p's converge.

Then you can use this p to compute the likelihood that a probe is selected for
by multiplying the probabilities over all the segments that contain it.  And
for genes you can compute the score by summing these probabilities.  (yes, this
is strange!)

## What?

Fairly straight forward.  Get the algorithms working, pipe them together, add
to a galaxy webserver.

Worked well with the pipeline from the Bioinformatics Core, but there have been
some troubles with getting Affymetrix SNP6.0 data to work (actually, this is
the primary platform at TCGA).

`map_probes.py` is a script that mangles the probe mapping accordingly after
providing some a basic *diff* of the unmapped probes and the markersfile.


# Oncoprint

backend refactoring: separation of data from view.  So now the server returns
more static html and code, and a JSON data structure to be rendered by that
code.

the frontend was rewritten by d3.  Interestingly enough, many people here in
the lab seem interested in d3 and I would be more than happy to talk, write
code, whatever, with anyone who is interested.  The last slide of this talk is
a quick summary that I thought of on the subway the other day.

Basically, d3 is a powerful framework for manipulating DOM elements based on
data.  It is great for making custom visualizations based on changing data or
if you want the visualization to be interactive.

## Future

All of this was actually originally inspired by a need to add new functionality
to the oncoprint, namely clinical data.  We are finally at the point that I can
start incorporating this and I've begun to incorporate clinical data into the
backend importer and database so that it can eventually be sent to the front
end for visualization.

With the addition of this clinical data however, it is becoming increasingly
clear that we are going to want to make the oncoprint more interactive, that
is, more dynamic sorting, etc.  The structure for this is in place, and now we
just need to see if it will hold up.

As for the portal, this is basically the overall pattern that future
development on the portal is going to look like (microcosm).  Separating data
from the functions that operate on the data (sorting, visualization, etc), at
the same time putting data transformation on the client side for increasing
interactivity.
