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
The Sample_ID column consists IDs followed by numbers and letters. The group column includes TRIPS, ZDIPL, ZPERR, ZLUXR, ZMHUE, ZMPBA, ZMPJA, ZMXCH, ZMXCP, ZMXNO, ZMXNT, ZMPIL, ZMXIL, ZMMLR, ZMMMR, and ZMMIL. Loci begin at column 4 of the file, and end at column 968. Within those columns, there are alleles. Additionally, there are unknown alleles, with ?/? in place of alleles. 
```
file fang_et_al_genotypes.txt
```
This file is all ASCII text with very long lines - there shouldnt be any issues in respect to ASCII. 

### Attributes of 'snp_position.txt' 
#### Here is the snippet of code used for data inspection, followed by what we learned about out file: 
```
wc snp_position.txt
```
This file has 984 lines/rows.
```
awk -F "\t" '{print NF; exit}' snp_position.txt
```
This file has 15 columns. 
Because this file is horrible huge, we can run cat for a first inspection. 
```
cat snp_position.txt 
```
I was wrong. Using cat did not give us an idea of how this file is organized due to the high number of data in this file. It seems that if any row gets pushed down, the data is immediately disorganized. The next best option is to print out our column headers. 
```
head -n 1 snp_position.txt
```
Our headers include SNP_ID, cdv_marker_id, Chromosome	Position, alt_pos (nucleotide location), mult_positions, amplicon,	cdv_map_feature.name,	gene,	candidate/random,	Genaissance_daa_id,	Sequenom_daa_id,	count_amplicons,	count_cmf, and	count_gene. 
```
ls -lh snp_position.txt 
```
This file is 81 kilobites - much smaller than the other file. 
```
file snp_position.txt
```
All ASCII text. 
### Data Processing
1) making our files with desired maize + teosinte data: 

Before processing, I wanted to make sure that "Group" was under $3:
```
awk '{ print $3 }' fang_et_al_genotypes.txt | head -n 3
```
This confirmed that Group is indeed under $3 assigned by awk. Similar to the 'cat' command, awk is being used here to show us the contents of a file. Specifically, column 3 of our file, which has been assigned $3. Head prints out the first 3 rows. 
Next, we want to create a folder with data solely on maize, which are groups ZMMIL, ZMMLR, and ZMMMR. 
```
awk '$3 ~ /Group|ZMMIL|ZMMLR|ZMMMR/' fang_et_al_genotypes.txt |cat > maize_fang.txt
```
Awk is a function that can be used for processing. Column $3 includes our desired groups (ZMMIL, ZMMLR, and ZMMMR) from our file. If awk matches our pattern, it will print out all of the content in our file (represented by $0). Because we want to move this information into a file, there is a pipe with the print feature 'cat', who's standard output goes into a new created file. 
Next, we will want to double check that this actually worked: 
```
cut -f 3 maize_fang.txt | uniq
```
The only groups that come up are ZMMIL, ZMMLR, and ZMMMR, success! 
```
less maize_fang.txt
```
It looks like we have our desired content for those groups. Next, we do the same for Teosinte:
```
awk '$3 ~ /Group|ZMPBA|ZMPIL|ZMPJA/' fang_et_al_genotypes.txt |cat > teosinte_fang.txt
```
As decribed above, we have made a new file with data that is exclusive to teosinte groups. I followed up with the previously te described step-checks. 

2) Before continuing, we need to prepare out data in order to join both files: 
```
awk -f transpose.awk maize_fang.txt > transposed_genotypes.txt
```
```
awk -f transpose.awk teosinte_fang.txt > transposed_genotypesteo.txt
```
We want folders with particular headers "SNP_ID", "Chromosome", and "Position" from our original file, so the next step is to make them. We also need to sort both files before joining them, so we can go ahead and sort the snp file, which includes the 3 headers:
```
cut -f 1,3,4 snp_position.txt | head -n 1 > snp_sort.txt 
```
This gave us our derised headers in the correct order through the previously described 'cut' of columns 1,3, and 4. The head command made sure we only pulled from the first line, where our headers are, and the output went into a new file. 
```
cut -f 1,3,4 snp_position.txt | tail -n +2 | sort -k1,1 -c >> snp_sort.txt
```
```
echo $?
```
All data from columns 1,3, and 4 got pulled. Next, the tail commaned removed the header of the file in order for the text data to be sorted (a step that is needed before joining files). We want it to be sorted by SNP_ID, which is column 1. The -c is there to check whether or not the command worked, and the data is sorted. A '0' means success! We don't want the contents of the file to be replaced, so we use >> instead of >. 
#### Maize Data
Sample_ID should be replaced by SNP_ID:
```
sed 's/Sample_ID/SNP_ID/' transposed_genotypes.txt | cat > maize_genotype.txt
```
This will replace any "Sample_ID" (there should only be 1) with SNP_ID. This can be checked by runnning 'less'. 
Next, 
BOTH the snp file, and maize genotype file need to be sorted in order to be joined. We previously sorted the snp_sorted.txt file, so now we need to sort the transposed_genotypes_final.txt file:
```
head -n +2 maize_genotype.txt | sort -k1,1 -c
```
#### Teosinte Data
Sample_ID should be replaced by SNP_ID:
```
sed 's/Sample_ID/SNP_ID/' transposed_genotypesteo.txt | head -n 1 | cat > transposed_genotypesteo_final.txt
```
Sort:
```
head -n +2 transposed_genotypesteo_final.txt | sort -k1,1 -c
```
