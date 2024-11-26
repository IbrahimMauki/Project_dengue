# Programming exercise tutorial  
The following is the stepwise procedure that to be used in working on the programming exercise shared.

## Getting started
### Creating neccessary directories and required files

1. Create your project directory and name it `Project_dengue` making sure it has the necessary subdirectories needed for any bioinformatics project
```sh
    mkdir -p Project_dengue/{raw_data/{compressed},processed_data,scripts}
```
2. To put the downloaded zip file into the compressed subdirectory inthe raw data and unzipping.

* First move into `compressed` directory by
```sh
    cd Project_dengue/raw_data/compressed/
```
* Then copy the file using `cp` command as bellow
```sh
    cp /mnt/c/Users/micro/Desktop/Msc/lisso/dengue.zip .
```
* Exctract it by the `unzip` command which can be installed by `sudo apt install unzip` when not installed. 
* Use `unzip` with `-d` option when specificing the output directory.
```sh
    unzip dengue.zip -d /home/genomics/Project_dengue/raw_data/
```
## Exercise

### Qn1: How manyfiles are in the zipped file?
Use `unzip` command with `-l` option to get a summary of the zipped file without extracting it.
```sh
    unzip -l dengue.zip
```
**Output**
```
Archive:  ./dengue.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     2795  2024-11-19 09:23   dengueseq1.fasta
     8006  2024-11-19 09:23   dengueseq2.fasta
     6476  2024-11-19 09:24   dengueseq3.fasta
     7844  2024-11-19 09:24   dengueseq4.fasta
    10936  2024-11-19 09:20   dengueseq5.fasta
---------                     -------
    36057                     5 files
```

### Qn2: How manylines are in each file?
Use `wc` with `-l` to count lines only in each file with `.fasta` extension in the `raw_data` directory where extracted files were placed.
```sh
    wc -l *.fasta
```
**Output**
```
  42 dengueseq1.fasta
  115 dengueseq2.fasta
   94 dengueseq3.fasta
  113 dengueseq4.fasta
  157 dengueseq5.fasta
  521 total
  ```
### Qn3: How manylines are in all the files combined?
Use the `cat` to combined all files and pipe `|` the output to count all lines combined by `wc -l`
```sh
    cat *.fasta | wc -l
```
**Output**
```
521
```

### Qn4: Mergethe files into one file and name it dengue_merged.fasta
Use `cat` command and redirect output to new the name.
```sh
    cat *.fasta > dengue_merged.fasta
```
### Qn5: How manyheaders does the new file have?
Use `grep` command with flag `-c`

```sh
    grep -c ">" dengue_merged.fasta
```

**Output**
```
5
```

### Qn5: How many sequences does the new file have?
Use `grep` command with flag `-c`
```bash
    grep -c ">" dengue_merged.fasta
```
**Output**
```
5
```

### Qn6: Extract the headers, and put them in a new file called dengue_headers.txt
Use `grep` command and redirect the of output to a new folder. You can use `cat` to view contents of the new file `dengue_headers.txt`
```bash
    grep ">" dengue_merged.fasta > dengue_headers.txt
```
### Qn7:a  Extract only the names of the viruses, and create a new file called `viruses.txt`
Use both `awk` to search for columns and pipe output to `sed` to remove identfires.
```bash
    awk -F '[>,]' '{print $2}' dengue_headers.txt | sed 's/^[^ ]* //' > viruses.txt
```

### Qn7:b  Extract only the unique identifiers and create a new file called `identifiers.txt`

Use the `awk` command to sort columns
```sh
    awk -F '>' '{print $2}' dengue_headers.txt | awk '{print $1}' > identifiers.txt
```
### Qn8: Create a file for only the sequences. Name it `dengue_seq.txt`
Use `grep -v` to invert the output or `sed` with `d` option to to delete all headers.
```sh
    grep -v '^>' dengue_merged.fasta > dengue_seq.txt
```
 or
 ```sh 
    sed '/^>/d' <dengue_merged.fasta > dengue_seq.txt
```
### Qn9:  Replace the values in `dengue_seq.txt` with small letters
Use the `tr` command to translate from upper to lower case.
```sh
    tr '[:upper:]' '[:lower:]' < dengue_seq.txt > dengue_seq_lowercase.txt
```
## ***Done*** :pen: