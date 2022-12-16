+++
title = "Home"
weight = 0
+++

<div style="text-align: center;">
<img src="varlociraptor-logo.svg" style="width: 25%">

Flexible, arbitrary-scenario, uncertainty-aware variant calling <br/> with parameter free filtration via FDR control.
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


* Calls SNVs, MNVs, indels, arbitrary replacements, inversions, duplications, haplotype blocks (combinations of any of the previous), and breakends.
* Supports all length ranges (from small to structural) with a unified statistical model.
* The statistical model entails all possible sources of uncertainty (mapping, typing, heterogeneity) and biases (strand, read pair orientation, read position, sampling, contamination, homologous regions).
* Resulting variant calls can be filtered by false discovery rate. No parameter tuning necessary.
* Maximum a posteriori allele frequency estimates are provided with each call.

### Calling modes

* Generic, grammar based configuration of the statistical model, allowing to call variants for arbitrary scenarios (including germline, tumor/normal/relapse, pedigrees, FFPE data, and anything else).
* Tumor-normal-calling, classifying variants as somatic in tumor, somatic in normal, germline, or absent.

---

[![Bioconda](https://img.shields.io/conda/dn/bioconda/varlociraptor?label=bioconda%20downloads)](https://bioconda.github.io/recipes/varlociraptor/README.html)
[![Repo](https://img.shields.io/badge/code%20on-github-blue)](https://github.com/varlociraptor/varlociraptor)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/varlociraptor/varlociraptor/rust.yml?branch=master&label=tests)](https://github.com/varlociraptor/varlociraptor/commits/master)
[![License](https://img.shields.io/github/license/varlociraptor/varlociraptor)](https://github.com/varlociraptor/varlociraptor/blob/master/LICENSE)
[![Codecov](https://img.shields.io/codecov/c/github/varlociraptor/varlociraptor/master.svg?label=test%20coverage)](https://codecov.io/gh/varlociraptor/varlociraptor)
[![API docs](https://img.shields.io/badge/API-documentation-blue.svg)](https://docs.rs/varlociraptor)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)

</div>
