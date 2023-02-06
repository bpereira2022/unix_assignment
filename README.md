# unix_assignment
## Beatriz Pereira - data inspection and processing of fang_et_al_genotypes.txt and snp_position.txt for BCB 546. 
### Attributes of fang_et_al_genotypes.txt
#### Here is the snippet of code used for data inspection: 
```
wc fang_et_al_genotypes.txt
```
```
awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```
```
head fang_et_al_genotypes.txt
```
```
head -n 1 fang_et_al_genotypes.txt
```
```
tail fang_et_al_genotypes.txt
```
```
less fang_et_al_genotypes.txt
```
```
ls -lh  fang_et_al_genotypes.txt 
```
#### By inspecting this file, I learned that (In preceeding order): 
- There are 968 columns total. Additionally, 2,783 lines/rows. The the amount of lines and columns indicates that running "cat" is not the right course of action. 
- Running "head" did not help determine what the headers could be - printing out the first line showed headers including Sample ID, JG_OTU, Group, and what looks to be SNP loci used in the study. Additionally, there are no comments in this file. 
- Using the interactive command "less", it seems that for each column, there is an allele, although it's hard to be sure, due to the large number of columns. 
- File ends with alles. 
- This file is 11 megabytes. 
- 
