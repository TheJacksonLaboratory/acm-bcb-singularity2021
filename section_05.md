# Using Containers

Building on our previous sections, in this unit, we’re going to build a container for BLAST and show how to use that container to construct a BLAST database and search sequences against that database.

## Containerizing BLAST 

BLAST stands for Basic Local Alignment Search Tool, and is a sophisticated software package for rapid searching of protein and nucleotide databases.  BLAST was developed by Steven Altschul in 1989, and has continually been refined, updated, and modified throughout the years to meet the increasing needs of the scientific community.

To cite BLAST, please refer to the following:
Altschul SF, Gish W, Miller W, Myers EW, Lipman DJ.  Basic local alignment search tool.  Journal of Molecular Biology.  Volume 215(3), pages 403-410. 1990.
PMID: 2231712  DOI: 10.1016/S0022-2836(05)80360-2.

To start, let’s create an empty file to use as our recipe file. 

```
$ touch Singularity.BLAST
```

The touch command allows modification of file timestamps, or in the case of this usage, where the file does not already exist, creates an empty file.

Now we’ll use nano to build out our recipe file.

```
$ nano Singularity.BLAST
```

This should open the basic nano text editor for you to access through your terminal.

Let’s type the following into our Singularity.BLAST file:

```
Bootstrap:docker
From:ubuntu

%labels
        MAINTAINER Shane Sanders

%post
        apt update && apt upgrade -y
        apt install -y wget gzip zip unzip ncbi-blast+ locales
        LANG=C perl -e exit
        locale-gen en_US.UTF-8

%runscript
        echo "Hello from BLAST!"
```

Let’s hit CTRL-O to save our modifications and then CTRL-X to exit the nano editor.

Ok, now to build the container:
```
$ sudo singularity build BLAST.sif Singularity.BLAST
```

This should take ~3 minutes to build the container and install BLAST.  If you’d like to time it on your system, you can prefix the command with the time command, as follows:

```
$ time sudo singularity build BLAST.sif Singularity.BLAST
```

And if you time this, at the end of the process, you should see output like:

```
real    2m58.694s
user    1m12.748s
sys    0m7.547s
```

Ok, let’s test that our container is built properly.  

```
$ ./BLAST.sif
Hello from BLAST!
```

Ok, now we can go into the container’s environment to verify things using the singularity shell command.

```
$ singularity shell BLAST.sif
Singularity> blastp -h
USAGE
  blastp [-h] [-help] [-import_search_strategy filename]
    [-export_search_strategy filename] [-task task_name] [-db database_name]
    [-dbsize num_letters] [-gilist filename] [-seqidlist filename]
    [-negative_gilist filename] [-negative_seqidlist filename]
    [-taxids taxids] [-negative_taxids taxids] [-taxidlist filename]
    [-negative_taxidlist filename] [-ipglist filename]
    [-negative_ipglist filename] [-entrez_query entrez_query]
    [-db_soft_mask filtering_algorithm] [-db_hard_mask filtering_algorithm]
    [-subject subject_input_file] [-subject_loc range] [-query input_file]
    [-out output_file] [-evalue evalue] [-word_size int_value]
    [-gapopen open_penalty] [-gapextend extend_penalty]
    [-qcov_hsp_perc float_value] [-max_hsps int_value]
    [-xdrop_ungap float_value] [-xdrop_gap float_value]
    [-xdrop_gap_final float_value] [-searchsp int_value] [-seg SEG_options]
    [-soft_masking soft_masking] [-matrix matrix_name]
    [-threshold float_value] [-culling_limit int_value]
    [-best_hit_overhang float_value] [-best_hit_score_edge float_value]
    [-subject_besthit] [-window_size int_value] [-lcase_masking]
    [-query_loc range] [-parse_deflines] [-outfmt format] [-show_gis]
    [-num_descriptions int_value] [-num_alignments int_value]
    [-line_length line_length] [-html] [-sorthits sort_hits]
    [-sorthsps sort_hsps] [-max_target_seqs num_sequences]
    [-num_threads int_value] [-ungapped] [-remote] [-comp_based_stats compo]
    [-use_sw_tback] [-version]

DESCRIPTION
   Protein-Protein BLAST 2.9.0+

Use '-help' to print detailed descriptions of command line arguments
```

The blastp command is for Protein BLASTs, where a protein sequence is searched against a protein database.

Let’s exit the container environment.

```
Singularity> exit
```

Now let’s try the containerized command from the server’s environment.

```
$ singularity exec BLAST.sif blastp -h
USAGE
  blastp [-h] [-help] [-import_search_strategy filename]
    [-export_search_strategy filename] [-task task_name] [-db database_name]
    [-dbsize num_letters] [-gilist filename] [-seqidlist filename]
    [-negative_gilist filename] [-negative_seqidlist filename]
    [-taxids taxids] [-negative_taxids taxids] [-taxidlist filename]
    [-negative_taxidlist filename] [-ipglist filename]
    [-negative_ipglist filename] [-entrez_query entrez_query]
    [-db_soft_mask filtering_algorithm] [-db_hard_mask filtering_algorithm]
    [-subject subject_input_file] [-subject_loc range] [-query input_file]
    [-out output_file] [-evalue evalue] [-word_size int_value]
    [-gapopen open_penalty] [-gapextend extend_penalty]
    [-qcov_hsp_perc float_value] [-max_hsps int_value]
    [-xdrop_ungap float_value] [-xdrop_gap float_value]
    [-xdrop_gap_final float_value] [-searchsp int_value] [-seg SEG_options]
    [-soft_masking soft_masking] [-matrix matrix_name]
    [-threshold float_value] [-culling_limit int_value]
    [-best_hit_overhang float_value] [-best_hit_score_edge float_value]
    [-subject_besthit] [-window_size int_value] [-lcase_masking]
    [-query_loc range] [-parse_deflines] [-outfmt format] [-show_gis]
    [-num_descriptions int_value] [-num_alignments int_value]
    [-line_length line_length] [-html] [-sorthits sort_hits]
    [-sorthsps sort_hsps] [-max_target_seqs num_sequences]
    [-num_threads int_value] [-ungapped] [-remote] [-comp_based_stats compo]
    [-use_sw_tback] [-version]

DESCRIPTION
   Protein-Protein BLAST 2.9.0+

Use '-help' to print detailed descriptions of command line arguments
```

