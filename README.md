#### Original biopython pages:
[biopython website](https://biopython.org)


[biopython github](https://github.com/biopython/biopython)

### About this repo
A function added to calculate hybridization percentage of primers/probs at specific temperature
this is useful for evaluating the impact of non-specific hybridizatoions. NOT for its own primer attachment sites

suppose you have one specific probe that could attach to its complement sequence:
> 5'-ACAGTGAATACGTCAT***ATGCTGCCA***-3'

also suppose that there is a non-specific attachment of 9nt 3' end on another site. so you can calcuate the melting temperature of this hybridizations (specific and non-specific) by native biopython functions (Tm_NN, Tm_GC, ...)

```
from Bio.SeqUtils import MeltingTemp
seq = 'ACAGTGAATACGTCATATGCTGCCA'
print(f'melting temperature of probe = {MeltingTemp.Tm_NN(seq)}', '\n',
      f'melting temperatue of non-specific attachment = {MeltingTemp.Tm_NN(seq[-9:])}')

>>> melting temperature of probe = 56.6621573331567
>>> melting temperatue of non-specific attachment = 22.229141439820523
```

However, understanding the ratio of which primers anneal to non-specific site across various annealing temperatures is crucial, as this knowledge allows you to optimize your assay and minimize off-targtes

```
from Bio

ATGCTGCCA
