# GWAS Analysis in Plink 
# Download Files
map File:
```sh
scp /scratch/lindseyKukoly/GWAS/gwas_data.map .
```
ped File:
```sh
scp /scratch/lindseyKukoly/GWAS/gwas_data.ped .
```
covar File:
```sh
scp /scratch/lindseyKukoly/GWAS/gwas_samples.covar .
```

# Quality Control 
Check for Sex Discrepancies 
```sh
/usr/local/plink/plink --noweb --file gwas_data --check-sex --recode --out check_sex
```
Combine Data
```sh
/usr/local/plink/plink --noweb --covar gwas_samples.covar --file gwas_data --recode --out input_data
```

Check for Missingness
```sh
/usr/local/plink/plink --noweb --file input_data --geno .10 --recode --out geno_removed
```

```sh
/usr/local/plink/plink --noweb geno_removed --mind .10 --recode --out missing_removed
```

Hardy-Weinberg Equilibrium 
```sh
/usr/local/plink/plink --noweb --file missing_removed --hwe 0.05 --recode --out hwe_removed
```

Minor Allele Frequency 
```sh
/usr/local/plink/plink --noweb --file hwe_removed --maf 0.01 --recode --out maf_removed 
```

Filter Chromosomes
```sh
/usr/local/plink/plink --noweb --file maf_removed --chr 1-22 --recode --out autosomal_chromosomes
```

Check for Relatedness 
```sh
/usr/local/plink/plink --noweb --file autosomal_chromosomes --genome --min .20 --recode --out relatedness_filtered
```
# Association Analysis 
Basic Association 
```sh
/usr/local/plink/plink --noweb --file relatedness_filtered --assoc --out GWAS_Output
```

Multi-covariate Association Analysis  
```sh
/usr/local/plink/plink --noweb --adjust --ci .95 --file relatedness_filtered --logistic --out GWAS_Output
```
