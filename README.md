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

```
$ cut -f3 fang_et_al_genotypes.txt | sort | uniq -c 
```
This codes pulls out the third coloumn from the fang_et_all file, sorts the columns and gives us the count of all the unique characters. 
For MAIZE: ZMMIL = 290 | ZMMLR = 1256 | ZMMMR = 27 | Total = 1573
For TIOSINTE: ZMPBA = 900 | ZMPIL = 41 | ZMPJA = 34 | Total = 975

```
$ grep 'ZMM' fang_et_al_genotypes.txt > maize_genotypes.txt
$ grep 'ZMP' fang_et_al_genotypes.txt > teosinte_genotypes.txt
```
This greps and outputs all the maize and tiosinte into separate files. 

```
$ head -n 1 fang_et_al_genotypes.txt > headers_genotypes.txt
```
This command pulls out the header into another file. 

```
$ cat headers_genotypes.txt maize_genotypes.txt > maize_head_genotypes.txt
$ cat headers_genotypes.txt teosinte_genotypes.txt > teosinte_head_genotypes.txt
```
This step combines the headers file and genotypes file so that both the files now have the header to their individual files. 

 ```
 $ awk -f transpose.awk maize_head_genotypes.txt > transposed_maize_genotypes.txt
 $ awk -f transpose.awk teosinte_head_genotypes.txt > transposed_
 teosinte_genotypes.txt
 ```
 Transposes both the genotypes files to swap the rows and columns. 
 
 ```
 $ sort -k1,1 transposed.maize_genotypes.txt > sorted.transposed_maize_genotypes.txt
 $ sort -k1,1 transposed.teosinte_genotypes.txt > sorted.transposed_teosinte.genotypes.txt
 $ tail -n +2 snp_position.txt | sort -k1,1 snp_position.txt > sorted_snp_position.txt
 ```
 
 Sorted the transposed files and the SNP file based on the first column of "SNP_ID"
 
 ```
 $ sort -k1,1 -c sorted.transposed_maize_genotypes.txt
 $ echo $?
 $ sort -k1,1 -c sorted.transposed_teosinte_genotypes.txt
 $ echo $?
 $ sort -k1,1 -c sorted_snp_position.txt
 $ echo $?
 ```
 This is to check and confirm that these files sorted. Obtaining 0 after echo $ tells us that there were no errors. 
 
 ```
 $ join -1 1 -2 1 -t $'\t' sorted_snp_position.txt sorted.transposed_maize_genotypes.txt > joined_maize.txt
 $ join -1 1 -2 1 -t $'\t' sorted_snp_position.txt sorted.transposed_teosinte.genotypes.txt > joined_teosinte.txt
 ```
 
 Joined both the SNP and genotype files
 
 



###Teosinte Data

here is my snippet of code used for data processing
Here is my brief description of what this code does
