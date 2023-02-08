# unix_assignment
## Beatriz Pereira - data inspection and processing of fang_et_al_genotypes.txt and snp_position.txt for BCB 546. 
### Attributes of 'fang_et_al_genotypes.txt' 
#### Here is the snippet of code used for data inspection, followed by what we learned about our file: 
```
wc fang_et_al_genotypes.txt
```
There are 2,783 lines/rows. 
```
awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```
There are 986 columns total. 
The the amount of lines and columns indicates that running "cat" is not the right course of action. 
```
head fang_et_al_genotypes.txt
```
Running "head" did not help determine what the headers could be. This is due to the amount of rows. 
```
head -n 1 fang_et_al_genotypes.txt
```
printing out the first line showed headers including Sample ID, JG_OTU, Group, and what looks to be SNP loci used in the study. Additionally, there are no initial comments in this file. 
```
tail fang_et_al_genotypes.txt
```
File ends with columns of alleles. 
```
less fang_et_al_genotypes.txt
```
Using the interactive command "less", it seems that for each column, there is the allele (homozygous vs. heterozygous), although it's still hard to be sure, due to the large number of columns.
```
ls -lh  fang_et_al_genotypes.txt 
```
This file is 11 megabytes. 
```
cut -f [column number] fang_et_al_genotypes.txt | uniq
```
The Sample_ID column consists IDs followed by numbers and letters. The group column includes TRIPS, ZDIPL, ZPERR, ZLUXR, ZMHUE, ZMPBA, ZMPJA, ZMXCH, ZMXCP, ZMXNO, ZMXNT, ZMPIL, ZMXIL, ZMMLR, ZMMMR, and ZMMIL. Loci begin at column 4 of the file, and end at column 968. Within those columns, there are allelses. Additionally, there are rows/columns with ?/? in place of alleles. 
```
file fang_et_al_genotypes.txt
```
This file is all ASCII text with very long lines - there shouldnt be any issues in respect to ASCII. 

### Attributes of 'snp_position.txt' 
#### Here is the snippet of code used for data inspection, followed by what we learned about out file: 
