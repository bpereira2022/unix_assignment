# unix_assignment
## Beatriz Pereira - data inspection and processing of fang_et_al_genotypes.txt and snp_position.txt for BCB 546. 
### Attributes of 'fang_et_al_genotypes.txt' 
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
```
cut -f [column number] fang_et_al_genotypes.txt | uniq
```
```
file fang_et_al_genotypes.txt
```
#### By inspecting this file, I learned that (In preceeding order to the above code): 
- There are 986 columns total. Additionally, 2,783 lines/rows. The the amount of lines and columns indicates that running "cat" is not the right course of action. 
- Running "head" did not help determine what the headers could be - printing out the first line showed headers including Sample ID, JG_OTU, Group, and what looks to be SNP loci used in the study. Additionally, there are no comments in this file. 
- Using the interactive command "less", it seems that for each column, there is the original base and what it has switched to (base change, ex.), although it's hard to be sure, due to the large number of columns. 
- File ends with alles. 
- This file is 11 megabytes. 
- The Sample_ID column consists IDs followed by numbers and letters. The group column includes TRIPS, ZDIPL, ZPERR, ZLUXR, ZMHUE, ZMPBA, ZMPJA, ZMXCH, ZMXCP, ZMXNO, ZMXNT, ZMPIL, ZMXIL, ZMMLR, ZMMMR, and ZMMIL. Loci begin at column 4 of the file, and end at column 968. Within those columns, there are base changes. Additionally, there are rows/columns with ?/? in place of base change. 
- This file is all ASCII text with very long lines

### Attributes of 'snp_position.txt' 
#### Here is the snippet of code used for data inspection: 
