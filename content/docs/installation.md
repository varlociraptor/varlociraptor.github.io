+++
title = "Installation"
weight = 1
+++

## Via Bioconda

Varlociraptor can be best installed via [Bioconda](https://bioconda.github.io).
After having Bioconda set up as outlined in the [docs](https://bioconda.github.io/#usage), you can install varlociraptor into an isolated software environment via

```bash
conda create --name varlociraptor varlociraptor 
```

Then, activate the environment via

```bash
conda activate varlociraptor 
```

Afterwards, varlociraptor will be available in your `$PATH`, such that you can issue

```bash
varlociraptor --help 
```

to inspect the command line interface.
In any case, make sure you have the [latest stable version](https://bioconda.github.io/recipes/varlociraptor/README.html) by comparing with what is printed by

```bash
varlociraptor --version
```

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
