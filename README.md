## Personal Cancer Genome Reporter (PCGR)- variant interpretation report for precision oncology

### Overview

The Personal Cancer Genome Reporter (PCGR) is a stand-alone software package for functional annotation and translation of individual cancer genomes for precision oncology. It interprets both somatic SNVs/InDels and copy number aberrations. The software extends basic gene and variant annotations from the [Ensembl’s Variant Effect Predictor (VEP)](http://www.ensembl.org/info/docs/tools/vep/index.html) with oncology-relevant, up-to-date annotations retrieved flexibly through [vcfanno](https://github.com/brentp/vcfanno), and produces interactive HTML reports intended for clinical interpretation (Figure 1).

![PCGR overview](PCGR_workflow.png)

### News
* _November 29th 2017_: **0.5.3 release**
   * Fixed bug with propagation of default options
* _November 23rd 2017_: **0.5.2 release**
* _November 15th 2017_: **0.5.1 pre-release**
  * Bug fixing (VCF validation)
* _November 14th 2017_: **0.5.0 pre-release**
  * Updated version of VEP (v90)
  * Updated versions of ClinVar, Uniprot KB, CIViC, CBMDB
  * Removal of ExAC (replaced by gnomAD), removal of COSMIC due to licensing restrictions
  * Users can analyze samples run without matching control (i.e. tumor-only)
  * PCGR pipeline is now configured through a [TOML-based configuration file](https://github.com/toml-lang/toml)
  * Bug fixes / general speed improvements
  * _Work in progress: Export of report data through JSON_


### Example reports
* [Report for a breast tumor sample (TCGA)](http://folk.uio.no/sigven/tumor_sample.BRCA.0.5.3.pcgr.html)
* [Report for a colon adenocarcinoma sample (TCGA)](http://folk.uio.no/sigven/tumor_sample.COAD.0.5.3.pcgr.html)


### PCGR documentation

[![Documentation Status](https://readthedocs.org/projects/pcgr/badge/?version=latest)](http://pcgr.readthedocs.io/en/latest/?badge=latest)

If you use PCGR, please cite our recent publication:

Sigve Nakken, Ghislain Fournous, Daniel Vodák, Lars Birger Aaasheim, Ola Myklebost, and Eivind Hovig. __Personal Cancer Genome Reporter: variant interpretation report for precision oncology__ (2017). _Bioinformatics (in press)_. doi:[10.1093/bioinformatics/btx817](https://doi.org/10.1093/bioinformatics/btx817)

### Annotation resources included in PCGR (v0.5.3)

* [VEP v90](http://www.ensembl.org/info/docs/tools/vep/index.html) - Variant Effect Predictor release 90 (GENCODE v27 as the gene reference dataset)
* [dBNSFP v3.4](https://sites.google.com/site/jpopgen/dbNSFP) - Database of non-synonymous functional predictions (March 2017)
* [gnomAD r1](http://gnomad.broadinstitute.org/) - Germline variant frequencies exome-wide (March 2017)
* [dbSNP b147](http://www.ncbi.nlm.nih.gov/SNP/) - Database of short genetic variants (April 2016)
* [1000 Genomes Project - phase3](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/) - Germline variant frequencies genome-wide (May 2013)
* [TCGA release 9.0](https://portal.gdc.cancer.gov/) - somatic mutations discovered across 33 tumor type cohorts (The Cancer Genome Atlas)
* [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar/) - Database of clinically related variants (November 2017)
* [DoCM](http://docm.genome.wustl.edu) - Database of curated mutations (v3.2, April 2016)
* [CIViC](http://civic.genome.wustl.edu) - Clinical interpretations of variants in cancer (November 11th 2017)
* [CBMDB](http://www.cancergenomeinterpreter.org/biomarkers) - Cancer Biomarkers database (November 11th 2017)
* [IntOGen catalog of driver mutations](https://www.intogen.org/downloads) - (May 2016)
* [DisGeNET](http://www.disgenet.org) - Database of curated gene-tumor type associations (May 2017)
* [Cancer Hotspots](http://cancerhotspots.org) - Resource for statistically significant mutations in cancer (2016)
* [UniProt/SwissProt KnowledgeBase 2017_10](http://www.uniprot.org) - Resource on protein sequence and functional information (October 2017)
* [Pfam v31](http://pfam.xfam.org) - Database of protein families and domains (March 2017)
* [DGIdb](http://dgidb.genome.wustl.edu) - Database of targeted cancer drugs  (v3.0, September 2017)
* [TSGene v2.0](http://bioinfo.mc.vanderbilt.edu/TSGene/) - Tumor suppressor/oncogene database (November 2015)

### Getting started

#### STEP 0: Python

A local installation of Python (it has been tested with [version 2.7.13](https://www.python.org/downloads/)) is required to run PCGR. Check that Python is installed by typing `python --version` in a terminal window. In addition, a [Python library](https://github.com/uiri/toml) for parsing configuration files encoded with [TOML](https://github.com/toml-lang/toml) is needed. To install, simply run the following command:

   	pip install toml

#### STEP 1: Installation of Docker

1. [Install the Docker engine](https://docs.docker.com/engine/installation/) on your preferred platform
   - installing [Docker on Linux](https://docs.docker.com/engine/installation/linux/)
   - installing [Docker on Mac OS](https://docs.docker.com/engine/installation/mac/)
   - NOTE: We have not yet been able to perform enough testing on the Windows platform, and we have received feedback that particular versions of Docker/Windows do not work with PCGR (an example being [mounting of data volumes](https://github.com/docker/toolbox/issues/607))
2. Test that Docker is running, e.g. by typing `docker ps` or `docker images` in the terminal window
3. Adjust the computing resources dedicated to the Docker, i.e.:
   - Memory: minimum 5GB
   - CPUs: minimum 4
   - [How to - Mac OS X](https://docs.docker.com/docker-for-mac/#advanced)

#### STEP 2: Download PCGR

1. Download and unpack the [latest software release (0.5.3)](https://github.com/sigven/pcgr/releases/tag/v0.5.3)
2. Download and unpack the data bundle (approx. 16Gb) in the PCGR directory
   * Download [the accompanying data bundle](https://drive.google.com/file/d/1NSeMWpLVMBcCEDYpOLsuWSnKfZEaamip/) from Google Drive to `~/pcgr-X.X` (replace _X.X_ with the version number, e.g `~/pcgr-0.5.3`)
   * Unpack the data bundle, e.g. through the following Unix command: `gzip -dc pcgr.databundle.GRCh37.YYYYMMDD.tgz | tar xvf -`

    A _data/_ folder within the _pcgr-X.X_ software folder should now have been produced
3. Pull the [PCGR Docker image (0.5.3)](https://hub.docker.com/r/sigven/pcgr/) from DockerHub (approx 4.2Gb):
   * `docker pull sigven/pcgr:0.5.3` (PCGR annotation engine)

#### STEP 3: Input preprocessing

The PCGR workflow accepts two types of input files:

  * An unannotated, single-sample VCF file (>= v4.2) with called somatic variants (SNVs/InDels)
  * A copy number segment file

  __NOTE: GRCh37 is currently supported as the reference genome build__

PCGR can be run with either or both of the two input files present.

* We __strongly__ recommend that the input VCF is compressed and indexed using [bgzip](http://www.htslib.org/doc/tabix.html) and [tabix](http://www.htslib.org/doc/tabix.html)
* If the input VCF contains multi-allelic sites, these will be subject to [decomposition](http://genome.sph.umich.edu/wiki/Vt#Decompose)
* Variants used for reporting should be designated as 'PASS' in the VCF FILTER column



The tab-separated values file with copy number aberrations __MUST__ contain the following four columns:
* Chromosome
* Start
* End
* Segment_Mean

Here, _Chromosome_, _Start_, and _End_ denote the chromosomal segment (GRCh37), and __Segment_Mean__ denotes the log(2) ratio for a particular segment, which is a common output of somatic copy number alteration callers. Below shows the initial part of a copy number segment file that is formatted correctly according to PCGR's requirements:

    Chromosome	Start	End	Segment_Mean
    1 3218329 3550598 0.0024
    1 3552451 4593614 0.1995
    1 4593663 6433129 -1.0277


#### STEP 4: Run example

A tumor sample report is generated by calling the Python script __pcgr.py__, which takes the following arguments and options:

	usage: pcgr.py [-h] [--input_vcf INPUT_VCF] [--input_cna INPUT_CNA]
			 [--force_overwrite] [--version]
			 pcgr_dir output_dir configuration_file sample_id

	Personal Cancer Genome Reporter (PCGR) workflow for clinical interpretation of
	somatic nucleotide variants and copy number aberration segments

	positional arguments:
	pcgr_dir              PCGR base directory with accompanying data directory,
					e.g. ~/pcgr-0.5.3
	output_dir            Output directory
	configuration_file    PCGR configuration file (TOML format)
	sample_id             Tumor sample/cancer genome identifier - prefix for
					output files

	optional arguments:
	-h, --help            show this help message and exit
	--input_vcf INPUT_VCF
					VCF input file with somatic query variants
					(SNVs/InDels). Note: GRCh37 is currently the only
					reference genome build supported (default: None)
	--input_cna INPUT_CNA
					Somatic copy number alteration segments (tab-separated
					values) (default: None)
	--force_overwrite     By default, the script will fail with an error if any
					output file already exists. You can force the
					overwrite of existing result files by using this flag
					(default: False)
	--version             show program's version number and exit


The configuration file, formatted using [TOML](https://github.com/toml-lang/toml) (an easy to read file format) enables the user to configure a number of options in the PCGR workflow, related to the following:

* MSI prediction
* Mutational signatures analysis
* Coding target size - for mutational burden analysis
* Tumor-only analysis options (i.e. exclusion of germline variants/enrichment for somatic calls)
* VEP/_vcfanno_ options
* Specification of INFO tags in VCF that denote sequencing depth/allelic support of variants
* Log-ratio thresholds for gains/losses in CNA analysis

The _examples_ folder contain input files from two tumor samples sequenced within TCGA. It also contains a PCGR configuration file. A report for a colorectal tumor case can be generated by running the following command in your terminal window:

`python pcgr.py --input_vcf ~/pcgr-0.5.3/examples/tumor_sample.COAD.vcf.gz`
`--input_cna ~/pcgr-0.5.3/examples/tumor_sample.COAD.cna.tsv`
` ~/pcgr-0.5.3 ~/pcgr-0.5.3/examples ~/pcgr-0.5.3/examples/pcgr_configuration_examples.toml tumor_sample.COAD`

This command will run the Docker-based PCGR workflow and produce the following output files in the _examples_ folder:

1. __tumor_sample.COAD.pcgr.html__ - An interactive HTML report for clinical interpretation
2. __tumor_sample.COAD.pcgr.vcf.gz__ - VCF file with rich set of annotations for precision oncology
3. __tumor_sample.COAD.pcgr.maf__ - A basic [MAF](https://wiki.nci.nih.gov/display/TCGA/Mutation+Annotation+Format+%28MAF%29+Specification) file for use as input in downstream analyses with other tools (e.g. [2020plus](https://github.com/KarchinLab/2020plus), MutSigCV)
4. __tumor_sample.COAD.pcgr.snvs_indels.tiers.tsv__ - Tab-separated values file with variants organized according to tiers of functional relevance
5. __tumor_sample.COAD.pcgr.mutational_signatures.tsv__ - Tab-separated values file with estimated contributions by known mutational signatures and associated underlying etiologies
6. __tumor_sample.COAD.pcgr.snvs_indels.biomarkers.tsv__ - Tab-separated values file with clinical evidence items associated with biomarkers for diagnosis, prognosis or drug sensitivity/resistance
7. __tumor_sample.COAD.pcgr.cna_segments.tsv.gz__ - Tab-separated values file with annotations of gene transcripts that overlap with somatic copy number aberrations

## Contact

sigven@ifi.uio.no
