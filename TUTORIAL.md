Tutorial
========

A quick tutorial to use MITGARD

- Download the available paired-end dataset SRR594407 of RNA-seq from Skeletal Muscle of *Mus musculus* from the SRA using [fastq-dump](https://ncbi.github.io/sra-tools/fastq-dump.html).
  ```
  fastq-dump --outdir folder/to/save/the/SRA/ --gzip --skip-technical --readids --read-filter pass --dumpbase --split-files --clip SRR594407
  ```
  The command above will download the fasq files with the reads processed (adapters removed and low-quality reads fitered).



- Download the mitochondrial genome of *Mus musculus* at the NCBI (Accession number [NC_005089](https://www.ncbi.nlm.nih.gov/nuccore/NC_005089.1/)) in fasta format and save it as ```Mmusculus_NC_005089.fasta```. 

  - Clean the header of the fasta (delete all strings after the first "space" or replace "space" with "underscore").
    ```
    cat Mmusculus_NC_005089.fasta | sed 's/ /_/g' > Mmusculus_NC_005089.fasta 
    ```

Running Paired-End
==================

- Run MITGARD in paired-end mode:
  ```
  MITGARD.py -s Mmusculus_Muscle -1 SRR594408_pass_1.fastq.gz -2 SRR594408_pass_2.fastq.gz -R Mmusculus_NC_005089.fasta -c 40 -M 50G
  ```

- Check the mitogenome assembly in the file ```Mmusculus_Muscle/Mmusculus_Muscle_mitogenome.fa```.

Running Single-End
==================

- If the user wants to merge the paired-end reads and run MITGARD in single-end mode.

  - Merge the reads using [PEAR](https://cme.h-its.org/exelixis/web/software/pear/) to convert the paired-end reads into merged single-ends reads.
    ```
    pear -k -j 32 -f SRR594407_pass_1.fastq.gz -r SRR594407_pass_2.fastq.gz -o SRR594407_merged_Mmusculus_Muscle
    ```

- Run MITGARD in single-end mode:
  ```
  MITGARD.py -s Mmusculus_Muscle -S SRR594407_merged_Mmusculus_Muscle.assembled.fastq -R Mmusculus_NC_005089.fasta -c 40 -M 50G
  ```

- Check the mitogenome assembly in the file ```Mmusculus_Muscle/Mmusculus_Muscle_mitogenome.fa```.