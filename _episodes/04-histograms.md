---
title: "Histograms"
teaching: 0
exercises: 0
questions:
- "How do I make a histogram in Python (without ROOT)?"
- "How can I change histogram settings?"
objectives:
- "First objective. (FIXME)"
keypoints:
- "First key point. (FIXME)"
---

~~~
import matplotlib.pyplot as plt
~~~
{: .language-python}

~~~
plt.hist(branches['nMuon'])
~~~
{: .language-python}
~~~
(array([8.7359e+04, 1.2253e+04, 3.5600e+02, 2.8000e+01, 2.0000e+00,
        1.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 1.0000e+00]),
 array([ 0. ,  3.9,  7.8, 11.7, 15.6, 19.5, 23.4, 27.3, 31.2, 35.1, 39. ]),
 <a list of 10 Patch objects>)
~~~
{: .output}

![nMuon_hist_1]({{ page.root }}/fig/nMuon_hist_1.png)

> ## What's with all the numbers above the plot?
>
> ~~~
> plt.hist(branches['nMuon']);
> ~~~
> {: .language-python}
> ~~~
> plt.hist(branches['nMuon'])
> plt.show()
> ~~~
> {: .language-python}

~~~
plt.hist((branches['nMuon'], bins=10, range=(0, 10))
plt.show()
~~~
{: .language-python}

![nMuon_hist_2]({{ page.root }}/fig/nMuon_hist_2.png)

> ## Binning and range tips
>
> ~~~
> branches['nMuon'].mean()
> ~~~
> {: .language-python}
> ~~~
> 2.35286
> ~~~
> {: .output}
>
> ~~~
> branches['nMuon'].std()
> ~~~
> {: .language-python}
> ~~~
> 1.19175912851549
> ~~~
> {: .output}
>
> ~~~
> branches['nMuon'].min()
> ~~~
> {: .language-python}
> ~~~
> 0
> ~~~
> {: .output}
>
> ~~~
> branches['nMuon'].max()
> ~~~
> {: .language-python}
> ~~~
> 39
> ~~~
> {: .output}

~~~
plt.hist(branches['nMuon'], bins=10, range=(0, 10))
plt.xlabel('Number of muons in event')
plt.ylabel('Number of events')
plt.show()
~~~
{: .language-python}

![nMuon_hist_3]({{ page.root }}/fig/nMuon_hist_3.png)


~~~
plt.hist((branches['Muon_pt'].flatten(), bins=100, range=(0, 100));
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_1]({{ page.root }}/fig/Muon_pt_hist_1.png)

~~~
plt.hist(branches['Muon_pt'].flatten(), bins=100, range=(0, 100));
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.ylabel('Number of muons / 1 MeV')
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_2]({{ page.root }}/fig/Muon_pt_hist_2.png)

~~~
plt.hist(branches['Muon_pt'].flatten(), bins=100, range=(0, 100));
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.ylabel('Number of muons / 1 MeV')
plt.yscale('log')
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_3]({{ page.root }}/fig/Muon_pt_hist_3.png)

~~~
import numpy as np
plt.hist(branches['Muon_pt'].flatten(), bins=np.logspace(np.log10(1), np.log10(100), 100));
plt.xlabel('Muon $p_{\mathrm{T}}$ [MeV]')
plt.xscale('log')
plt.ylabel('Number of muons')
plt.show()
~~~
{: .language-python}

![Muon_pt_hist_4]({{ page.root }}/fig/Muon_pt_hist_4.png)

{% include links.md %}
