+++
title = "Home"
weight = 0
+++

<div style="text-align: center;">
<img src="varlociraptor-logo.svg" style="width: 25%">

Flexible, uncertainty-aware variant calling <br/> with parameter free filtration via FDR control.
</div>

<style>
ul {
  display: inline-block;
  text-align: left;
}
h1 {
  display: none;
}
</style>

<div style="text-align: left">

### Key features


* Calls SNVs, MNVs, indels, arbitrary replacements, inversions, duplications, and breakends in all length ranges (from small to structural) with a unified statistical model.
* The statistical model entails all possible sources of uncertainty (mapping, typing, heterogeneity) and biases (strand, read pair orientation, read position, sampling, contamination).
* Resulting variant calls can be filtered by false discovery rate. No parameter tuning necessary.
* Maximum a posteriori allele frequency estimates are provided with each call.

### Calling modes

* Tumor-normal-calling, classifying variants as somatic in tumor, somatic in normal, germline, or absent.
* Generic, grammar based configuration of the statistical model, allowing to call variants for arbitrary scenarios (including germline, tumor/normal/relapse, pedigrees, FFPE data, and anything else).

</div>
