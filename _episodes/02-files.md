---
title: "Opening files"
teaching: 20
exercises: 0
questions:
 - "How do I open a ROOT file with Uproot?"
 - "How can I tell what's in the file?"
 - "How do I access the contents?"
objectives:
 - "Open a ROOT file and list its contents."
 - "Access objects within a ROOT file."
keypoints:
 - "Opening and navigating files in Uproot isn't that different from doing so in ROOT."
 - "Use `.keys()` to see the contents of a file."
 - "Use the form `file['key']` to access an object inside a file."
---

# Example motivation

In order to learn how to use Uproot, we'll try to do a very short and simple analysis to look for resonances in a dimuon events.
The example ROOT file is from real CMS data during proton-proton collisions in 2012.
Make sure you've downloaded it from the [Setup]({{ page.root }}/setup.html) page and put it in your working directory.
Our first goal is just to get to the data within Uproot, so we need to open the file and navigate to the muon information.

# Opening a file

The first thing you must do whenever you want to use Uproot is import it, just like any other Python module:

~~~
import uproot
~~~
{: .language-python}

> ## Import errors?
>
> If you get something like:
> 
> ~~~
> Traceback (most recent call last):
>   File "<stdin>", line 1, in <module>
> ModuleNotFoundError: No module named 'uproot'
> ~~~
> {: .output}
> 
> then this means Uproot hasn't been installed properly.
> Check the [Setup]({{ page.root }}/setup.html) page for detailed instructions.
> If you're using `conda` (like Anaconda or Miniconda),
> make sure you've activated the same environment where you installed Uproot.
{: .callout}

Now open the ROOT file and assign it to a variable (which I've named `file` here):

~~~
file = uproot.open('uproot-tutorial-file.root')
~~~
{: .language-python}

If you inspect `file`, you can see that it's a `ReadOnlyDirectory` object:

~~~
file
~~~
{: .language-python}
~~~
<ReadOnlyDirectory '/' at 0x(some hexadecimal number here)>
~~~
{: .output}

# File contents

Just like any other kind of directory, you can list the contents (of the file).
The name of each item in the file is called a "key".

~~~
file.keys()
~~~
{: .language-python}
~~~
['Events;1']
~~~
{: .output}

We can see that there is one key: "Events".
This doesn't tell us what kind of object it refers to, though.
ROOT files can contain many different types of objects, including subdirectories.
The following function provides a way to inspect the types of each item:

~~~
file.classnames()
~~~
{: .language-python}
~~~
{'Events;1': 'TTree'}
~~~
{: .output}

The output contains pairs of the form `name: type`.
Therefore the key `Events` refers to a TTree object.
This is where all the data in this file is stored.

> ## Why the `;1`?
>
> You may be wondering why there's a `;1` after "Events".
> This notation refers to the *cycle number*, which is a detail of the ROOT file format that we don't need to care about.
> This `;1` shows up if you open the file in ROOT itself as well.
{: .callout}

# Accessing contents

Now we want to actually access the object inside the file.
You can do this just as you would to get an item in an array:

~~~
file['Events']
~~~
{: .language-python}
~~~
<TTree 'Events' (6 branches) at 0x(hexadecimal number)>
~~~
{: .output}

This expression refers to the actual TTree object, which we will look at next.

{% include links.md %}
