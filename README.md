# genomic-interval-enrichment-analysis-for-extreme-heterozygote-excess-HetExc-variants

## Intro

Deviations from Hardy-Weinberg Equilibrium (HWE) are not solely rooted in biological or genetic factors; sequencing/mapping errors play a significant role. Previous studies have underscored extreme heterozygote excess (HetExc) for specific variants in unstable genomic regions, suggesting the influence of genomic instability on observed deviations from HWE. Given the importance of variants occurring in coding regions for germline and somatic variant classification, we aimed at genomic intervals within the human exome enriched for HetExc variants. The identified regions could serve as indicators of potential spurious mutations, providing essential guidance for variant classification in genomic research.

## Methods

### Defining genomic intervals with HetExc variants

**Obtaining gencode.v44.annotation and processing** The file was downloaded from [GENCODE](https://www.gencodegenes.org/human/), and further processed to create a subset of exons from protein-coding Ensembl canonical transcripts. The processing details are outlined in the `gtf2bed.ipynb` notebook, in the scripts directory.

**Obtaining vcf files and processing**

We obtained exome and genome VCF files from gnomAD v4 (accessed on 2023-11-16) and processed them on HPC clusters provided by the Digital Research Alliance of Canada (the Alliance). The processing included the following steps: 1. Obtaining VCF files for chromosomes 1-22 and X.

2.  Filtering variants to retain only HetExc variants (indicated by an inbreeding coefficient ≤ -0.3).

3.  Intersecting VCF files with exon coordinates obtained from GENCODE.

4.  Uniting proximal intervals (+/- 300 bp), then filtering united intervals with a variant count of less than two.

The Bash script used for processing exome VCF files can be found in the scripts directory under the name gnomad_proc.sh.

**Annotating the identified intervals**

## Results

The results section presents details regarding our analysis of exome VCF files. If genome VCF files were considered for analysis, we explicitly indicate that. Therefore, the results are primarily based on exome VCF files unless stated otherwise.

### HetExc interval count

We ordered genes based on the number of HetExc intervals identified in them to see which genes have the highest level of HetExc intervals. The table below represents our findings, where it can be seen that *MUC3A* is the top one, harboring a total of eight intervals residing in four exons.

| Gene Symbol      | Transcript ID      | HetExc interval count | Affected exon IDs                                                                                                |
|------------------|--------------------|-----------------------|------------------------------------------------------------------------------------------------------------------|
| *MUC3A*          | ENST00000379458.9  | 8                     | ENSE00003733255.1, ENSE00001760877.2, ENSE00003728907.1, *ENSE00003711593.1*                                     |
| ENSG00000241489  | ENST00000651111.1  | 7                     | ENSE00003850317.1                                                                                                |
| *FGF13*          | ENST00000315930.11 | 7                     | ENSE00003765119.2, ENSE00001124987.8                                                                             |
| *IDS*            | ENST00000340855.11 | 7                     | ENSE00001368349.7                                                                                                |
| *ANKRD36C*       | ENST00000295246.7  | 6                     | ENSE00003650405.1, ENSE00003564919.1, ENSE00002490625.1, ENSE00002460612.1, ENSE00002471049.1, ENSE00002526875.1 |
| *FCGBP*          | ENST00000616721.6  | 6                     | ENSE00003727252.1, ENSE00003736302.1, ENSE00003726191.1, ENSE00003713265.1, ENSE00003717014.1, ENSE00003742840.1 |
| *MBNL3*          | ENST00000370853.8  | 6                     | ENSE00002203352.2                                                                                                |
| *MUC4*           | ENST00000463781.8  | 6                     | ENSE00001854802.1                                                                                                |
| *NBPF1*          | ENST00000430580.6  | 6                     | ENSE00003524745.1, ENSE00003641270.1, ENSE00003610699.1, ENSE00001784848.1, ENSE00003477087.1, ENSE00001765538.1 |
| *AFF2*           | ENST00000370460.7  | 5                     | ENSE00001452771.2                                                                                                |
| *ANKRD36*        | ENST00000420699.9  | 5                     | ENSE00002456047.1, ENSE00002485219.1, ENSE00002463900.1, ENSE00002496065.1, ENSE00003626869.1                    |
| *CNTNAP3B*       | ENST00000377561.7  | 5                     | ENSE00003468443.1, ENSE00003676979.1, ENSE00003669769.1, ENSE00003522083.1, ENSE00001844388.3                    |
| *ARMCX5-GPRASP2* | ENST00000652409.1  | 4                     | ENSE00003842799.1                                                                                                |
| *DCX*            | ENST00000636035.2  | 4                     | ENSE00003900373.1                                                                                                |
| *GPRASP3*        | ENST00000457056.6  | 4                     | ENSE00001458491.1                                                                                                |
| *HRNR*           | ENST00000368801.4  | 4                     | ENSE00001447986.4                                                                                                |
| *KMT2C*          | ENST00000262189.11 | 4                     | ENSE00003680506.1, ENSE00003668340.1, ENSE00003571980.1, ENSE00001783379.1                                       |
| *LANCL3*         | ENST00000378619.4  | 4                     | ENSE00001957816.2                                                                                                |
| *LILRA2*         | ENST00000391738.8  | 4                     | ENSE00003624947.1, ENSE00003459519.1, ENSE00003895381.1, ENSE00003894905.1                                       |
| *MAP2K3*         | ENST00000342679.9  | 4                     | ENSE00003566030.1, ENSE00003656597.1, ENSE00003544515.1, ENSE00003553037.1                                       |

### HetExc interval variant density

A total of 631 HetExc intervals were identified, with a median size of 89 bp (interquartile range: 227), harboring 2 to 113 HetExc variants. There are cases with HetExc interval length \> 1 kb, with a significant number of variants in them. The table below shows cases where the HetExc interval length is over 1 kb, along with the number of variants identified in them.

To further prioritize the intervals, we calculated a weighted variant density score. The approach involves filtering intervals with a length less than 20bp then the weighted variant density ($wvd_i$) for interval $i$ is calculated using the formula:

$$ wvd_i = \log_{10} \left( \frac{v_i}{\sqrt{\frac{l_i}{v_i^2}}} \right) $$

where $v_i$ represents the variant count in interval $i$ and $l_i$ is the length of the interval in base pairs (bp). This formula utilizes a logarithmic transformation to emphasize variations in variant density while considering the size of the interval.

| Gene Symbol | Transcript ID     | Exon ID           | HetExc.chr | HetExc.start | HetExc.end | HetExc.nCount | HetExc.length |
|-------------|-------------------|-------------------|------------|--------------|------------|---------------|---------------|
| *ZNF717*    | ENST00000652011.2 | ENSE00003849189.2 | chr3       | 75736810     | 75738889   | 77            | 2079          |
| *MUC3A*     | ENST00000379458.9 | ENSE00003733255.1 | chr7       | 100959267    | 100960873  | 60            | 1606          |
| *GPRIN2*    | ENST00000374314.6 | ENSE00003923399.1 | chr10      | 46549129     | 46550723   | 28            | 1594          |
| *MUC6*      | ENST00000421673.7 | ENSE00001739408.1 | chr11      | 1016608      | 1018523    | 113           | 1915          |
| *OR9G1*     | ENST00000642097.1 | ENSE00003812474.1 | chr11      | 56700404     | 56701554   | 59            | 1150          |
| *KCNJ12*    | ENST00000583088.6 | ENSE00002732101.3 | chr17      | 21415290     | 21416967   | 56            | 1677          |
| *MAGEB16*   | ENST00000399988.6 | ENSE00003978144.1 | chrX       | 35802147     | 35803191   | 9             | 1044          |

#### Are HetExc intervals depleted for other class of variants?
