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
[b'nMuon', b'Muon_pt', b'Muon_eta', b'Muon_phi', b'Muon_mass', b'Muon_charge']
~~~
{: .output}

The above output is a list of the branch names.
So we can see that for each event, we will have the number of muons in the event (`nMuon`) and the pT, eta, phi, mass, and charge of each muon.

But how do we get the actual data from the table? There are several ways with uproot, but the simplest is with the `arrays()` function:

~~~
tree.arrays()
~~~
{: .language-python}
~~~
{b'nMuon': array([2, 2, 1, ..., 3, 2, 3], dtype=uint32), b'Muon_pt': <JaggedArray [[10.763697 15.736523] [10.53849 16.327097] [3.2753265] ... [6.343258 6.9803934 5.0852466] [3.3099499 15.68049] [11.444268 3.082721 4.9692106]] at 0x7f948aedab80>, b'Muon_eta': <JaggedArray [[1.0668273 -0.5637865] [-0.42778006 0.34922507] [2.2108555] ... [-0.59949154 -0.0495256 -0.9052992] [1.6359439 0.4766063] [0.44437575 -1.6943384 0.7640488]] at 0x7f948aed5d60>, b'Muon_phi': <JaggedArray [[-0.034272723 2.5426154] [-0.2747921 2.5397813] [-1.2234136] ... [-2.9530895 0.26560423 -3.1138406] [0.87988055 -1.7524908] [-0.47927496 2.2850282 0.5623152]] at 0x7f948aed5d90>, b'Muon_mass': <JaggedArray [[0.10565837 0.10565837] [0.10565837 0.10565837] [0.10565837] ... [0.10565837 0.10565837 0.10565837] [0.10565837 0.10565837] [0.10565837 0.10565837 0.10565837]] at 0x7f948aed5970>, b'Muon_charge': <JaggedArray [[-1 -1] [1 -1] [1] ... [-1 1 1] [1 -1] [1 -1 1]] at 0x7f948aed5c70>}
~~~
{: .output}

You can see lots of numbers in there, which indeed are from the data in the tree.

> ## What is all this?
>
> We'll go through the structure of the data a bit later.
> What type of object is this whole thing, though?
> It's not explicitly stated in the output, but you may recognize it.
> In any case, we can check with the built-in Python function `type()`:
>
> ~~~
> type(tree.arrays())
> ~~~
> {: .language-python}
> ~~~
> <class 'dict'>
> ~~~
> {: .output}
>
> It's a `dict` (dictionary) object.
> `dict`s are very useful, but one thing that can be problematic here is that there's no easy way to access all of the information for just a single event with a `dict` like this.
> We'll deal with that issue later in the lesson.
{: .callout}

# Branches

Now we assign this object (which contains both the names and contents of the branches) to another variable (`branches`):

~~~
branches = tree.arrays(namedecode='utf-8')
~~~
{: .language-python}

> ## Why the 'utf-8'?
>
> I added a parameter to the `arrays()` function above, called `namedecode`, and I've set this parameter to the string `'utf-8'`.
> This is so that, when we deal with the `branches` variable, we can ignore the `b` before the names of the branches.
> Technically speaking, this means that all the `bytes` objects in `keys()` have been converted into normal strings by UTF-8 character decoding.
>
> To see what this means in practice, let's try something:
>
>~~~
>tree.arrays()['nMuon']
>~~~
>{: .language-python}
>~~~
>Traceback (most recent call last):
>  File "<stdin>", line 1, in <module>
>KeyError: 'nMuon'
>~~~
>{: .output}
>
> What?
> `KeyError` means that Python is complaining that 'nMuon' is not a key in `tree`.
> But we know that 'nMuon' is the name of one of the branches.
> The problem is Python sees that the string 'nMuon' is not of the same type as the `bytes` object `b'nMuon'` (even though the data bits of the two are identical), and this makes it treat the two as unequal.
> If we *didn't* include `namedecode='utf-8'` in `arrays()`, then we would always have to write the `b`, like this:
>
>~~~
>tree.arrays()[b'nMuon']
>~~~
>{: .language-python}
>
> This would be really annoying, so we avoid it.
> We can verify that the `b`s are gone in `branches` by running `keys()` on it:
>
> ~~~
> branches.keys()
> ~~~
> {: .language-python}
> ~~~
> dict_keys(['nMuon', 'Muon_pt', 'Muon_eta', 'Muon_phi', 'Muon_mass', 'Muon_charge'])
> ~~~
> {: .output}
>
> You can ignore `dict_keys`, it has no effect here.
> Now (thankfully) we won't see any `bytes` objects again in this tutorial.
{: .callout}

