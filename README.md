# UNIX-Exercise

## DATA INSPECTION

### Attributes of `fang_et_al_genotypes.txt`

The code used for data inspection is as followed:

```
$ head fang_et_al_genotypes.txt
$ du -h fang_et_al_genotypes.txt
$ file fang_et_al_genotypes.txt
$ wc fang_et_al_genotypes.txt
```

By inspecting this file I learned that:

1. There was a lot going on in the file and I could not figure out how the file was separated into rows and columns. 
2. The size of this file is 6.1M
3. Through this, we were able to find out that our file ASCII text file that contains very long lines
4. There are a total of 2783 lines, 2744038 words and 11051939 bytes (characters in the file)

### Attributes to `snp_position.txt`

The code used for data insepction is as followed:

```
$ du -h snp_position.txt
$ file snp_position.txt 
$ grep -v "^#" snp_position.txt | awk -F "\t" '{print NF; exit}'
```

By inspecting this file I learned that:

1. The size of the file is 38K. 
2. Through this, we were able to find out that our file is a ASCII text file. 
3. There are a total of 15 columns in the file. 

## Data Processing

### Maize Data
In order to obtain Maize and Teosinte Genotypes in two different files, firstly we have to grep the similar ones together, add headers to them and transpose them, sort them and then join at the end. 

This greps and outputs all the maize and tiosinte into separate files. 
```
$ grep 'ZMM' fang_et_al_genotypes.txt > maize_genotypes.txt
$ grep 'ZMP' fang_et_al_genotypes.txt > teosinte_genotypes.txt
```

This command pulls out the header into another file. 

```
$ head -n 1 fang_et_al_genotypes.txt > headers_genotypes.txt
```

This step combines the headers file and genotypes file so that both the files now have the header to their individual files. 

```
$ cat headers_genotypes.txt maize_genotypes.txt > maize_head_genotypes.txt
$ cat headers_genotypes.txt teosinte_genotypes.txt > teosinte_head_genotypes.txt
```
 
Transposes both the genotypes files to swap the rows and columns. 
```
$ awk -f transpose.awk maize_head_genotypes.txt > transposed_maize_genotypes.txt
$ awk -f transpose.awk teosinte_head_genotypes.txt > transposed_teosinte_genotypes.txt
```

Sorted the transposed files and the SNP file based on the first column of "SNP_ID"

```
$ sort -k1,1 transposed_maize_genotypes.txt > sorted_transposed_head_maize_genotypes.txt
$ sort -k1,1 transposed_teosinte_genotypes.txt > sorted_transposed_head_teosinte_genotypes.txt
$ tail -n +2 snp_position.txt | sort -k1,1 > sorted_snp.txt
```

This is to check and confirm that these files sorted. Obtaining 0 after echo $ tells us that there were no errors. 

```
$ sort -k1,1 -c sorted_transposed_head_maize_genotypes.txt
$ echo $?
$ sort -k1,1 -c sorted_transposed_head_teosinte_genotypes.txt
$ echo $?
$ sort -k1,1 -c sorted_snp.txt
$ echo $?
```
Joins the SNP and Maize/Teosinte Genotype Dataset 
```
 $ join -1 1 -2 1 -t $'\t' sorted_snp.txt sorted_transposed_head_maize_genotypes.txt > combined_maize.txt
 $ join -1 1 -2 1 -t $'\t' sorted_snp.txt sorted_transposed_head_teosinte_genotypes.txt > combined_teosinte.txt
 ```
 
## Maize Data

