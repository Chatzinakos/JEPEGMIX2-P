# JEPEGMIX2-P

# Download JEPEGMIX2
The current release (Version 0.2.0) of JEPEGMIX2-P is for a Linux user. The pre-compiled executables for other operating systems (e.g.,  Windows, MacOS) will be available soon. The latest source codes of JEPEGMIX2-P are available upon request (christos.chatzinakos@downstate.edu). 

|**Direct download link**|**Versiom**|**Release Date**|**Note**|
|:---|:---|:---|:---|
|[JEPEGMIX2-P (jepegmix2_P2) for a Linux user](https://www.dropbox.com/sh/tiedrhw8ncbl01n/AAAQpphs1OWHZPj2397n0t-ta?dl=0)|v02.1.0|09/06/2024|Includes info file for several scenarios|

# Download Reference Panels
|**Direct download link**|**Number of Samples**|**Number of populations**|**NCBI build**|**Release Date**|**Note**|
|:---|:---|:---|:---|:---|:---|
|[33Kg](https://www.dropbox.com/sh/sw03zojcwzpdeed/AACucpdpPMlbtVCp9zrwXLATa?dl=0)|32953|29|build 37 (hg19)|Jul. 16 2018|Includes chr1-chr22|

Here are the 33Kg population abbreviations used by JEPEGMIX2-P. AFR is an abbreviation for African; AMR for admixed American; ASN for East Asian; EUR for European and SAS for South Asian. 

|**Population Abbreviation**|**Number of Subjects**|**Super Population**|**Population Description**|
|:---|:---|:---|:---|
|ACB|164|AFR|African Caribbeans in Barbados|
|ASW|162|AFR|African Ancestry in Southwest US|
|BEB|86|SAS|Bengali from Bangladesh|
|CCE|3409|ASN|China Central East|
|CCS|2613|ASN|China Central South|
|CDX|95|ASN|Chinese Dai in Xishuangbanna, China|
|CEU|6360|EUR|Utah residents with Northern and Western European ancestry| 
|CLM|98|AMR|Colombians from Medellin, Colombia|
|CNE|2330|ASN|China North East|
|CSE|2020|ASN|China South-East|
|ESN|140|AFR|Esan in Nigeria|
|FIN|3529|EUR|Finnish in Finland|
|GBR|2020|EUR|British in England and Scotland|
|GIH|110|SAS|Gujarati Indian from Houston, Texas|
|GWD|113|AFR|Gambian in Western Divisions in the Gambia|
|IBS|1309|EUR|Iberian Population in Spain|
|ITU|95|SAS|Indian Telugu from the UK|
|JPT|107|ASN|Japanese in Tokyo, Japan|
|KHV|226|ASN|Kinh in Ho Chi Minh City, Vietnam|
|LWK|99|AFR|Luhya in Webuye, Kenya|
|MSL|87|AFR|Mende in Sierra Leone|
|MXL|187|AMR|Mexican Ancestry from Los Angeles, USA|
|ORK|5772|EUR|Orkney Island study|
|PEL|110|AMR|Peruvians from Lima, Peru|
|PJL|121|SAS|Punjabi from Lahore, Pakistan|
|PUR|138|AMR|Puerto Rican in Puerto Rico|
|STU|110|SAS|Sri Lankan Tamil from the UK|
|TSI|1291|EUR|Toscani in Italia|
|YRI|52|AFR|Yoruba in Ibadan, Nigeria|

# Download SNP Annotation Data

|**Direct download link**|**Version**|**Release Date**|
|:---|:---|:---|
|[SNP annotations](https://www.dropbox.com/sh/doxfref8zvempr8/AACBg0rMUJtxN7X26G707SiFa?dl=0)|v2.1.0|09/06/2024|

# JEPEGMIX2-P Input File Format
###JEPEGMIX2-P takes as input a plain text file with rows and columns denoting SNPs and variables, respectively. The first line of the input file should be column names/headers. Data entries on each line should be separated by white space. 
The file should contain six columns, and optional another one: 1) rsid (SNP ID), 2) chr (chromosome number), 3) bp (base pair position), 4) a1 (reference allele), 5) a2 (alternative allele), 6) z (normally distributed GWAS/meta-analysis summary statistic, i.e. two-tailed Z-score) and 7) AF (Allele Frequency information, Optional)). JEPEGMIX2-P does not require the input data to be sorted in ascending order by chromosome number and base pair position or SNP ID. Here is a sample JEPEGMIX2-P input file.

```
rsid        chr  bp         a1  a2  z          AF    

rs1000109   9    117908733  C   T  -0.714464   0.89  

rs10001109  4    44904653   C   T   0.721919   0.40   

rs1000112   22   28273339   C   G  -0.583666   0.24 

rs10001127  4    130547376  T   C   0.069329   0.09    

rs1000113   5    150240076  T   C  -1.447288   0.33  
```
**DISTMIX2 (version.0.2.0) imputation:** If your input summary data is generated from non-imputed genotype data (i.e. non-imputed GWASs or exome sequencing studies), you may want to impute summary statistics of unmeasured functional variants based on external panels (e.g. 33Kg). By using "--impute" option, you can make the software impute them using DISTMIX2 before performing JEPEGMIX2-P analysis. In this case, the software assumes that all SNPs in your input file are measured with Info=1.

