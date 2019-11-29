---
title: "Further resources"
teaching: 5
exercises: 0
questions:
- "Where can I find more advanced information on these topics?"
- "What other Python packages exist with tools for more complicated analysis?"
objectives:
- "Find tutorials and documentation on Python tools (both generic and HEP-specific)."
keypoints:
- "Python and many Python packages have a huge userbase and are well supported by documentation, tutorials, and the community."
---

# Caveat

One important note that I did not address at all: The methods that I showed here work great for operating on one small-ish file (smaller than your system's RAM) at a time.
uproot has specialized methods and options that are better when running on many data files or larger files.
This is one of many topics covered in the uproot documentation below.

# Python in HEP

 - [Tutorial by Jim Pivarski, creator of uproot](https://github.com/jpivarski/2019-07-29-dpf-python)

# Scikit-HEP

 - [https://github.com/scikit-hep]()

## Uproot

 - [https://github.com/scikit-hep/uproot]()
 - [https://uproot.readthedocs.io]()

## Awkward-Array

 - [https://github.com/scikit-hep/awkward-array]()

## Other tools

 - [mplhep](https://github.com/scikit-hep/mplhep) (plotting utilities)
 - [pyhf](https://github.com/scikit-hep/pyhf) (histogram fitting)

# Standard packages

 - [numpy](https://numpy.org/devdocs/)
 - [matplotlib](https://matplotlib.org/contents.html)
 - [pandas](https://pandas.pydata.org/pandas-docs/stable/) (data frames)

{% include links.md %}
