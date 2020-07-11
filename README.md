# ![nf-core/deepvariant](docs/images/nf-core-deepvariant_logo.png)

**[Google's DeepVariant](https://github.com/google/deepvariant) variant caller as a Nextflow pipeline**.

[![GitHub Actions CI Status](https://github.com/nf-core/deepvariant/workflows/nf-core%20CI/badge.svg)](https://github.com/nf-core/deepvariant/actions)
[![GitHub Actions Linting Status](https://github.com/nf-core/deepvariant/workflows/nf-core%20linting/badge.svg)](https://github.com/nf-core/deepvariant/actions)
[![Nextflow](https://img.shields.io/badge/nextflow-%E2%89%A519.10.0-brightgreen.svg)](https://www.nextflow.io/)

[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg)](http://bioconda.github.io/)
[![Docker](https://img.shields.io/docker/automated/nfcore/deepvariant.svg)](https://hub.docker.com/r/nfcore/deepvariant)

## Introduction

The pipeline is built using [Nextflow](https://www.nextflow.io), a workflow tool to run tasks across multiple compute infrastructures in a very portable manner. It comes with docker containers making installation trivial and results highly reproducible.

### What is DeepVariant and why in Nextflow?

The Google Brain Team in December 2017 released a [Variant Caller](https://www.ebi.ac.uk/training/online/course/human-genetic-variation-i-introduction/variant-identification-and-analysis/what-variant) based on DeepLearning: DeepVariant.

In practice, DeepVariant first builds images based on the BAM file, then it uses a DeepLearning image recognition approach to obtain the variants and eventually it converts the output of the prediction in the standard VCF format.

DeepVariant as a Nextflow pipeline provides several advantages to the users. It handles automatically, through **preprocessing steps**, the creation of some extra needed indexed and compressed files which are a necessary input for DeepVariant, and which should normally manually be produced by the users.
Variant Calling can be performed at the same time on **multiple BAM files** and thanks to the internal parallelization of Nextflow no resources are wasted.
Nextflow's support of Docker allows to produce the results in a computational reproducible and clean way by running every step inside of a **Docker container**.

For more detailed information about Google's DeepVariant please refer to [google/deepvariant](https://github.com/google/deepvariant) or this [blog post](https://research.googleblog.com/2017/12/deepvariant-highly-accurate-genomes.html). <br />
For more information about DeepVariant in Nextflow please refer to this [blog post](https://blog.lifebit.ai/post/deepvariant/?utm_campaign=documentation&utm_source=github&utm_medium=web)

## Quick Start

i. Install [`nextflow`](https://nf-co.re/usage/installation)

ii. Install either [`Docker`](https://docs.docker.com/engine/installation/) or [`Singularity`](https://www.sylabs.io/guides/3.0/user-guide/) for full pipeline reproducibility (please only use [`Conda`](https://conda.io/miniconda.html) as a last resort; see [docs](https://nf-co.re/usage/configuration#basic-configuration-profiles))

iii. Download the pipeline and test it on a minimal dataset with a single command

```bash
nextflow run nf-core/deepvariant -profile test,<docker/singularity/conda/institute>
```

> Please check [nf-core/configs](https://github.com/nf-core/configs#documentation) to see if a custom config file to run nf-core pipelines already exists for your Institute. If so, you can simply use `-profile <institute>` in your command. This will enable either `docker` or `singularity` and set the appropriate execution settings for your local compute environment.

iv. Start running your own analysis!

**Warning DeepVariant can be very computationally intensive to run.**

A typical run on **whole genome data** looks like this:

```bash
nextflow run nf-core/deepvariant --genome hg19 --bam yourBamFile --bed yourBedFile -profile standard,docker
```

In this case variants are called on the bam files contained in the testdata directory. The hg19 version of the reference genome is used.
One vcf files is produced and can be found in the folder "results"

A typical run on **whole exome data** looks like this:

```bash
nextflow run nf-core/deepvariant --exome --genome hg19 --bam_folder myBamFolder --bed myBedFile -profile standard,docker
```

See [usage docs](docs/usage.md) for all of the available options when running the pipeline.

## Documentation

The nf-core/deepvariant pipeline comes with documentation about the pipeline, found in the `docs/` directory:

1. [Installation](https://nf-co.re/usage/installation)
2. Pipeline configuration
    * [Local installation](https://nf-co.re/usage/local_installation)
    * [Adding your own system config](https://nf-co.re/usage/adding_own_config)
    * [Reference genomes](https://nf-co.re/usage/reference_genomes)
3. [Running the pipeline](docs/usage.md)
4. [Output and how to interpret the results](docs/output.md)
5. [Troubleshooting](https://nf-co.re/usage/troubleshooting)
6. [More about DeepVariant](docs/about.md)

## More about the pipeline

As shown in the following picture, the worklow both contains **preprocessing steps** ( light blue ones ) and proper **variant calling steps** ( darker blue ones ).

Some input files ar optional and if not given, they will be automatically created for the user during the preprocessing steps. If these are given, the preprocessing steps are skipped. For more information about preprocessing, please refer to the "INPUT PARAMETERS" section.

The worklow **accepts one reference genome and multiple BAM files as input**. The variant calling for the several input BAM files will be processed completely indipendently and will produce indipendent VCF result files. The advantage of this approach is that the variant calling of the different BAM files can be parallelized internally by Nextflow and take advantage of all the cores of the machine in order to get the results at the fastest.

<p align="center">
  <img src="https://github.com/nf-core/deepvariant/blob/master/pics/pic_workflow.jpg">
</p>

## Credits

This pipeline was originally developed at [Lifebit](https://lifebit.ai/?utm_campaign=documentation&utm_source=github&utm_medium=web), by @luisas, to ease and reduce cost for variant calling analyses

Many thanks to nf-core and those who have helped out along the way too, including (but not limited to): @ewels, @MaxUlysse, @apeltzer, @sven1103 & @pditommaso

## Contributions and Support

If you would like to contribute to this pipeline, please see the [contributing guidelines](.github/CONTRIBUTING.md).

For further information or help, don't hesitate to get in touch on [Slack](https://nfcore.slack.com/channels/deepvariant) (you can join with [this invite](https://nf-co.re/join/slack)).

## Citation

<!-- TODO nf-core: Add citation for pipeline after first release. Uncomment lines below and update Zenodo doi. -->
<!-- If you use  nf-core/deepvariant for your analysis, please cite it using the following doi: [10.5281/zenodo.XXXXXX](https://doi.org/10.5281/zenodo.XXXXXX) -->

You can cite the `nf-core` publication as follows:

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen.
>
> _Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).  
> ReadCube: [Full Access Link](https://rdcu.be/b1GjZ)
