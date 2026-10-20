## AWK script for filtering VCF files and calling genotypes based on DV/DP ratios 

## Input file format
The script reads VCF files and assumes that

1. the genotype field has DP and DV subfields;
2. the DP is the 3th subfield of the genotype field;
3. the DV is the 4th subfield of the genotype field;
4. the INFO field (column 8) has DP and MQ subfields.

## Usage example
```shell
filter_vcf.awk \
  -v dphom=2 \
  -v dphet=4 \
  -v minqual=40 \
  -v mindp=100 \
  -v minhomn=1 \
  -v minhomp=0.9 \
  -v tol=0.2 \
  -v minmaf=0.01 \
  -v minpresent=0.9 VCF_FILE
```

## Parameters:
* dphom ... minimum read depth to accept a homozygous genotype call
* dphet ... minimum read depth to accept a heterozygous genotype call
* minqual ... minimum SNP quality (6th column of the VCF)
* mindp ... SNPs with a total depth (parsed from DP subfield of the INFO 
  field) will be discarded.
* minhomn ... SNPs with fewer than minhomn homozygous calls for the REF 
  and/or the ALT allele will be discarded.
* minhomp ... SNPs with a fraction of heterozygous calls among all 
  present genotye calls exceeding 1 - minhomp will be discarded.
* tol ... Genotypes with DV/DP <= tol will be called 0/0; genotypes with 
  DV/DP >= 0.5 - tol & DV/DP <= 0.5 + tol will be called 0/1; and 
  genotypes with DV/DP >= 1 - tol will be called 1/1.
* minmaf ... SNPs with a minor allele frequency below minmaf
  will be discarded.
* minpresent ... SNPs with a fraction of present data less than 
  minpresent will be discarded.

