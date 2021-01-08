+++
title = "Filtering variant calls"
weight = 3
+++

Varlociraptor provides built-in filtering mechanisms. Most importantly, these are designed to only require a single, meaningful parameter.

## Filtering by controlling FDR

Variant calls from varlociraptor can be filtered by controlling the false discovery rate (FDR).
Let `calls.bcf` be the output of the varlociraptor calling command.
Then, by running e.g.

```bash
varlociraptor filter-calls control-fdr calls.bcf --events SOMATIC_TUMOR --fdr 0.05 --var SNV
```

calls can be filtered to all single nucleotide variants (SNVs) that newly occur in a tumor sample (i.e. are somatic) with an FDR of 5%.
Of course, this kind of filtration works for all variant types.
The reason for stratifying by variant type is that their probabilities can fall into largely different ranges.
For example, most likely the probabilities for SNVs are likely much stronger than for indels and structural variants.
Hence, performing a combined instead of stratified FDR control would lead to a domination of SNVs in the resulting set.
Instead, by stratification we get the maximum resultion for each type of variant.

Also, note that above FDR control means that Varlociraptor will try to find a set of variants that most closely reaches the given FDR threshold.
Hence, it will start with the very clear cases, followed by borderline cases, and if there is still something left up to the desired threshold, it will even add clearly wrong variant calls, just to guarantee that the FDR is as close as possible to the desired threshold.
While this is the most appropriate way from a statistical point of view, you might want to apply additional filters that keep the probability of an individualy variant call above a given threshold.
One way to achieve this is to use the posterior odds filter below, e.g. requiring at least `barely` support for each variant.

## Filtering by posterior odds

In addition to controlling the FDR, variants can also be filtered by their individual posterior odds for a given event.
With, e.g.

```bash
varlociraptor filter-calls posterior-odds --events SOMATIC_TUMOR --odds positive < calls.bcf
```

calls can be filtered to all those where the odds for being somatic in the tumor sample are at least positive. For the minimum odds, the [scale of Kass and Raftery](https://doi.org/10.2307/2291091) is used, categorizing odds into `none`, `barely`, `positive`, `strong`, and `very strong`.
