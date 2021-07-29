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

```
wget ftp://ftp.ncbi.nih.gov/refseq/daily/rsnc.0728.2021.faa.gz
```

```
gunzip rsnc.0728.2021.faa.gz
```

Now we will convert the protein FASTA file we downloaded into a BLAST database to search against.

```
$ time singularity exec BLAST.sif makeblastdb -in rsnc.0728.2021.faa -dbtype prot -out RefSeqExample
```

This gives us the following:
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

```
wget ftp://ftp.ncbi.nih.gov/refseq/H_sapiens/mRNA_Prot/human.1.protein.faa.gz
gunzip human.1.protein.faa.gz
```
