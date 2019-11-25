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
Muon_pt = branches['Muon_pt']
~~~
{: .language-python}

~~~
plt.hist(Muon_pt);
~~~
{: .language-python}

![nMuon_hist]({{ page.root }}/fig/nMuon_hist.png)

{% include links.md %}
