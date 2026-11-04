+++
title = "Filtering alteration calls"
weight = 3
+++

Varlociraptor provides built-in filtering mechanisms. Most importantly, these are designed to only require a single, meaningful parameter.

## Filtering by controlling FDR

Alteration calls from varlociraptor can be filtered by controlling the false discovery rate (FDR). You can choose to either control the global or the local FDR.
We recommend the local FDR for the following reason.

Controlling the global FDR means that Varlociraptor will try to find a set of alterations that most closely reaches the given FDR threshold.
Hence, it will start with the very clear cases, followed by borderline cases, and if there is still something left up to the desired threshold, it will even add clearly wrong alteration calls, just to guarantee that the FDR is as close as possible to the desired threshold.

In contrast, local FDR control means that for each individual alteration call, the probability to be a false discovery may not exceed the given threshold.
This leads to more intuitive results, since no obvious false positives are inserted just to reach the desired threshold as it happens with global FDR control.

### Smart vs. strict mode

In either case (local or global), you can choose between `smart` or `strict` FDR control.
In the latter case, Varlociraptor controls the FDR over the sum of the posterior probabilities of the chosen events of interest (the events you want to filter for, see below).
In the former case, Varlociraptor first controls the FDR over the probabilities for the alteration to be present (i.e. not absent and not being an artifact).
Second, it keeps only those alterations that pass the FDR control and in addition where the posterior probabilities for the events of interest sum up to at least 50% probability.
If the calls were made with `--no-absent`, `PROB_ABSENT` is missing and presence is determined by the artifact probability only (or is certain when using `--smart-retain-artifacts`).

The smart mode leads to more intuitive results, as it successfully captures borderline cases between different events, while effectively removing false positives.
It is therefore recommended to use the smart mode (as we also do in all examples below).

#### Retaining artifacts

Depending on the use case, it can be desired to consider alterations marked as potential artifacts as wanted or unwanted when performing FDR control. For this purpose, Varlociraptor offers the flag `--smart-retain-artifacts`.
The flag is only considered in `smart` mode.
When the flag is activated, the `artifact` event is not considered when deciding about presence or absense of the alteration (i.e. the probability for presence becomes 1 - the probability of the absent event).
Using the flag can be useful when maximum sensitivity is wanted and the user intends to carefully investigate artifact calls manually instead of filtering them out during FDR control.

### Control local FDR

Let `calls.bcf` be the output of the varlociraptor calling command.
Then, by running e.g.

```bash
varlociraptor filter-calls control-fdr --mode local-smart calls.bcf --events SOMATIC_TUMOR --fdr 0.05
```

calls can be filtered to all alterations that newly occur in a tumor sample (i.e. are somatic) with a **local** FDR of 5%.

### Control global FDR

For controlling the global FDR, you could issue the following command:

```bash
varlociraptor filter-calls control-fdr calls.bcf --mode global-smart --events SOMATIC_TUMOR --fdr 0.05 --var SNV
```

The reason for stratifying by alteration type here (`--var SNV`) is that their probabilities can fall into largely different ranges.
For example, most likely the probabilities for SNVs are likely much stronger than for indels and structural variants.
Hence, performing a combined instead of stratified FDR control would lead to a domination of SNVs in the resulting set.
Instead, by stratification we get the maximum resultion for each type of alteration in case of global FDR control.

### Filtering by posterior width

Sometimes, the question is not whether an alteration is present, but how precisely its allele frequency (e.g. a methylation rate) is determined.
For this purpose, you can split the range of possible allele frequencies into equally sized bins, define one event per bin in the scenario, and call with `--no-absent` (see [Calling variants](@/docs/calling.md)), e.g.

```yaml
samples:
  sample:
    universe: "[0.0,1.0]"
events:
  bin0: "sample:[0.0,0.1["
  bin1: "sample:[0.1,0.2["
  # ...
  bin9: "sample:[0.9,1.0]"
```

Then, the option `--subsets adjacent-<k>` lets Varlociraptor sum up the posterior probabilities of every window of `k` adjacent events (in the order given via `--events`) and use the maximum of these window sums instead of the sum over all given events.

```bash
varlociraptor filter-calls control-fdr calls.bcf --mode local-strict --events BIN0 BIN1 BIN2 BIN3 BIN4 BIN5 BIN6 BIN7 BIN8 BIN9 --fdr 0.05 --subsets adjacent-3
```

keeps only calls whose allele frequency lies, with a probability of at least 95%, within three adjacent bins, i.e. within an interval of width at most 0.3.
With global FDR control (`--mode global-strict`), the maximum window sums are used analogously, such that the expected fraction of reported calls whose allele frequency lies outside of their best window does not exceed the given FDR.

Hence, `k` bounds the accepted posterior width: the smaller `k` and the more events, the more precise the allele frequency has to be.
Since windows always start and end at bin borders, the effectively accepted width can vary, depending on where the posterior is located relative to the bins.
Defining more, narrower bins (and increasing `k` accordingly) reduces this effect.
