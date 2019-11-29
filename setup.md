---
title: Setup
---

This tutorial requires Jupyter, Python, uproot (of course), matplotlib, and scipy (for the optional fitting section).

# Getting uproot and Jupyter

There are many ways of running the software necessary for this tutorial.
I've put three common options in the sections below (you only need to use one).
For ease of debugging, I recommend using Binder unless you already have another option up and running, but feel free to use whatever gets you to a Jupyter notebook with uproot, matplotlib, and scipy installed.

## Binder

Simply follow [this link to Binder](https://mybinder.org/v2/gh/masonproffitt/uproot-tutorial-notebooks/master).
It should open up Jupyter with all necessary packages and files installed.

## SWAN

You must have a CERN account for this option.
Go to [swan.cern.ch]().
For the "software stack" in the configuration settings, just make sure it has "Python 3" in the name.
If you haven't done so before, you'll need to run `pip install --user uproot` in a Jupyter notebook and then restart the kernel (Kernel->Restart).

## Anaconda (local installation)

The easiest way to run everything locally is through [Anaconda](https://www.anaconda.com/distribution/).
Download it and follow the instructions. 
Once you're in the Anaconda environment, create a new environment with uproot installed by running
`conda create -n uproot uproot`
and then activate that environment by running
`conda activate uproot`.
(After the first time doing this, you only have to run the last command within Anaconda when you want to use uproot again.)
Finally, run `jupyter notebook`.
A browser window should pop up with Jupyter open.

# Required data file

You will need to download a small (3.5 MiB)
[CMS open data file](https://github.com/masonproffitt/uproot-tutorial-notebooks/raw/master/uproot-tutorial-file.root)
as well. (This is already done for you if you're running on Binder).

# Once you have Jupyter open...

Just open a new Python 3 notebook.
Whenever you see Python code in the tutorial, you can type it in and run it (Shift-Enter).
The exercises are done in the notebook.
You are encouraged to experiment far beyond what's in the tutorial whenever you have a question or are just curious about something!

{% include links.md %}