Next let's just look at each branch individually.
You can access a single branch from `branches` in a similar way to getting an item from a ROOT file object (array-like notation):
~~~
branches['nMuon']
~~~
{: .language-python}
~~~
array([2, 2, 1, ..., 3, 2, 3], dtype=uint32)
~~~
{: .output}

You can see the partial list of numbers in the output that represents the number of muons in each event.
It's abbreviated with an ellipsis (`...`) so that it doesn't take up the whole page.

> ## More detail on the branch object
>
> The output says `array`, but the actual type of the branch is an `ndarray`, which is just a numpy array:
>
> ~~~
> type(branches['nMuon'])
> ~~~
> {: .language-python}
> ~~~
> <class 'numpy.ndarray'>
> ~~~
> {: .output}
>
> The `dtype=uint32` means that the **d**ata**type** of each array entry is a 32-bit unsigned integer.
{: .callout}

The rest of the branches are all a special uproot type:

~~~
type(branches['Muon_pt'])
~~~
{: .language-python}
~~~
<class 'awkward.array.jagged.JaggedArray'>
~~~
{: .output}

This must be a *jagged array* because the number of entries is different for different events (because each event can have a different number of muons).
We can still look at the content in the same way, though:

~~~
branches['Muon_pt']
~~~
{: .language-python}
~~~
<JaggedArray [[10.763697 15.736523] [10.53849 16.327097] [3.2753265] ... [6.343258 6.9803934 5.0852466] [3.3099499 15.68049] [11.444268 3.082721 4.9692106]] at 0x(hexadecimal number)>
~~~
{: .output}

Note that there are square brackets `[]` surrounding the list of entries for each event.
This is basically an array of arrays (or a 2D array).

## Events

If we want to focus on a particular event, we can index it just like a normal array:

~~~
branches['Muon_pt'][0]
~~~
{: .language-python}
~~~
array([10.763697, 15.736523], dtype=float32)
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
array([3.2753265], dtype=float32)
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
> > [10.763697 15.736523]
> > [10.53849  16.327097]
> > [3.2753265]
> > [11.429154  17.634033   9.624728   3.5022252]
> > [ 3.2834418  3.6440058 32.911224  23.721754 ]
> > [3.566528 4.572504 4.371863]
> > [57.6067  53.04508]
> > [11.319675 23.906353]
> > [10.193569 14.204061]
> > [11.470704   3.4690065]
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Earlier I mentioned that the `branches` object that we have is not convenient for getting information from all branches at once for one particular event.
That is, it would be nice if we could do something like
~~~
branches[0]
~~~
{: .language-python}

and get all muon information about the first event.
However, the above command results in an error.
We can get this kind of functionality, but we have to move to a slightly different type of object--a `Table`.
`Table` is a class in the `awkward` package that uproot uses internally to handle jagged arrays.
We can import `awkward` and then create a `Table` from our `branches` object:

~~~
import awkward
table = awkward.Table(branches)
~~~
{: .language-python}

Let's try again, now with `table`:

~~~
table[0]
~~~
{: .language-python}
~~~
<Row 0>
~~~
{: .output}

Well, there's no error, so that's good.
The return type is `Row`, which is another class from `awkward`.
Unfortunately, the output representation of a `Row` is not helpful at all for understanding what's in it.
We can access the information inside by selecting out an individual column (branch):

~~~
table[0]['nMuon']
~~~
{: .language-python}
~~~
2
~~~
{: .output}

This is the number of muons in the first event, as we've already seen.
It's a bit more work and not so obvious how to get all of the information out:

~~~
for column_name in table[0]:
    print(column_name, '=', table[0][column_name])
~~~
{: .language-python}
~~~
nMuon = 2
Muon_pt = [10.763697 15.736523]
Muon_eta = [ 1.0668273 -0.5637865]
Muon_phi = [-0.03427272  2.5426154 ]
Muon_mass = [0.10565837 0.10565837]
Muon_charge = [-1 -1]

~~~
{: .output}

There we go. Now we finally see the whole picture for an individual event.

{% include links.md %}
