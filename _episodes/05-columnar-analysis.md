---
title: "Columnar Analysis"
teaching: 0
exercises: 0
questions:
- "What is columnar analysis?"
- "What are its advantages over row-based analysis?"
- "How do I perform selections and operations on columns?"
objectives:
- "First objective. (FIXME)"
keypoints:
- "First key point. (FIXME)"
---

~~~
len(tree)
len(table)
len(branches['nMuon'])
len(branches['Muon_pt']) # or any of the other branches...
~~~
{: .language-python}
~~~
100000
~~~
{: .output}

Note that this is *not* the total number of muons!

~~~
len(branches['Muon_pt'].flatten())
~~~
{: .language-python}
~~~
235286
~~~
{: .output}

~~~
single_muon_mask = branches['nMuon'] == 1
~~~
{: .language-python}

~~~
single_muon_mask
~~~
{: .language-python}
~~~
array([False, False,  True, ..., False, False, False])
~~~
{: .output}

~~~
single_muon_mask.sum()
~~~
{: .language-python}
~~~
13447
~~~
{: .output}

~~~
branches['Muon_pt'][single_muon_mask]
~~~
{: .language-python}
~~~
<JaggedArray [[3.2753265] [3.837803] [16.145521] ... [8.18912] [13.272973] [9.484549]] at 0x(hexadecimal number)>
~~~
{: .output}

~~~
len(branches['Muon_pt'][single_muon_mask])
~~~
{: .language-python}
~~~
13447
~~~
{: .output}

broadcasting

~~~
plt.hist(branches['Muon_pt'][single_muon_mask].flatten(), bins=100, range=(0, 100))
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.ylabel('Number of single muons / 1 MeV')
plt.yscale('log')
plt.show()
~~~
{: .language-python}

~~~
eta_mask = abs(branches['Muon_eta']) < 2
~~~
{: .language-python}

~~~
eta_mask
~~~
{: .language-python}
~~~
<JaggedArray [[True True] [True True] [False] ... [True True True] [True True] [True True True]] at 0x(hexadecimal number)>
~~~
{: .output}

~~~
eta_mask.flatten().sum()
~~~
{: .language-python}
~~~
204564
~~~
{: .output}

~~~
plt.hist(branches['Muon_eta'].flatten(), bins=50, range=(-2.5, 2.5))
plt.title('No selection')
plt.xlabel('Muon $\eta$')
plt.ylabel('Number of muons')
plt.show()

plt.hist(branches['Muon_eta'][eta_mask].flatten(), bins=50, range=(-2.5, 2.5))
plt.title('With $|\eta| < 2$ selection')
plt.xlabel('Muon $\eta$')
plt.ylabel('Number of muons')
plt.show()
~~~
{: .language-python}

![Muon_eta_hist_1]({{ page.root }}/fig/Muon_eta_hist_1.png)

![Muon_eta_hist_2]({{ page.root }}/fig/Muon_eta_hist_2.png)


~~~
~single_muon_mask
~~~
{: .language-python}
~~~
array([ True,  True, False, ...,  True,  True,  True])
~~~
{: .output}

~~~
single_muon_mask & eta_mask
~~~
{: .language-python}
~~~
<JaggedArray [[False False] [False False] [False] ... [False False False] [False False] [False False False]] at 0x(hexadecimal number)>
~~~
{: .output}

~~~
single_muon_mask | eta_mask
~~~
{: .language-python}
~~~
<JaggedArray [[False True] [True True] [True] ... [True True True] [False True] [True False True]] at 0x(hexadecimal number)>
~~~
{: .output}

~~~
False == False & False
~~~
{: .language-python}
~~~
True
~~~
{: .output}

~~~
False == (False & False)
~~~
{: .language-python}
~~~
True
~~~
{: .output}

~~~
(False == False) & False
~~~
{: .language-python}
~~~
False
~~~
{: .output}

~~~
(branches['nMuon'] == 1) & (abs(branches['Muon_eta']) < 2)
~~~
{: .language-python}

~~~
plt.hist((branches['Muon_pt'][single_muon_mask & eta_mask].flatten(),
          branches['Muon_pt'][single_muon_mask & ~eta_mask].flatten()),
         bins=25, range=(0, 50))
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.ylabel('Number of single muons / 1 MeV')
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_eta_split_1]({{ page.root }}/fig/Muon_pt_hist_eta_split_1.png)

~~~
plt.hist([branches['Muon_pt'][single_muon_mask & eta_mask].flatten(),
          branches['Muon_pt'][single_muon_mask & ~eta_mask].flatten()],
         label=['$|\eta| < 2$', '$|\eta| \geq 2$'],
         bins=25, range=(0, 50))
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.ylabel('Number of single muons / 1 MeV')
plt.legend()
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_eta_split_2]({{ page.root }}/fig/Muon_pt_hist_eta_split_2.png)

~~~
plt.hist((branches['Muon_pt'][single_muon_mask & eta_mask].flatten(),
          branches['Muon_pt'][single_muon_mask & ~eta_mask].flatten()),
         label=['$|\eta| < 2$', '$|\eta| \geq 2$'],
         bins=25, range=(0, 50), density=True)
plt.title('Normalized')
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.ylabel('Number of single muons / 1 MeV')
plt.legend()
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_eta_split_3]({{ page.root }}/fig/Muon_pt_hist_eta_split_3.png)

~~~
%%time

eta_count = 0

for event in branches['Muon_eta']:
    for eta in event:
        if eta < 2:
            eta_count += 1
            
eta_count
~~~
{: .language-python}
~~~
CPU times: user 1.31 s, sys: 3.27 ms, total: 1.32 s
Wall time: 1.32 s

219321
~~~
{: .output}

~~~
%%time

(branches['Muon_eta'] < 2).flatten().sum()
~~~
{: .language-python}
~~~
CPU times: user 4.65 ms, sys: 3.24 ms, total: 7.89 ms
Wall time: 6.41 ms

219321
~~~
{: .output}

{% include links.md %}
