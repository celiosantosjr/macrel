# Macrel: (Meta)genomic AMP Classification and Retrieval

Pipeline to mine antimicrobial peptides (AMPs) from (meta)genomes.

[![Build Status](https://travis-ci.com/BigDataBiology/macrel.svg?branch=master)](https://travis-ci.com/BigDataBiology/macrel)
[![Documentation Status](https://readthedocs.org/projects/macrel/badge/?version=latest)](https://macrel.readthedocs.io/en/latest/?badge=latest)
[![license: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Install with Bioconda](https://anaconda.org/bioconda/macrel/badges/installer/conda.svg)](https://anaconda.org/bioconda/macrel)
[![Install with Bioconda](https://anaconda.org/bioconda/macrel/badges/downloads.svg)](https://anaconda.org/bioconda/macrel)

If you use this software in a publication please cite

>   Santos-Júnior CD, Pan S, Zhao X, Coelho LP. 2020.
>   Macrel: antimicrobial peptide screening in genomes and metagenomes.
>   PeerJ 8:e10555. DOI: [10.7717/peerj.10555](https://doi.org/10.7717/peerj.10555)


## License

GPLv3.

While Macrel as a whole is **GPL v3** licensed (to comply with it being used in
some of its dependencies, namely Peptides), the macrel-specific code is also
licensed under the **MIT** license.

## Install

The recommended method of installation is through
[bioconda](https://anaconda.org/bioconda/macrel):

```bash
conda install -c bioconda macrel
```

To install from source, [read the docs](https://macrel.readthedocs.io/en/latest/install)

### Examples

> Macrel uses a _subcommand interface_. You run `macrel COMMAND ...` with the
> COMMAND specifying which components of the pipeline you want to use.

To run these examples, first download the example sequences from
[github](https://github.com/BigDataBiology/macrel/tree/master/example_seqs), or
by running:

```bash
macrel get-examples
```

The main output file generated by Macrel consists of a table with 6 columns containing
the: sequence access code, peptide sequence, classification of peptide accordingly
composition and structure, the probability associated with the AMP prediction,
hemolytic activity prediction and probability associated to hemolytic activity
prediction. All peptides outputted in this table are considered AMPs (p > 0.5),
although peptides predicted as AMPs with probabilities closer to 1 are more likely
to be active. 

To run Macrel on peptides, use the `peptides` subcommand:

```bash
macrel peptides \
    --fasta example_seqs/expep.faa.gz \
    --output out_peptides
```

In this case, we use `example_seqs/expep.faa.gz` as the input sequence. This should
be an amino-acid FASTA file. The outputs will be written into a folder called
`out_peptides`, and Macrel will 4 threads. An example of output using
this mode can be found at `test/peptides/expected.prediction`.

To run Macrel on contigs, use the `contigs` subcommand:

```bash
macrel contigs \
    --fasta example_seqs/excontigs.fna.gz \
    --output out_contigs
```

In this example, we use the example file `excontigs.fna.gz` which is a FASTA
file with nucleotide sequences, writing the output to `out_contigs`.
An example of output using this mode can be found at `test/contigs/expected.prediction`.
Additionally to the prediction table, this mode also produces two files containing
general gene prediction information in the contigs and a fasta file containing the
predicted and filtered small genes (<= 100 amino acids).

To run Macrel on paired-end reads, use the `reads` subcommand:

```bash
macrel reads \
    -1 example_seqs/R1.fq.gz \
    -2 example_seqs/R2.fq.gz \
    --output out_metag \
    --outtag example_metag
```

The paired-end reads are given as paired files (here, `example_seqs/R1.fq.gz`
and `example_seqs/R2.fq.gz`). If you only have single-end reads, you can omit
the `-2` argument. An example of outputs using this mode can be found at
`test/reads/expected.prediction` and `test/reads/expected.smorfs.faa`.
Additionally to the prediction table, this mode also produces a contigs fasta file, 
and the two files containing general gene prediction coordinates and a fasta file
containing the predicted and filtered small genes (<= 100 amino acids).

To run Macrel to get abundance profiles, you only need the short reads file
and a reference with peptide sequences. Use the `abundance` subcommand:


```bash
macrel abundance \
    -1 example_seqs/R1.fq.gz \
    --fasta example_seqs/ref.faa.gz \
    --output out_abundance \
    --outtag example_abundance
```

This mode returns a table of abundances containing two columns, the first with the
name of the AMPs and the second with the number of reads mapped back to each peptide
using the given reference. An example of this output using the example file can be found
at `test/abundances/expected.abundance.txt`.

### Community

Macrel is actively developed to fix all issues and assimilate all suggestions we get from 
users. To make this communication more dynamic and even more colaborative, we created a google
community. There, Macrel users can discuss issues, ongoing projects, colaborations and
much more. Please pay us a visit:

[**Go to AMPsphere community now!**](https://groups.google.com/g/ampsphere-users?pli=1)

*Technical issues still are encouraged to be preferentialy sent to the reserved space in the
Macrel github repository.*

---

This is a project hosted by the **Big Data Biology Research Group**, follow this [link](big-data-biology.org/) to know us more.