```
$ awk -F "\t" '$3 ~ /1/' combined_maize.txt | sort -t $'\t' -k4,4n > chr1_maize.txt
$ awk -F "\t" '$3 ~ /2/' combined_maize.txt | sort -t $'\t' -k4,4n > chr2_maize.txt
$ awk -F "\t" '$3 ~ /3/' combined_maize.txt | sort -t $'\t' -k4,4n > chr3_maize.txt
$ awk -F "\t" '$3 ~ /4/' combined_maize.txt | sort -t $'\t' -k4,4n > chr4_maize.txt
$ awk -F "\t" '$3 ~ /5/' combined_maize.txt | sort -t $'\t' -k4,4n > chr5_maize.txt
$ awk -F "\t" '$3 ~ /6/' combined_maize.txt | sort -t $'\t' -k4,4n > chr6_maize.txt
$ awk -F "\t" '$3 ~ /7/' combined_maize.txt | sort -t $'\t' -k4,4n > chr7_maize.txt
$ awk -F "\t" '$3 ~ /8/' combined_maize.txt | sort -t $'\t' -k4,4n > chr8_maize.txt
$ awk -F "\t" '$3 ~ /9/' combined_maize.txt | sort -t $'\t' -k4,4n > chr9_maize.txt
$ awk -F "\t" '$3 ~ /10/' combined_maize.txt | sort -t $'\t' -k4,4n > chr10_maize.txt
```
### Missing data encoded by '-'

```
$ awk -F "\t" '$3 ~ /1/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr1d_maize.txt
$ awk -F "\t" '$3 ~ /2/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr2d_maize.txt
$ awk -F "\t" '$3 ~ /3/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr3d_maize.txt
$ awk -F "\t" '$3 ~ /4/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr4d_maize.txt
$ awk -F "\t" '$3 ~ /5/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr5d_maize.txt
$ awk -F "\t" '$3 ~ /6/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr6d_maize.txt
$ awk -F "\t" '$3 ~ /7/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr7d_maize.txt
$ awk -F "\t" '$3 ~ /8/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr8d_maize.txt
$ awk -F "\t" '$3 ~ /9/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr9d_maize.txt
$ awk -F "\t" '$3 ~ /10/' combined_maize.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr10d_maize.txt
```
### SNPs file with unknown position

```
$ awk -F '\t' '$4 == "unknown"' combined_maize.txt > unknwon_SNPs_maize.txt

```
### SNPs file with multiple position

```
$ awk -F '\t' '$4 == "multiple"' combined_maize.txt > multiple_SNPs_maize.txt
```
## Teosinte Data

```
$ awk -F "\t" '$3 ~ /1/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr1_teosinte.txt
$ awk -F "\t" '$3 ~ /2/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr2_teosinte.txt
$ awk -F "\t" '$3 ~ /3/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr3_teosinte.txt
$ awk -F "\t" '$3 ~ /4/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr4_teosinte.txt
$ awk -F "\t" '$3 ~ /5/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr5_teosinte.txt
$ awk -F "\t" '$3 ~ /6/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr6_teosinte.txt
$ awk -F "\t" '$3 ~ /7/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr7_teosinte.txt
$ awk -F "\t" '$3 ~ /8/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr8_teosinte.txt
$ awk -F "\t" '$3 ~ /9/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr9_teosinte.txt
$ awk -F "\t" '$3 ~ /10/' combined_teosinte.txt | sort -t $'\t' -k4,4n > chr10_teosinte.txt
```
### Missing data encoded by '-'

```
$ awk -F "\t" '$3 ~ /1/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr1d_teosinte.txt
$ awk -F "\t" '$3 ~ /2/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr2d_teosinte.txt
$ awk -F "\t" '$3 ~ /3/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr3d_teosinte.txt
$ awk -F "\t" '$3 ~ /4/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr4d_teosinte.txt
$ awk -F "\t" '$3 ~ /5/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr5d_teosinte.txt
$ awk -F "\t" '$3 ~ /6/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr6d_teosinte.txt
$ awk -F "\t" '$3 ~ /7/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr7d_teosinte.txt
$ awk -F "\t" '$3 ~ /8/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr8d_teosinte.txt
$ awk -F "\t" '$3 ~ /9/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr9d_teosinte.txt
$ awk -F "\t" '$3 ~ /10/' combined_teosinte.txt | sort -t $'\t' -k4,4n | sed 's/?/-/g' > chr10d_teosinte.txt
```
### SNPs file with unknown position

```
$ awk -F '\t' '$4 == "unknown"' combined_teosinte.txt > unknwon_SNPs_teosinte.txt

```
### SNPs file with multiple position

```
$ awk -F '\t' '$4 == "multiple"' combined_teosinte.txt > multiple_SNPs_teosinte.txt
``` 
 


