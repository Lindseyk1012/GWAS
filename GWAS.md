# GWAS Analysis in Plink 
# Download Files
map File:
```sh
/scratch/lindseyKukoly/GWAS/gwas_data.map
```
ped File:
```sh
/scratch/lindseyKukoly/GWAS/gwas_data.ped
```
covar File:
```sh
/scratch/lindseyKukoly/GWAS/gwas_samples.covar
```

# Quality Control 
Check for Sex Discrepancies 
```sh
plink --file gwas_data --check-sex --recode --out check_sex
```
Combine Data
```sh
plink --covar gwas_samples.covar --file gwas_data --recode --out input_data
```

Check for Missingness
```sh
plink --file input_data --geno .10 --recode --out geno_removed
```

```sh
plink --file geno_removed --mind .10 --recode --out missing_removed
```

Hardy-Weinberg Equilibrium 
```sh
plink --file missing_removed --hwe 0.05 --recode --out hwe_removed
```

Minor Allele Frequency 
```sh
plink --file hwe_removed --maf 0.01 --recode --out maf_removed 
```

Filter Chromosomes
```sh
plink --file maf_removed --chr 1-22 --recode --out autosomal_chromosomes
```

Check for Crytic Relatedness 
```sh
plink --file autosomal_chromosomes --genome --min .20 --recode --out relatedness_filtered
```
# Association Analysis 
Basic Association 
```sh
plink --file crytic --assoc --out GWAS_Output
```

Multi-covariate Association Analysis  
```sh
plink --adjust --ci .95 --file crytic --logistic --out GWAS_Output
```
