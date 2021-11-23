# coefplot
Stata module for plotting regression coefficients and other results

`coefplot` plots results from estimation commands or Stata matrices. Results
from multiple models or matrices can be combined in a single graph. The default
behavior of `coefplot` is to draw markers for coefficients and horizontal
spikes for confidence intervals. However, `coefplot` can also produce various
other types of graphs.

To install the `coefplot` package from the SSC Archive, type

    . ssc install coefplot, replace

in Stata. Stata version 11 or newer is required.

---

Installation from GitHub:

    . net install coefplot, replace from(https://raw.githubusercontent.com/benjann/coefplot/master/)

---

Main changes:

    23nov2021 (1.8.5)
    - -set dp comma- could break -coefplot-; this is fixed (hopefully)
    
    17dec2020 (1.8.4)
    - automatic offsets are no longer added to the plot positions of the 
      estimates if multiple series are plotted but the coefficients of the series
      are distinct (such that there is at most one estimate per coefficient)
    - an "invalid attempt to modify label" error was returned when applying option 
      -generate()- without -replace- in a situation in which value label definitions
      from an earlier call to -coefplot, generate()- were still in memory, even
      though the corresponding variables have been dropped; -coefplot- is now less
      strict; it now assumes that it is ok to overwrite existing value labels if a
      variable with the same name could successfully be created

    17apr2020
    - installation files added to GitHub distribution
    
    07mar2019 (1.8.3)
    - play(), xoverhangs, and yoverhangs did not work in case of multiple plots;
      this is fixed
  
    22feb2019 (1.8.2)
    - cirecast() can now be used as shorthand notation for ciopts(recast()) 
    - citype() can now has additional methods: probit, atanh, and log
    - offset() can now contain expressions, e.g. offset(1/7)
    - model options from p#() were only applied in the first relevant subgraph and
      were omitted in subsequent subgraphs; this is fixed
    - computation of CIs failed if degrees of freedom were larger than 2e17
      (can happen with -mi estimate-); this is fixed

    18sep2017 (1.8.1)
    - mlabel() now supports string expressions to generate complicated marker labels
    - the backslash is no longer required between models when merging multiple 
      models into one series (as long as the same options are to be used for all
      models); that is, you can now type "(m1 m2 m3, opts)" instead of 
      "(m1 \ m2 \ m3, opts)"
    - noeqlabels did not remove the gap between equations; this is fixed; if you 
      want to keep the gap, type noeqlabels together with eqlabels(, gap(1))
    - eform(), rescale() and transform() returned error if no match was found
      in a model that had only one coefficient; this is fixed

    14feb2017 (1.8.0)
    - options p1(), p2(), etc. are now allowed to specify plot options
    - wildcard characters (*, ?) can now be used in model names
    - eform() and rescale() broke computation of t and p values; this is fixed

    02feb2017 (1.7.8)
    - rename() and eqrename() now have a regex suboption

    24jan2017 (1.7.7)
    - new sort() option
    - new orderby() option
    - paired-coordinate plots are now supported in ciopts(recast()); in addition to
      the official types, plot type -pcrarrow- is provided for reverse arrows
    - cismooth() now has suboptions color() and pstyle()
    - p-values are now available internal variable @pval; t (or z) statistics as @t
    - shorthand syntax se(#), ci((# #) ...), aux(# ...), and at(#) is now allowed 
      when plotting results from a matrix
    - df(.) is now allowed (set the DFs to missing)
    - twoway option pcycle() is now supported (default is set to 15)
    - coeflabels() did not support the angle() suboption; this is fixed
    - eqlabels() now has suboption -labels- to use variable labels
    - eqrename() now also affects equation names set by asequation()
    - mlabels() could crash if there were no matches; this is fixed
    - addplot(, below) now no longer messes up the legend; options nodraw, draw, 
      by(), name(), saving(), play() are no longer allowed within addplot()
    - changed behavior of rename()/eqrename(): rename(* = str) will now add str as
      a suffix, not as a prefix; this is useful, e.g., for plotting results from
      -proportion- or -mean, over()-
    - coefficients starting with "." caused error when trying to retrieve the 
      labels; this is fixed

    29jan2015 (1.7.5)
    - coefplot accidentally left behind extra observations in the dataset if the 
      existing number of observations was lower than the number of observations 
      required to hold the results plotted by coefplot; this is fixed

    08dec2014 (1.7.4)
    - fysize() and fxsize() did not work in case of multiple plots; this is 
      fixed
    - there was a small chance that coefplot left behind a mata object if the 
      user pressed break; this is fixed
    - option -nodrop- added
    - now uses "equation" as plot label instead of ".=equation" if model is .
    - twoway plot options such as msymbol() were not correctly passed through
      to the plots if specified at the subgraph level; this is fixed
    - collecting plot options across subgraphs has been improved (not relevant 
      if norecycle is specified); the rules are: (1) plot options specified at
      a lower level take precedence over later plot options specified at an 
      upper level. (2) within level, the rightmost option is used.
    - coefplot now picks up results from e(b_mi) and e(V_mi) by default if 
      plotting results from -mi estimate-
    - transform() suboption added to at()
    - interaction() suboption added to coeflabels()
    - plot options key() and nokey added
    - replace option could fail due to conflicts with existing labels; this is 
      fixed

    05jun2014 (1.7.1)
    - grid() now allows suboptions

    15may2014 (1.7.0)
    - internal variables are now accessed as @b, @ll, @ul, ...
      (syntax \`...' is discontinued)
    - variances, SEs and DFs now available as internal variables (@V, @se, @df)
    - mlabels() option added
    - transform() option added
    - aux() option added
    - weight() option added
    - asequation now allowed without argument
    - default prefix for generate() is now "__"
    - CIs are now stored in wide format (i.e. as separate variables)

    26feb2014 (1.6.5)
    - if() option added

    29jan2014 (version 1.6.1)
    - citype(logit|normal) added
    - ci() now uses syntax ci((ll ul) ...)
      ci("ll ul" ...) is still supported but undocumented
    - option swapnames added
    - options at(_coef) and at(_eq) added

    23nov2013 (version 1.5.8)
    - improved recovery of plot positions from e(at) for margins
    - syntax at(matrix(...)) now possible
    - axis() option added
    - new suboptions in eqlabels(), groups(), headings(): 
      gap(), wrap(), truncate(), nobreak 
    - plotlabels() and bylabels() options added

    28aug2013 (version 1.5.4)
    - cismooth() added; cishade removed
    - problem with too many ciopts() fixed

    26aug2013 (version 1.5.2)
    - pstyles are now sensitive to plot types

    25aug2013 (version 1.5.1)
    - coefplot released on SSC