+++
title = "Installation"
weight = 1
+++

## Via Bioconda

Varlociraptor can be best installed via [Bioconda](https://bioconda.github.io).
After having Bioconda set up as outlined in the [docs](http://bioconda.github.io/user/install.html), you can install varlociraptor via

```bash
conda install varlociraptor 
```

or alternatively create an isolated software environment with

```bash
conda create --name varlociraptor varlociraptor 
```

In the latter case, activate the environment via

```bash
conda activate varlociraptor 
```

Afterwards, varlociraptor will be available in your `$PATH`, such that you can issue

```bash
varlociraptor --help 
```

to inspect the command line interface.

## Via Cargo

Alternatively you can install varlociraptor via the Rust package manager [Cargo](https://doc.rust-lang.org/cargo/).
For building, it needs the following dependencies (including headers):

* zlib
* bzip2
* xz
* GSL (if you have GSLv2, you need to specify `--features gslv2` in the command below)
* cmake

Given that Cargo is installed in your system, run

```bash
cargo install varlociraptor 
```

Afterwards, varlociraptor will be available in your `$PATH`, such that you can issue

```bash
varlociraptor --help 
```

to inspect the command line interface.
