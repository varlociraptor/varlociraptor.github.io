+++
title = "Reporting issues"
weight = 5
+++

For reporting issues, please use our [issue tracker](https://github.com/varlociraptor/varlociraptor/issues).
If problems entail certain variant calls, it is particularly easy to create a proper test case with varlociraptor (see below).

## Issues with called variants

If you have the feeling that a variant call from varlociraptor is wrong, we would be grateful to receive a test case.
For this purpose, varlociraptor provides an interface to automatically create test cases from runs.
Let

```bash
varlociraptor call variants tumor-normal --purity 0.75 --tumor tumor.bcf --normal normal.bcf
```

be the original command you ran which yielded a variant call you believe to be wrong.
Then, you can issue

```bash
varlociraptor call variants --testcase-prefix testcase --testcase-locus CHROM:POS tumor-normal --purity 0.75 --tumor tumor.bcf --normal normal.bcf
```

with `CHROM:POS` being the locus of the variant as it is defined in the vcf file (1-based position).
Varlociraptor will then create a folder under the given prefix (here `testcase`), containing

* relevant subsets of the given BAM files (the reads around the locus),
* a VCF file with the candidate variant,
* a file `testcase.yaml` containing the used parameters, the reference sequence around the variant, and sample information.

The `testcase.yaml` looks similar to this:

```yaml
expected:
  allelefreqs:
    # write down a list of expressions of the form
    - sample_name > 0.45 && sample_name < 0.55
  posteriors:
    # write down a list of expressions of the form
    - PROB_SOMATIC_TUMOR <= 0.05
    - PROB_GERMLINE_HET > 0.05

# necessary bam files
samples:
  tumor:
    path: tumor.bam
    properties: '{"insert_size":{"mean":312.0,"sd":11.89254089203071},"max_del_cigar_len":30,"max_ins_cigar_len":12,"frac_max_softclip":0.69}'
  normal:
    path: normal.bam
    properties: '{"insert_size":{"mean":312.0,"sd":11.89254089203071},"max_del_cigar_len":30,"max_ins_cigar_len":12,"frac_max_softclip":0.69}'


# candidate variant
candidate: candidates.vcf

# reference sequence
reference:
  name: chr1
  seq: cccaaaatgctgggattataggcataagttaccatgcctggccATTTTTGTGTCTTTCTTGATGAGCAACTGCTCTGTTCCAGCCCTGTGCTGGGCATATTCACATCTTTTTCTTCTCTCTCTCTCTtttctttctttctttctttcttttctttctttctttctttcctttctttctttctttctttctttctttctttctttctttttctttttctttccttccttccttcttcctttctttctttctttctttctttttctttccttccttccttcttccttccttgcttgcttccttccttctttccctccctccctccctccctccttacttccctccctccctctctctttctctttccttctttttctttcgactgtgtcttgttct

options: '{"Call": {"kind": {"Variants": {"spurious_ins_rate": 2.8e-06, "spurious_del_rate":
  5.1e-06, "spurious_insext_rate": 0.0, "spurious_delext_rate": 0.0, "indel_window":
  100, "omit_snvs": false, "omit_indels": false, "max_indel_len": 1000, "max_depth":
  200, "reference": "../hg18/chr1.fa", "candidates": "candidates.vcf", "output": null,
  "testcase_locus": "chr1:17926776:1", "testcase_prefix": "/tmp/testcase", "mode":
  {"TumorNormal": {"tumor": "tumor.bam", "normal": "normal.bam", "purity": 1.0}}}}}}'
```

The first section `expected`, has to be edited by you in order to denote the expected allele frequencies, and the posterior probabilities.
Thereby, both fields are optional.

Finally, the testcase can be zipped and attached to a [new issue in our issue tracker](https://github.com/varlociraptor/varlociraptor/issues).
Before uploading a testcase like this, make sure that the testcase does not contain identifying information about your samples (e.g., no clear names in files).
In general, even creating test cases from clinical samples might be fine, because they only contain reads around the problematic locus.
Of course, that decision is up to you.
In any case, your help with improving varlociraptor is greatly appreciated.
