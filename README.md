# TDMD in C.elegans pipeline
## 1 TDMD identification analysis
### 1.1 Cutadapt （https://cutadapt.readthedocs.io/en/stable/） in FASTQ
cutadapt -a TGGAATTCTCGGGTGCCAAG -A GATCGTCGGACTGTAGAACT -o `test_R1_cut.fastq` -p `test_R2_cut.fastq` `test_R1.fastq test_R2.fastq` --minimum-length 18 -j 10

### 1.2 Pear (https://cme.h-its.org/exelixis/web/software/pear/doc.html) in FASTQ
pear -f `test_R1_cut.fastq` -r `test_R2_cut.fastq` -j 10 -o `test`

### 1.3 Collapse (http://hannonlab.cshl.edu/fastx_toolkit/) PCR duplicated reads
fastx_collapser  -i `test.assembled.fastq` -o `test_collapsed.fasta`

### 1.4 Remove UMI sequences from 5' and 3' of FASTA
cutadapt -u 4 -u -4 -m 18 `test_collapsed.fasta` -o `test_cutN.fasta` -j 10              

### 1.5 Run hyb(https://github.com/gkudla/hyb) software
module load hyb

module load unafold

hyb analyse in=`test_cutN.fasta` db=`20230616martquery_0613142828_717_WBcel235_unique` type=mim pref=mim format=fasta

### 1.6 Potential TDMD miRNA-target RNA hybrids identification
python3 CLASH.py Viennad_to_Table -i `test_cutN_comp_20230616martquery_0613142828_717_WBcel235_unique_hybrids_ua` -c `20230618_Exp325_Celegans_CS.txt` -t `20230616martquery_0613142828_717_WBcel235_unique.fasta` -n `EXP325_20230618_C.elegans_name.txt`

```ruby
20230616martquery_0613142828_717_WBcel235_unique.fasta file size is 66.1MB. The dataset folder has a small size file, called 20230616martquery_0613142828_717_WBcel235_unique_small.fasta.
  
20230618_Exp325_Celegans_CS.txt file size is 900MB. The dataset folder has a small size file, called 20230616martquery_0613142828_717_WBcel235_unique_small.txt.
```

  
