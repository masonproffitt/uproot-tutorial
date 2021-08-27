---
title: "Trees, Branches, and Events"
teaching: 20
exercises: 5
questions:
- "How do I access a TTree?"
- "How can I tell what branches are in a TTree?"
- "How do I read the data from a TTree?"
objectives:
- "List the branches in a tree."
- "Access the branches in a tree."
- "Create a table from tree branches."
- "Access data for a particular event."
keypoints:
- "TTrees are tables of data."
- "Trees are made of branches, which are columns in the table."
- "Each row represents an event."
---

# Trees

Trees in ROOT are basically just tables of information.
Trees are composed of branches, which are the columns of the table.
The rows usually represent events (individual bunch crossings).

First we assign the tree to a variable (named `tree` here)

~~~
tree = file['Events']
~~~
{: .language-python}

In order to find out what information is in the tree, we need to know what the branches (columns) are.
The term `key` is used (again) here to refer to the names of the branches.

~~~
tree.keys()
~~~
{: .language-python}
~~~
['nMuon', 'Muon_pt', 'Muon_eta', 'Muon_phi', 'Muon_mass', 'Muon_charge']
~~~
{: .output}

The above output is a list of the branch names.
So we can see that for each event, we will have the number of muons in the event (`nMuon`) and the pT, eta, phi, mass, and charge of each muon.

But how do we get the actual data from the table? There are several ways with Uproot, but the simplest is with the `arrays()` function:

~~~
tree.arrays()
~~~
{: .language-python}
~~~
<Array [{nMuon: 2, Muon_pt: [10.8, ... -1, 1]}] type='100000 * {"nMuon": uint32,...'>
~~~
{: .output}

You can see some numbers in there, which indeed are from the data in the tree.

# Branches

Now we assign this object (which contains both the names and contents of the branches) to another variable (`branches`):

~~~
branches = tree.arrays()
~~~
{: .language-python}

Next let's just look at each branch individually.
You can access a single branch from `branches` in a similar way to getting an item from a ROOT file object (array-like notation):

~~~
branches['nMuon']
~~~
{: .language-python}
~~~
<Array [2, 2, 1, 4, 4, 3, ... 0, 3, 2, 3, 2, 3] type='100000 * uint32'>
~~~
{: .output}

You can see the partial list of numbers in the output that represents the number of muons in each event.
It's abbreviated with an ellipsis (`...`) so that it doesn't take up the whole page.

> ## Error?
>
> If you get something like:
>
> ~~~
> Traceback (most recent call last):
>   File "<stdin>", line 1, in <module>
> KeyError: 'nMuon'
> ~~~
> {: .output}
>
> then you are almost certainly using an older version of Uproot that is not compatible with the rest of this tutorial.
> If this is the case, install the latest version of Uproot, restart the notebook's kernel, and try again.
{: .callout}

These `Array` objects are a special type provided by the Awkward Array package.
The `type=100000 * uint32` means that there are 100,000 entries and that each entry is a 32-bit unsigned integer. Each entry corresponds to one event.

Let's look at another branch:

~~~
branches['Muon_pt']
~~~
{: .language-python}
~~~
<Array [[10.8, 15.7], ... 11.4, 3.08, 4.97]] type='100000 * var * float32'>
~~~
{: .output}

This is a *jagged array* because the number of entries is different for different events (because each event can have a different number of muons).
Note that there are square brackets `[]` surrounding the list of entries for each event.
The `type='100000 * var * float32'` means that there are 100,000 rows, each containing a **var**iable number of 32-bit floating point numbers.
This is basically an array of arrays (or a 2D array).

## Events

If we want to focus on a particular event, we can index it just like a normal array:

~~~
branches['Muon_pt'][0]
~~~
{: .language-python}
~~~
<Array [10.8, 15.7] type='2 * float32'>
~~~
{: .output}

From the above output, the first event has two muons, and the two numbers in the list are the muons' pT.
It's not specified anywhere in the file, but the units are GeV.
Let's look at the third event:

~~~
branches['Muon_pt'][2]
~~~
{: .language-python}
~~~
<Array [3.28] type='1 * float32'>
~~~
{: .output}

It only has one muon.

> ## Exercise
>
> Print out the pT of all muons that are in only the first 10 events.
> (There are many possible ways to do this.)
>
> > ## Solution
> >
> > Here's one way to do it.
> > All that matters is that you get the same numbers (and number of numbers) in the output
> >
> > ~~~
> > for i in range(10):
> >     print(branches['Muon_pt'][i])
> > ~~~
> > {: .language-python}
> > ~~~
> > [10.8, 15.7]
> > [10.5, 16.3]
> > [3.28]
> > [11.4, 17.6, 9.62, 3.5]
> > [3.28, 3.64, 32.9, 23.7]
> > [3.57, 4.57, 4.37]
> > [57.6, 53]
> > [11.3, 23.9]
> > [10.2, 14.2]
> > [11.5, 3.47]
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

What if we want to get all of the information about a single event?
So far we've accessed data in `branches` by providing a branch name, but we can also just use an event index:

~~~
branches[0]
~~~
{: .language-python}
~~~
<Record ... 0.106], Muon_charge: [-1, -1]} type='{"nMuon": uint32, "Muon_pt": va...'>
~~~
{: .output}

This is a `Record` object, which is another special type provided by Awkward Array.
It functions basically the same way as a standard Python dictionary (`dict`).
Unfortunately, most of the interesting information is still hidden in the above output to save space.
A little trick we can use to force printing all the data is adding `.tolist()`:

~~~
branches[0].tolist()
~~~
{: .language-python}
~~~
{'nMuon': 2,
 'Muon_pt': [10.763696670532227, 15.736522674560547],
 'Muon_eta': [1.0668272972106934, -0.563786506652832],
 'Muon_phi': [-0.03427272289991379, 2.5426154136657715],
 'Muon_mass': [0.10565836727619171, 0.10565836727619171],
 'Muon_charge': [-1, -1]}
~~~
{: .output}

There we go. Now we can see the whole picture for an individual event.

> ## `.tolist()`
>
> `.tolist()` is a NumPy function that has been extended to Awkward Array objects.
> As the name suggests, it converts NumPy arrays to Python lists.
> In the case of trees, which have named branches, it actually converts to a dictionary of lists.
> It can be very useful when you want to understand exactly what's in an `Array` or `Record`.
> Be careful when using it, though--trying to print out an entire branch or tree could cause Python to crash if it's large enough.
> Therefore it's best to only use `tolist()` on one or a few events at a time to be safe.
{: .callout}

{% include links.md %}
