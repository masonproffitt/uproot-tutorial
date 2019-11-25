---
title: "Opening files"
teaching: 0
exercises: 0
questions:
- "How do I open a ROOT file with uproot?"
- "How can I tell what's in the file?"
objectives:
- "First objective. (FIXME)"
keypoints:
- "First key point. (FIXME)"
---

The first thing you must do whenever you want to use `uproot` is to import it,
just like any other Python module:

~~~
import uproot
~~~
{: .language-python}

If you get something like:

~~~
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'uproot'
~~~
{: .output}

then this means `uproot` hasn't been installed properly.
If you're using conda (like Anaconda or Miniconda),
make sure you've activated the same environment where you installed `uproot`.

Now open the ROOT file and assign it to variable (which I've named `file` here):

~~~
file = uproot.open('uproot-tutorial-file.root')
~~~
{: .language-python}

If you inspect `file`, you can see that it's a `ROOTDirectory` object:

~~~
file
~~~
{: .language-python}
~~~
<ROOTDirectory b'uproot-tutorial-file.root' at 0x(some hexadecimal number here)>
~~~
{: .output}

Just like any other kind of directory, you can list the contents (of the file):
~~~
file.keys()
~~~
{: .language-python}
~~~
[b'Events;1']
~~~
{: .output}

~~~
file.classnames()
~~~
{: .language-python}
~~~
[(b'Events;1', 'TTree')]
~~~
{: .output}

~~~
file.get('Events')
~~~
{: .language-python}
~~~
<TTree b'Events' at 0x(hexadecimal number)>
~~~
{: .output}

~~~
file['Events']
~~~
{: .language-python}
~~~
<TTree b'Events' at 0x(hexadecimal number)>
~~~
{: .output}

{% include links.md %}
