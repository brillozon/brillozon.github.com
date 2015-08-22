---
layout: post
title: Stampede References
description: Reference links and citations from StampedeCon 2015 presentation
category: Publications
tags: [bigdata,visualization]
---
{% include JB/setup %}

# What is this?

Well.  During my
[presentation](http://stampedecon.com/sessions/interactive-visualization-in-human-time/)
at the big-data conference
[StampedeCon](http://stampedecon.com/), I noted that I
would have the references at the end of the presentation and that the
[slides](http://www.slideshare.net/StampedeCon/interactive-visualization-in-human-time-stampedecon-2015)
would be published after the conference.  And indeed, the slides include
the references and the slides were published.  w00t!

But.

The reference slide is fairly hard to read, and not all of the links
are working links in the slides.  So here is an annotated set of the
references.

Enjoy.

I put the references in slide deck order below, since that will be the
way you would naturally want to see the references.  Well.  Almost.
First I&apos;ll give the link to the GitHub repository where the scripts
that I used to create the charts I made for the presentation are.

#### Slides 5,6,9,10,11,13,16(partial),23,24,25 - Charts

These charts were created for the presentation using RStudio and the
scripts located on GitHub in the repository below.

* [https://github.com/oci-labs/StampedeCon-2015](https://github.com/oci-labs/StampedeCon-2015)

#### Slide 7 - Information Dense

It is hard to talk too much about Edward Tufte when talking about
visualization.  He is acknowledged as a leader in the field and the
book here is often cited when discussing visualization.  The chart
of Napolean&apos;s march on Moscow in 1812 that I used is described
in the book as "...may well be the best statistical graphic ever drawn".
High praise indeed.

* Edward R. Tufte, 2001

  The Visual Display of Quantitative Information

#### Slide 8 - Hidden Insights

On this slide, I included a diagram from an article that introduces
some of the interesting aspects of the new neural networking work that
is being done.  In this case it is an attempt to visualize what a neural
network has "learned" during training.  This is done by presenting noise
as input and observing what the network produces - with the presumption
that this will be what it "thinks" it has been trained to recognize.
In this case a network that was trained to recognized dumbbells shows
that it is recognizing dumbbells; but with arms attached!  Something
that would have been very difficult to determine without being able to
visualize this.

I&apos;ll also note here that while there was no attribution requirement
in the article at the time I accessed it. A notice was added since
that the images (not including those derived from MIT images) are
covered under the [Creative Commons Attribution 4.0 International
License](http://creativecommons.org/licenses/by/4.0/).

* Alexander Mordvintsev, Christopher Olah, Mike Tyka.

  Inceptionism: going deeper into Neural Networks. June 17, 2015

  [http://googleresearch.blogspot.co.uk/2015/06/inceptionism-going-deeper-into-neural.html](http://googleresearch.blogspot.co.uk/2015/06/inceptionism-going-deeper-into-neural.html)

#### Slide 13 - Brushing and Linking

The brushing and linking on this chart were indeed created using
RStudio and the script is captured in the GitHub repository above.
But the actual images were generated using the GGobi tool, an open
source visualization tool funded (until about 2010) by the National
Science Foundation.  This tool has an ancillary R package that allows
R data frames to be used as input data sets for the visualizations.
That is what I used to show the concept of brushing and linking using
one of the R data sets that the GGobi download included.

* [http://www.ggobi.org/](http://www.ggobi.org/)

#### Slide 14 - Response Times

On this slide, I included a screen capture of my own visualization.
The data was served using the open source _imMens_ big data visualization
tool which is then viewed on a browser.  This view matches those that are
included in the cited article.  The article itself presents the results of
research conducted by a team at the University of Washington Interactive
Data Lab.  The lab was moved from the original Stanford Visualization Lab
in 2013, which had produced such visualization tools as D3.js and Protovis
as well as the original Polaris which was commercialized as Tableau.

The article cited discusses the results that were published in the I.E.E.E.
Transactions on Visualization and Computer Graphics (Proc InfoVis) in 2014.
The _imMens_ tool is available from GitHub at the GitHub link below as well.

* Zhicheng Liu, Jeffery Heer

  Effects of latency on interactive visual analysis. August 12, 2014

  [http://istc-bigdata.org/index.php/effects-of-latency-on-interactive-visual-analysis/](http://istc-bigdata.org/index.php/effects-of-latency-on-interactive-visual-analysis/)

* [https://github.com/uwdata/imMens](https://github.com/uwdata/imMens)

#### Slide 29 - Covariance Aggregation

In this slide I include the formula that is presented in the Pébay paper
for covariance.  This is an efficient mechanism to implement sliding
windows and other aggregation strategies for large sets of data where
descriptive statistics are being generated.  The Apache HIVE project
includes a Java implementation of the moment and covariance generating
formulas.  I have implemented a C++ version of these formulas as well
and might put that up on GitHub if there is any interest.

* Pébay, Phillipe,

  "Formulas for Robust, One-Pass Parallel Computation of Covariances and Arbitrary-Order Statistical Moments", 2008

  [http://prod.sandia.gov/techlib/access-control.cgi/2008/086212.pdf](http://prod.sandia.gov/techlib/access-control.cgi/2008/086212.pdf)

* Java Implementation in Apache Hive as

  org.apache.hadoop.hive.ql.udf.generic.GenericUDAFCovariance

  [https://hive.apache.org/javadocs/r0.10.0/api/org/apache/hadoop/hive/ql/udf/generic/GenericUDAFCovariance.html](https://hive.apache.org/javadocs/r0.10.0/api/org/apache/hadoop/hive/ql/udf/generic/GenericUDAFCovariance.html)

At this point, all of the references from the presentation have been
included.  Hopefully you will find this helpful in exploring some of
the more interesting aspects of large data visualizations.

