
* Why `blastn` is run?

    To check that the contigs of the new assembly are matching the previous assembly.

## Not to forget

* Both amplicon or metagenomic data 
* Only illumina sequencing

* Artic protocol is actually an amplicon protocol since they are capturing the virus using known sequences.

"The primer bed file we have been using until now (https://github.com/drpatelh/test-datasets/blob/viralrecon/genome/NC_045512.2/sara/nCoV-2019.schemeMod.bed) doesnt match any of the original ARTIC files in here"

Lo que entiendo es que el variant calling de la pipeline funciona de la siguiente manera
En el punto:

**5. vi. Choice of multiple variant calling routes**

Todos los **call variants** tienen:

* Consensus sequence generation 

* Variant annotation

* Consensus assessment report

Esto es porque el consensus sequence generation se hace ya que estamos mirando intrahost variability y es por ello que
ivar se llama así como explica en su paper y en su [repositorio](https://github.com/andersen-lab/ivar). 

Mi pregunta es entonces cuando se utiliza el reference genome a parte de para comparar con el assembly?

El reference genome se utiliza para mapar las reads con bowtie!!! 
Y en el caso de nuevas assemblies para comprar la assembly con el reference genome

### Variant calling

From Instituto Carlos III [repository](https://github.com/BU-ISCIII/SARS_Cov2_consensus-nf/blob/master/docs/output.md)

In this pipeline to generate the consensus viral genome we use the approach of calling for variants between the mapped 
reads and the reference viral genome, and adding these variants to the reference viral genome.

### Annotation

SnpEff and SnpSift

SnpEff [8] is a genetic variant annotation and functional effect prediction toolbox. It annotates and predicts the 
**effects of genetic variants on genes and proteins** (such as amino acid changes). SnpSift[9] annotates genomic 
variants using databases, filters, and manipulates genomic annotated variants. Once you annotated your files using 
SnpEff, you can use SnpSift to help you filter large genomic datasets in order to find the most significant variants for 
your experiment.

### Consensus genome

BCFtools

Bcftools ftom the SAMtools project [4] is a set of utilities that manipulate variant calls in the Variant Call Format 
(VCF) and its binary counterpart BCF. The resulting variant calling vcf for haploid genomes is indexed and then the 
**consensus genome** is created adding the variants to the reference viral genome. This consensus genome was obtained using 
the predominant variants (majority) of the mapping file.

Bedtools

Bedtools [10] are a swiss-army knife of tools for a wide-range of genomics analysis tasks. In this case we use:

* bedtools genomecov computes histograms (default), per-base reports (-d) and BEDGRAPH (-bg) summaries of feature 
coverage (e.g., aligned sequences) for a given genome.

* bedtools maskfasta masks sequences in a FASTA file based on intervals defined in a feature file. This may be useful 
for creating your own masked genome file based on custom annotations or for masking all but your target regions when
aligning sequence data from a targeted capture experiment.

### 6. de novo assembly

#### 6.3.1 BLAST

**blastn** is used to align the assembled contigs against the virus reference genome.

#### 6.3.2 ABACAS

ABACAS was developed to rapidly contiguate (align, order, orientate), visualize and design primers to close gaps on 
shotgun assembled contigs based on a reference sequence

#### 6.3.3 PlasmidID

PlasmidID was used to graphically represent the alignment of the reference genome relative to a given assembly. This 
helps to visualize the coverage of the reference genome in the assembly. To find more information about the output files 
refer to the documentation.

#### 6.3.4 Assembly QUAST

QUAST is used to generate a single report with which to evaluate the quality of the de novo assemblies across all of the 
samples provided to the pipeline. The HTML results can be opened within any browser (we recommend using Google Chrome). 
Please see the QUAST output docs for more detailed information regarding the output files.

#### 6.3.5 Call variants relative to reference

##### Minimap2

assembly-to-assembly alignment

##### Seqwish

Used with Minimap2 lossless conversion from pairwise alignments between sequences to a variation graph encoding the sequences and their alignments
Seqwish was used to induce a genome variation graph from all-versus-all alignments between scaffold assembly contigs and contigs from a reference genome.

#### vg

vg is a collection of tools for working with genome variation graphs. vg was used to call variants from the genome 
variation graph induced from all-versus-all alignments between scaffold assembly contigs and contigs from a reference 
genome.

### 7. QC and visualization for raw read, alignment, assembly and variant calling results

#### 7.1 Bandage

Bandage, a Bioinformatics Application for Navigating De novo Assembly Graphs Easily, is a GUI program that allows users 
to interact with the assembly graphs made by de novo assemblers and other graphs in GFA format. Bandage was used to 
render induced genome variation graphs as static PNG and SVG images.

`(base) \[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$`
