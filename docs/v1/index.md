# tfsites.AnnotateAndVisualizeTFSites v1

**Author(s):** Joe Solvason  

**Contact:** Joe Solvason (solvason@eng.ucsd.edu)

**Adapted as a GenePattern Module by:** Ted Liefeld (jliefeld@cloud.ucsd.edu)

**Task Type:** Transciption factor analysis

**LSID:**  urn:lsid:genepattern.org:module.analysis:00442


## Introduction

`AnnotateAndVisualizeTfSites` annotates transcription factor binding sites across a DNA sequence. Multiple transcription factors can be analyzed. Each binding site is labeled with the TF name and a unique binding site ID. If a relative affinity dataset from defineTfSites is provided for a transcription factor, the affinity of this site will be labeled and the intensity of the binding site’s color will be proportional to the affinity.


## Methodology

We iterate across every k-mer in the DNA sequence and identify those that conform to the IUPAC definition for transcription factor binding sites. For each binding site, we report its sequence, position, TF name, affinity (if PBM data is given), direction (“+” if it follows the given IUPAC and “-” if it follows the reverse complement of the IUPAC), and a unique ID. 

Using the binding sites identified in the DNA sequence, an image of the DNA sequence and all annotated binding sites is generated. Each binding site is plotted as a polygon that points in the direction of the site (right for positive, left for negative, and straight for a palindrome sequence).  

If the sequence is greater than 500 nucleotides in length, the annotation images are saved to multiple files. Each file contains a maximum of 500 nucleotides. 


## Parameters

<span style="color: red;">*</span> indicates required parameter

### Inputs and Outputs

- <span style="color: red;">*</span>**DNA sequence(s) to annotate (.tsv)**
    - This file contains one or more DNA sequences to be annotated. 
- **TF information (.tsv)**
    - This file contains all the information for the transcription factors being analyzed, including its name, binding site definition, desired color on the plot, any relative PBM affinity data, and any PFM score data.
- **all TF reference data**
    - File(s) referenced in the TF information file.     
- <span style="color: red;">*</span>**TF sites in DNA sequence table output filename (.tsv)**
    - Name of the output file containing the list of binding sites.
- <span style="color: red;">*</span>**TF sites in DNA sequence annotated image output filename  (.png)**
    - Base name of the output file for the plots. If the length of the sequence is greater than 500, the visualization will be broken up into multiple output files with the following name format: `[base name]_zoom=[start pos],[end pos].png`

### Other Parameters
- **plot dimensions (integer)**
    - `Default = 200`
    - Height and width of the image in inches, seperated by a comma. 
- **plot resolution (integer)**
    - `Default = 200`
    - Resolution of the plot, in dots (pixels) per inch
- **region of DNA to visualize (dash-separated string)**
    - `Default = None`
    - Given a start position and an end position, zoom into a portion of the sequence. The numbers in the range are inclusive. For example, the first 200 nucleotides of the sequence would be specified as: `1-200.`
    - If `region of DNA to visualize` is specified, then `number of bases included per plot` is ignored
- **number of bases included per plot (integer)**
    - `Default = 500`
    - Interval size (in nucleotides) used to partition the output plot. By default, the sequence will be divided into 500-nucleotide segments.
    - If `region of DNA to visualize` is specified, then `number of bases included per plot` is ignored

## Input File(s)

1.  DNA Sequences To Annotate (.tsv)
- Columns:
    - `Seq_name:` name of the DNA sequence
    - `Seq:` the sequence
 
```
seq_name	    seq
ZRS                 AACTTTAATGCCTATGTTTGATTTGAAGTCATAGCATAAAAGGTAACATAAGCAACATCCTGACCAATTATCCAAACCATCCAGACATCCCTGAATGGC...
Hand2_mm1689        CACCACTGGGTGATCCATAGTATGGAATATTTTTATGAGAAACAGCCACATAACATGTACCTGTTAATGTAGGCTTTGTGTTTATTTGCAATAGCAGAG...
```
    
2. Relative Affinity PBM data (.tsv)
- Columns
  - `Seq:` the sequence of every possible k-mer
  - `Rel_aff:` the relative affinity of the k-mer normalized to the max IUPAC k-mer

```
seq          rel_aff
AAAAAAAA     0.147
AAAAAAAC     0.107
AAAAAAAG     0.13
AAAAAAAT     0.125
AAAAAACA     0.123
```

       
## Output File(s)

1.  TF Sites Output Table (.tsv)
- Columns
    - `Seq_name:` Name of the sequence
    - `Kmer_id:` unique ID associated with each k-mer
    - `Kmer:` sequence of the k-mer
    - `Pos-1idx:` starting position of the k-mer
    - `Aff:` relative affinity of the k-mer
    - `Site_direction:` direction of the binding site (+ if it follows the given IUPAC or - if it follows the reverse complement of the IUPAC)
    - `Duplicate_kmer_seq_ids:` name of kmer IDs that have the same k-mer sequence

```
seq_name        kmer_id	         kmer        pos-1idx   aff      site_direction  duplicate_kmer_seq_ids
Hand2_mm1689    Hand2_mm1689:2	 ATGGAATA    22         0.103    +               Hand2_mm1689:2,22
Hand2_mm1689    Hand2_mm1689:3	 GAGGAACT    114        0.131    +	
Hand2_mm1689    Hand2_mm1689:4	 ATGGATTC    155        0.103    +	
Hand2_mm1689    Hand2_mm1689:5	 TGATCCTA    344        0.095    -	
Hand2_mm1689    Hand2_mm1689:6	 AATTCCAT    494        0.111    -               Hand2_mm1689:17,6

```

2.  Annotated Sequence Image(s)  (.png)

- ZRS sequence: 
   <img src="./04-output_vis-sites-zrs-start_pos=0.png"/>

- Hand2_mm1689 sequence:
   <img src="./04-output_vis-sites-hand2-start_pos=0.png"/>
    
  
## Example Data

[Example input data is available on github](https://github.com/genepattern/tfsites.annotateTfSites/data)

    
## Version Comments

- **1.0.0** (2023-01-12): Initial draft of document scaffold.
- **1.0.1** (2024-02-02): Draft completed.
