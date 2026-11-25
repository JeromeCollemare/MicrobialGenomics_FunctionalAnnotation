---
title: "Functional annotation: Assignment 1 - annotation using similarity search"
teaching: 0
exercises: 120
questions:
- Functional annotation based on sequence similarity
objectives:
- Build a command line for BLAST
- Read the help from bioinformatic tools and understand how different arguments impact the output
- Interpret BLAST results
keypoints:
- Choose tools’ arguments according to the output you would like to obtain
- Not all results are biologically relevant and thresholds must be used to keep significant hits only
---

#  Background
In this exercise, you will determine **if the predicted proteins in *Z. tritici* share sequence similarity with other proteins from other organisms using BLAST**. Two major non-redundant (nr) databases are commonly used: the Swiss-Prot database (572,214 manually annotated and reviewed sequences (10/2024); http://www.uniprot.org) and the NCBI nr database (840,420,260 sequences (11/2024); all non-redundant GenBank CDS translations+PDB+SwissProt+PIR+PRF excluding environmental samples from WGS projects). Because of the size of the NCBI database, you will “blast” the *Z. tritici* proteins against the Swiss-Prot sequences only, which is available on the server (**~/data/fungalgenomics\_collemare/day1/uniprot\_sprot.fasta**).

Before performing a BLAST search locally, a BLAST database must be built. This step had already been done using the following command:
~~~
$ makeblastdb -dbtype prot -in /path/to/uniprot_sprot.fasta
~~~
You can find more information about makeblastdb on the online BLAST manual https://www.ncbi.nlm.nih.gov/books/NBK279690/
{: .bash}

#  BLAST
Using the BLAST manual and its Appendices (https://www.ncbi.nlm.nih.gov/books/NBK279690) or the BLAST help, prepare a suitable BLAST command to search the Swiss-Prot database with *Z. tritici* proteins as query. Follow the questions to guide you towards the needed command line.

> ## Question 1
> 
> Which BLAST tool do you need to compare protein queries to protein databases?
>
>> ## Solution
>> 
>> BlastP is dedicated to search similarities between protein sequences
>> 
> {: .solution}
{: .challenge}


> ## Question 2
> 
> Which arguments do you have to use in any basic BLAST command?
>
>> ## Solution
>> 
>> blastp -db database\_name -query input\_file\ these two arguments tell BLAST to compare sequences in the -query to sequences in the -db
>> 
> {: .solution}
{: .challenge}


We can now try to find out for how many different species there is at least one genome sequenced. We can do this using the command line. 

~~~
$ cat assembly_summary.txt | grep -v "^#" | cut -f8 | cut -f1,2 -d" " | sort -u | wc -l
~~~
{: .bash}

Execute a test `BLAST` search using the file on the server .**~/data/fungalgenomics\_collemare/day1/Mycgr3.100000.fasta**, which contains a single sequence, and save the results in a text file. Open the result file. List the different information that are reported.

> ## Question 3
> 
> Which information do you think is important to report for each gene when making a large tabular annotation file? Is the output format convenient to build an annotation file?
>
>> ## Solution
>> 
>> In the result file, you first find information about the database and your query sequence. Then, a list with the names of
sequences in the database (also called subject) that share similarities with your query sequence, including a score (bits) and an E-value which indicate how significant are the similarities. Below, for each reported subject sequence is shown the alignment with the query sequence, as well as more specific information about this alignment: length of the alignment, score, E-value, number of identical positions, number of similar positions, number of gaps. At the end of the file, are indicated the parameters used by BLAST to perform the search.
>> When assigning a function, it is important to only show reliable information. The name and description of the subject sequences, as well as the reliability of the result given by E-values or scores are important. The bitscore derives directly from the alignment, meaning that the more identical two sequences, the higher the score. However, it does not consider the size of the database which has a direct impact on the false discovery rate. E-values are scores normalized to the size of the database and thus provide an estimation of the chance to find similarity by random. Thus, the lower the E-value, the more significant the hit. Protein identity/similarity is also an important information to keep because it indicates how distantly related the query and subject proteins are. The length of the alignment can be useful because it will indicate the protein coverage of the alignment, meaning how much of the query and subject sequences are aligned. A significant alignment over a very short portion of the query sequence would not provide very strong information about the function of the protein.
>> The default output format is not convenient as alignments themselves are not necessary. A tabular format is more useful.
>> 
> {: .solution}
{: .challenge}

Let’s start with the output format.

> ## Question 4
> 
> Which argument can you use to output the results in a tabular format?
>
>> ## Solution
>> 
>> -outfmt 6
>> 
> {: .solution}
{: .challenge}

Test your command line with a single sequence first and save the results in a file. Open the result file.

> ## Question 5
> 
> which information is indicated in each column? Why does the result file contain more than one line? Are all results strongly supported?
>
>> ## Solution
>> 
>> name of query sequence | name of subject sequence | identity percentage | alignment length | number of identical positions | number of gaps | start position in query sequence | end position in query sequence | start position in subject sequence | end position in subject sequence | e-value | bit score
>> The file contains several lines because one query sequence can align to different subject sequences in the database.*
>> Apart from the first three subject sequences, all others exhibit a high e-value and low bit score, corresponding to short alignments with low similarity. These results are not reliable and should not be consider for predicting protein function.
>> 
> {: .solution}
{: .challenge}

A fungal genome typically contains 10-12k genes. It is therefore not very handy to report several subject sequences for each protein and it is usually preferred to report the best hit only.


> ## Question 6
> 
> Which argument can you use in the command line to report only the best hit per query sequence?
>
>> ## Solution
>> 
>> -max_target_seqs 1
>> 
> {: .solution}
{: .challenge}


Test your command line with a single sequence first and save the results in a file. Open the file and check if the result corresponds to your expectation.

Now that you are familiar with blast command lines, run a blast search for the first 30 annotated proteins of *Z. tritici* using the file on the server .**~/data/fungalgenomics\_collemare/day1/Mycgr3.30proteins.fasta** (the run should last around 5 minutes). Open the result file.


> ## Question 7
> 
> Do you have only one result per query sequence as expected? How many query sequences have more than one hit (you can use a simple bash command using cut, uniq and sort to calculate the numbers)?
>
>> ## Solution
>> 
>> ~~~
>> $ cut -f1 blast\_result4.txt | uniq -c | sort -r
>> ~~~
>> {: .bash}
>> For a given gene, they correspond to high-scoring segment pair (HSPs), meaning that different regions of the subject protein align with the query protein with a gap in between. It is commonly found when either the query or subject sequence contains a deletion.
>> 
> {: .solution}
{: .challenge}

The different reported regions for a single query are high-scoring pairs (HSPs). Quite often, one HSP is more significant than others. For the annotation file, it is thus sensible to report only one HSP.

> ## Question 8
> 
> Which argument can you use to limit the number of reported HSPs?
>
>> ## Solution
>> 
>> -max_hsps 1
>> 
> {: .solution}
{: .challenge}

Test your command line and save the results in a file. Check if the results meet your expectations.


> ## Question 9
> 
> Do all proteins share similarity with proteins from the Swiss-Prot database? If not, is there another argument that you could use to keep only the most relevant hits (choosing an arbitrary threshold)?
>
>> ## Solution
>> 
>> 5 proteins (Mycgr3|100080, Mycgr3|100157, Mycgr3|100264, Mycgr3|100300, Mycgr3|100533) show results with clear high e-value, meaning that these results are not reliable. It can be concluded that these five proteins are not sharing any sequence similarity with any known sequence and are specific to *Z. tritici*.
>> An argument that could be used to report only significant hits is -evalue: the significant threshold is an arbitrary choice. You can use more stringent e-values if you want to increase the accuracy of the results.
>> 
> {: .solution}
{: .challenge}


> ## Question 10
> 
> What is the biological meaning when a protein does not share sequence similarity with any other sequence?
>
>> ## Solution
>> 
>> Proteins specific to *Z. tritici* may indicate a new biological function reflecting adaptation of *Z. tritici* to its specific environment.
>> 
> {: .solution}
{: .challenge}


{% include links.md %}