**Note:** If the input summary data comes a different genome assembly (e.g. NCBI build 36 (hg18)) than NCBI build 37 (hg19), it needs to be converted by the user to NCBI build 37 (hg19) using a software, called [liftover](https://genome.ucsc.edu/cgi-bin/hgLiftOver), from UCSC.

**When study allele frequency information is not available**

Genome-wide association studies/meta-analyses typically do not provide study allele frequency information due to privacy issues. However, information about ethnic proportion of the study samples is usually available from the publications. By using the prior ethnic information, users can run JEPEGMIX2-P for association summary data lacking allele frequency information. In this case, JEPEGMIX2-P does not require study allele frequency information (the column ‘AF’) in the input file, but optionally JEPEGMIX2-P needs one more input text file (super population weight file) specifying ethnic proportion of the study samples.

Here is a sample JEPEGMIX2-P Super Population weight file, which is **Strongly recommended if your GWAS has not have AF column.** 


```
Super_Pop	 Wgt   
EUR       	0.4
ASN	        0.6
```
The first line of the super-population weight file should be column names/headers. The file should contain two columns: 1) SuperPop (Super population abbreviation) and 2) Wgt (Super population proportion). The columns of data should be separated by white space. The weight file name should be specified with the ‘--populationWeight’ or ‘-w’ option. The weights must be possitives and the summation of all weights must be 1. Practically you put the Super population proportion of the GWAS study.

# JEPEGMIX2-P output file format for genes analysis

The JEPEGMIX2-P output file, for genes analysis, has twelve columns: 1) type name (Type), 2) tissue name (Tissue), 3) gene name (Geneid), 4) chromosome (Chr), 5) bare paise start (bp_s), 6) bare paise end (bp_e), 7) number of functional SNPs associated with gene (Num_SNPs), 8) pre sestimated q value (Q), 9) JEPEGMIX2-P z-value (Zval) 10) JEPEGMIX2-P p-value (Pval) 11) JEPEGMIX2-P q-value with Holm method (Qval_holm), 12) JEPEGMIX2-P q-value with FDR method (Qval_fdr). The first line of the output file is column names/headers.  Here is a sample JEPEGMIX2-P genes output file, with 3 genes:

```
Type          Tissue              NAME    Chr   bp_s        bp_e    Num_SNPs     Q        Zval       Pval     Qval_holm  Qval_fdr

Gene   TW_Artery_Coronary_0.5      FABP3    1   30852689   32699227   76     0.0127005    1.85878   0.0630578      1   0.99  

Gene   TW_Artery_Coronary_0.5      TXLNA    1   32480657   32480657   1      0.00194273  -1.72314   0.0848635      1   1  

Gene   TW_Artery_Coronary_0.5      DCDC2B   1   32160918   33610468   11     0.0205775    0.996671  0.318924       1   1

```

# JEPEGMIX2-P output file format for pathway analysis

The JEPEGMIX2-P output file, for pathway analysis, has ten columns: 1) type name (Type), 2) tissue name (Tissue), 3) gene name (Geneid), 4) degrees of freedom (df), 5) chi-square (Chisq), 6) JEPEGMIX2-P p-value (Pval) 7) JEPEGMIX2-P q-value with Holm method (Qval_holm), 8) JEPEGMIX2-P q-value with FDR method (Qval_fdr), 9) JEPEGMIX2-P Direction_of_Pathway (Direction_of_Pathway) and 10) JEPEGMIX2-P significant genes (Significant_genes). The first line of the output file is column names/headers.  Here is a sample JEPEGMIX2-P pathways output file, with 3 pathways:

```
Type          Tissue         NAME                                      df   Chisq      Pval     Qval_holm   Qval_fdr Direction_of_Pathway Significant_genes

Path_c Br_Put_bas_gang    GO_CYTOPLASMIC_MRNA_PROCESSING_BODY           7    8.06931   0.326522    1           1         8.17711   NA
Path_c Br_Put_bas_gang    GO_CYTOPLASMIC_MRNA_PROCESSING_BODY_ASSEMBLY  1    1.12329   0.289211    1           1         1.12329   NA
Path_c Br_Put_bas_gang    GO_CYTOPLASMIC_REGION                         39   19.247    0.996651    1           1        -5.27472   NA
```

# Programm Options

|**Option**|**Short Flag**|**Parameter**|**Default**|**Description**|
|:---|:---|:---|:---|:---|
|--version|-v|none|none|Prints version information.|
|--help|-h|none|none|Outputs a full description of all JEPEGMIX2-P options.|
|--impute|-p|none|FALSE|Imputes summary statistics of unmeasured functional SNPs using DISTMIX2 imputation before JEPEGMIX2-P analysis.|
|--reference|-r|filename|none|The filename of the reference population data.|
|--referenceIndex|-i|filename|none|The filename of the reference population index data.|
|--annotation|-a|filename|none|The filename of the SNP annotation data set.|
|--output|-o|filename|out.gene.jepegmix2.txt|The genes filename of JEPEGMIX2-P output.|
|--zpath|-x|filename|out.path.jepegmix2.txt"|The pathways filename of JEPEGMIX2-P output.|
|--populationWeight|-w|filename|none|The filename of the super population weight data.|
|--windowSize|-n|decimal|0.5|The size of the DISTMIX2 imputation prediction window (Mb).|
|--wingSize|-m|decimal|0.25|The size of the wing added on the left and right of the DISTMIX2 imputation prediction window (Mb).|
|--snp_el|none|none|FALSE|Conditional analysis flag.|
|--measured_snps|-z|integer|50|Number of measured Snps.|
|--rmv|none|none|FALSE|Rare variant flag.|
|--snps_perc|-s|decimal|0.75|SNPs Percentage belong to the model|
|--exc|none|none|none|Which chr(s) and interval(s) to exclude (--exc 6:25-35,5:10-20)(Position is in MB), only for pathway analysis.|
|--gnexc|none|none|none|Which gene include (--gnexc C4A,NOC2L), only for pathway analysis.|
|--chromosome|-c|none|none|The chromosome number (or the chromosome number and arm, e.g., -c 1q), only for gene analysis|
