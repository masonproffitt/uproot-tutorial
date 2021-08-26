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
Uproot has specialized methods and options that are better when running on many data files or larger files.
This is one of many topics covered in the Uproot documentation below.

# Scikit-HEP project

- <https://github.com/scikit-hep>

## Uproot

- [GitHub repository](https://github.com/scikit-hep/uproot4)
- [Official documentation](https://uproot.readthedocs.io)

## Awkward Array

- [GitHub repository](https://github.com/scikit-hep/awkward-1.0)
- [Official documentation](https://awkward-array.org/)

## Vector

- [GitHub repository](https://github.com/scikit-hep/vector)
- [Official documentation](https://vector.readthedocs.io)

## Other tools

- [Hist](https://github.com/scikit-hep/hist) (creating histograms)
- [`pyhf`](https://github.com/scikit-hep/pyhf) (histogram fitting and likelihoods)
- [`mplhep`](https://github.com/scikit-hep/mplhep) (plotting utilities)

# Standard packages

- [NumPy](https://numpy.org/devdocs/)
- [Matplotlib](https://matplotlib.org/contents.html)
- [SciPy](https://docs.scipy.org/doc/scipy/index.html)
- [pandas](https://pandas.pydata.org/pandas-docs/stable/) (data frames)

{% include links.md %}
