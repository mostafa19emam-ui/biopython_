### Original biopython pages:
[biopython website](https://biopython.org)


[biopython github](https://github.com/biopython/biopython)

## About this repo
A function added to calculate hybridization percentage of primers/probs at specific temperature
this is useful for evaluating the impact of non-specific hybridizatoions. NOT for its own primer attachment sites

suppose you have one specific probe that could attach to its complement sequence:
> 5'-ACAGTGAATACGTCAT***ATGCTGCCA***-3'

also suppose that there is a non-specific attachment of 9nt 3' end on another site. so you can calcuate the melting temperature of this hybridizations (specific and non-specific) by native biopython functions (Tm_NN, Tm_GC, ...)

```
from Bio.SeqUtils import MeltingTemp
seq = 'ACATACGTCATATGCTGCCA'
print(f'melting temperature of probe = {MeltingTemp.Tm_NN(seq)}', '\n',
      f'melting temperatue of non-specific attachment = {MeltingTemp.Tm_NN(seq[-9:])}')

>>> melting temperature of probe = 51.291240229480024
>>> melting temperatue of non-specific attachment = 22.229141439820523
```

However, understanding the ratio of which primers anneal to non-specific site across various annealing temperatures is crucial, as this knowledge allows you to optimize your assay and minimize off-targtes

```
from Bio.SeqUtils import MeltingTemp
import matplotlib.pyplot as plt
seq = 'ACATACGTCATATGCTGCCA'
Temps = list(range(0,60,5))
Xs = [MeltingTemp.hybrid_percent(seq[-9:],t) for t in Temps]
plt.plot(Temps,Xs)
plt.show()
```

<img width="562" height="294" alt="image" src="https://github.com/user-attachments/assets/1d401bfa-de38-4315-a040-84c5d9533582" />


## Theory
Melting temperature is the temperature that which half of the DNA stransds hybridized into double-stranded form. assuming thermodynamic eqilibrium, the Gibbs free energy is equal to zero. so the reaction have not tendency to move in any direction. this is the main assume of Nearest-Neighbour method to calculate Tm.

$$ ssDNA_{1}+ssDNA_{2}\ \leftrightarrow \ \ dsDNA $$

$$ \Delta G^{\ast }=\Delta G°+RT\ln{K}=0\ \ \ \Rightarrow \ \ \ \ \Delta H°-T\Delta S°=-RT\ln{K} $$

It is possible to calculate temperatue if we have K, ΔH and ΔS. by the Tm defenition it is possible to calculate K; ΔH and ΔS could be caculated with Nearest-Neighbour method so it is possible to calculate melting temperature (famous Tm_NN formula)
In the reverse way, we can also calculate the K given Temperature, ΔH and ΔS.

$$ K=e^{-\ \frac{\Delta H°-T\Delta S°}{RT}} $$

In the same way we determine ΔH and ΔS by NN method and then calculate K and then concentration of double strand DNA

$$ K=\frac{\left[ds\right]}{\left(c_{1}-\left[ds\right]\right)\left(c_{2}-\left[ds\right]\right)}\ \ \ \ \ \ \ \ \Rightarrow \ \ \ \ \ {\left[ds\right]}^{2}-\left(c_{1}+c_{2}+\frac{1}{K}\right)\left[ds\right]+c_{1}c_{2}=0 $$

finally the ratio is obtained by dividing dsDNA concentration to least primer concentration

$$ x=\frac{\left[ds\right]}{c_{1}} $$


