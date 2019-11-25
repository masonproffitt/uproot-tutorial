---
title: "Trees, Branches, and Events"
teaching: 0
exercises: 0
questions:
- "How do I access a TTree?"
- "How can I tell what branches are in a TTree?"
- "How do I read the data from a TTree?"
objectives:
- "First objective. (FIXME)"
keypoints:
- "First key point. (FIXME)"
---

~~~
tree = file['Events']
~~~
{: .language-python}

~~~
tree.keys()
~~~
{: .language-python}
~~~
[b'nMuon', b'Muon_pt', b'Muon_eta', b'Muon_phi', b'Muon_mass', b'Muon_charge']
~~~
{: .output}

~~~
tree.arrays()
~~~
{: .language-python}
~~~
{b'nMuon': array([2, 2, 1, ..., 3, 2, 3], dtype=uint32), b'Muon_pt': <JaggedArray [[10.763697 15.736523] [10.53849 16.327097] [3.2753265] ... [6.343258 6.9803934 5.0852466] [3.3099499 15.68049] [11.444268 3.082721 4.9692106]] at 0x7f948aedab80>, b'Muon_eta': <JaggedArray [[1.0668273 -0.5637865] [-0.42778006 0.34922507] [2.2108555] ... [-0.59949154 -0.0495256 -0.9052992] [1.6359439 0.4766063] [0.44437575 -1.6943384 0.7640488]] at 0x7f948aed5d60>, b'Muon_phi': <JaggedArray [[-0.034272723 2.5426154] [-0.2747921 2.5397813] [-1.2234136] ... [-2.9530895 0.26560423 -3.1138406] [0.87988055 -1.7524908] [-0.47927496 2.2850282 0.5623152]] at 0x7f948aed5d90>, b'Muon_mass': <JaggedArray [[0.10565837 0.10565837] [0.10565837 0.10565837] [0.10565837] ... [0.10565837 0.10565837 0.10565837] [0.10565837 0.10565837] [0.10565837 0.10565837 0.10565837]] at 0x7f948aed5970>, b'Muon_charge': <JaggedArray [[-1 -1] [1 -1] [1] ... [-1 1 1] [1 -1] [1 -1 1]] at 0x7f948aed5c70>}
~~~
{: .output}

~~~
type(tree.arrays())
~~~
{: .language-python}
~~~
<class 'dict'>
~~~
{: .output}

~~~
branches = tree.arrays(namedecode='utf-8')
~~~
{: .language-python}

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
>~~~
>tree.arrays()[b'nMuon']
>~~~
>{: .language-python}

~~~
branches.keys()
~~~
{: .language-python}
~~~
dict_keys(['nMuon', 'Muon_pt', 'Muon_eta', 'Muon_phi', 'Muon_mass', 'Muon_charge'])
~~~
{: .output}

~~~
branches['nMuon']
~~~
{: .language-python}
~~~
array([2, 2, 1, ..., 3, 2, 3], dtype=uint32)
~~~
{: .output}

~~~
type(branches['nMuon'])
~~~
{: .language-python}
~~~
<class 'numpy.ndarray'>
~~~
{: .output}

~~~
type(branches['Muon_pt'])
~~~
{: .language-python}
~~~
<class 'awkward.array.jagged.JaggedArray'>
~~~
{: .output}

~~~
branches['Muon_pt']
~~~
{: .language-python}
~~~
<JaggedArray [[10.763697 15.736523] [10.53849 16.327097] [3.2753265] ... [6.343258 6.9803934 5.0852466] [3.3099499 15.68049] [11.444268 3.082721 4.9692106]] at 0x(hexadecimal number)>
~~~
{: .output}

~~~
branches['Muon_pt'][0]
~~~
{: .language-python}
~~~
array([10.763697, 15.736523], dtype=float32)
~~~
{: .output}

~~~
branches['Muon_pt'][2]
~~~
{: .language-python}
~~~
array([3.2753265], dtype=float32)
~~~
{: .output}

~~~
for i in range(10):
    print(branches['Muon_pt'][i])
~~~
{: .language-python}
~~~
[10.763697 15.736523]
[10.53849  16.327097]
[3.2753265]
[11.429154  17.634033   9.624728   3.5022252]
[ 3.2834418  3.6440058 32.911224  23.721754 ]
[3.566528 4.572504 4.371863]
[57.6067  53.04508]
[11.319675 23.906353]
[10.193569 14.204061]
[11.470704   3.4690065]
~~~
{: .output}

~~~
import awkward
table = awkward.Table(branches)
~~~
{: .language-python}

~~~
first_event = table[0]
~~~
{: .language-python}

~~~
type(first_event)
~~~
{: .language-python}
~~~
<class 'awkward.array.table.Table.Row'>
~~~
{: .output}

~~~
table[0]['nMuon']
~~~
{: .language-python}
~~~
2
~~~
{: .output}

~~~
for column_name in first_event:
    print(column_name, '=', first_event[column_name])
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

{% include links.md %}
