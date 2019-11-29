---
title: "Getting Physics-Relevant Information"
teaching: 0
exercises: 0
questions:
- "How can I use columnar analysis to do something useful for physics?"
objectives:
- ""
keypoints:
- ""
---

~~~
two_muons_mask = branches['nMuon'] == 2

two_muons_table = table[two_muons_mask]

import uproot_methods

two_muons_p4 = uproot_methods.TLorentzVectorArray.from_ptetaphim(two_muons_table['Muon_pt'],
                                                                 two_muons_table['Muon_eta'],
                                                                 two_muons_table['Muon_phi'],
                                                                 two_muons_table['Muon_mass'])


two_muons_p4
~~~
{: .language-python}
<JaggedArrayMethods [[TLorentzVector(10.764, 1.0668, -0.034273, 0.10566) TLorentzVector(15.737, -0.56379, 2.5426, 0.10566)] [TLorentzVector(10.538, -0.42778, -0.27479, 0.10566) TLorentzVector(16.327, 0.34923, 2.5398, 0.10566)] [TLorentzVector(57.607, -0.53209, -0.071798, 0.10566) TLorentzVector(53.045, -1.0042, 3.0895, 0.10566)] ... [TLorentzVector(9.5837, -1.5126, -0.22681, 0.10566) TLorentzVector(3.3317, 2.1995, -2.7097, 0.10566)] [TLorentzVector(46.362, -1.9284, -2.3773, 0.10566) TLorentzVector(43.904, -2.2734, 0.86049, 0.10566)] [TLorentzVector(3.3099, 1.6359, 0.87988, 0.10566) TLorentzVector(15.68, 0.47661, -1.7525, 0.10566)]] at 0x7f180ddc0400>
~~~
{: .output}

~~~
two_muons_p4.pt
two_muons_p4.eta
two_muons_p4.phi
two_muons_p4.E
two_muons_p4.mass

first_muon_p4 = two_muons_p4[:, 0]
first_muon_p4

second_muon_p4 = two_muons_p4[:, 1]
second_muon_p4

first_muon_p4.delta_r(second_muon_p4)

plt.hist(first_muon_p4.delta_r(second_muon_p4), bins=100)
plt.xlabel('$\Delta R$ between muons')
plt.ylabel('Number of two-muon events')
plt.show()

sum_p4 = first_muon_p4 + second_muon_p4
sum_p4

opposite_sign_muons_mask = two_muons_table['Muon_charge'][:, 0] != two_muons_table['Muon_charge'][:, 1]

opposite_sign_muons_table = two_muons_table[opposite_sign_muons_mask]


opposite_sign_muons_p4 = uproot_methods.TLorentzVectorArray.from_ptetaphim(opposite_sign_muons_table['Muon_pt'],
                                                                           opposite_sign_muons_table['Muon_eta'],
                                                                           opposite_sign_muons_table['Muon_phi'],
                                                                           opposite_sign_muons_table['Muon_mass'])

dimuon_p4 = opposite_sign_muons_p4[:, 0] + opposite_sign_muons_p4[:, 1]

plt.hist(dimuon_p4.mass, bins=np.logspace(np.log10(0.1), np.log10(1000), 200))
plt.xlabel('Dimuon invariant mass [GeV]')
plt.ylabel('Number of dimuon events')
plt.xscale('log')
plt.yscale('log')
plt.show()
~~~
{: .language-python}

![dimuon_invariant_mass_hist_1.png]({{ page.root }}/fig/dimuon_invariant_mass_hist_1.png)

![dimuon_invariant_mass_hist_2_labeled.png]({{ page.root }}/fig/dimuon_invariant_mass_hist_2_labeled.png)

~~~
len(dimuon_p4)
~~~
{: .language-python}
~~~
37183
~~~
{: .output}

{% include links.md %}