Same output, so now let's put our new BLAST container to use!

## Acquiring Protein Data

To start, we are going to need some data to serve as our database to search against.  For this exercise, we will use NCBI's daily RefSeq build of proteins.  Let's download and uncompress this file.

```
$ wget ftp://ftp.ncbi.nih.gov/refseq/daily/rsnc.0728.2021.faa.gz
```

This shouldn't take long, and produces the following output:
```
--2021-07-29 20:05:24--  ftp://ftp.ncbi.nih.gov/refseq/daily/rsnc.0728.2021.faa.gz
           => ‘rsnc.0728.2021.faa.gz’
Resolving ftp.ncbi.nih.gov (ftp.ncbi.nih.gov)... 165.112.9.229, 130.14.250.13, 2607:f220:41e:250::13, ...
Connecting to ftp.ncbi.nih.gov (ftp.ncbi.nih.gov)|165.112.9.229|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /refseq/daily ... done.
==> SIZE rsnc.0728.2021.faa.gz ... 683095678
==> PASV ... done.    ==> RETR rsnc.0728.2021.faa.gz ... done.
Length: 683095678 (651M) (unauthoritative)

rsnc.0728.2021.faa.gz       100%[========================================>] 651.45M  85.6MB/s    in 7.2s

2021-07-29 20:05:31 (90.2 MB/s) - ‘rsnc.0728.2021.faa.gz’ saved [683095678]
```

Now we will unzip the data file.

```
$ gunzip rsnc.0728.2021.faa.gz
```

Now we will convert the protein FASTA file we downloaded into a BLAST database to search against.

```
$ time singularity exec BLAST.sif makeblastdb -in rsnc.0728.2021.faa -dbtype prot -out RefSeqExample
```

This gives us the following output:
```
Building a new DB, current time: 07/29/2021 18:02:04
New DB name:   /home/labuser/RefSeqExample
New DB title:  rsnc.0728.2021.faa
Sequence type: Protein
Keep MBits: T
Maximum file size: 1000000000B
Adding sequences from FASTA; added 3094926 sequences in 228.992 seconds.

real    3m50.119s
user    1m49.394s
sys     0m4.019s
```

Now we need some sequences of interest to search against the RefSeq database we just constructed.  Let's download all the RefSeq proteins for Human Chromosome 1:

```
$ wget ftp://ftp.ncbi.nih.gov/refseq/H_sapiens/mRNA_Prot/human.1.protein.faa.gz
$ gunzip human.1.protein.faa.gz
```

We've downloaded a multi-line FASTA file, but for ease of use, we will now convert this into a single-fasta.

```
# Convert multi-line FASTA to single-line FASTA
$ sed -e 's/\(^>.*$\)/#\1#/' human.1.protein.faa | tr -d "\r" | tr -d "\n" | sed -e 's/$/#/' | tr "#" "\n" | sed -e '/^$/d' > human.1.protein.faa.cleaned.fasta
```

Now, we'll BLAST the proteins of Human chromosome 1 against the RefSeq database.  Normally, you would want to run your entire query sequence against the database and interpret results, but because these cloud systems we are connected to are small, in the interest of time we are going to reduce our number of query sequences.

```
head -n 20 human.1.protein.faa.cleaned.fasta > search_query.fasta
```

This pulls the first 10 protein sequences listed in the human chromosome 1 file into a new file, search_query.fasta.  So instead of searching all 8,067 sequences, we will only search 10 (0.1%) against the RefSeq database we've constructed.

```
$ time singularity exec BLAST.sif blastp -num_threads 8 -db RefSeqExample -query search_query.fasta -outfmt 6 -out BLASTP_Results.txt -max_target_seqs 1
```

This gives us the following output:
```
Warning: [blastp] Examining 5 or more matches is recommended

real    2m26.278s
user    7m40.642s
sys     0m1.062s
```
Checking the output file:
```
$ head BLASTP_Results.txt
```

Gives the following:
```
NP_001355814.1  XP_042624488.1  47.067  716     228     15      44      651     34      706     1.50e-111   356
NP_001355815.1  XP_042584322.1  46.857  525     174     12      29      477     181     676     2.60e-73    249
NP_001355815.1  XP_042584322.1  33.428  353     114     11      205     457     145     476     1.04e-11    73.2
NP_001355815.1  XP_042584322.1  36.087  230     70      6       1       230     499     651     7.15e-05    51.6
NP_001355815.1  XP_042584322.1  34.899  298     107     10      232     451     91      379     1.9     37.4
NP_001361386.1  XP_042585651.1  85.403  1651    186     11      1       1614    1       1633    0.0     2513
NP_001347100.1  XP_042582653.1  25.943  212     120     7       26      224     7       194     1.67e-04    48.9
NP_001347100.1  XP_042582653.1  23.445  209     115     9       28      222     105     282     1.7     36.6
NP_001355178.1  XP_012696246.2  68.670  549     171     1       52      600     76      623     0.0     821
NP_001355183.1  XP_042566333.1  25.424  295     191     13      31      322     21      289     1.17e-07    59.7
```
