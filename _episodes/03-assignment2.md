---
title: "Functional annotation: Assignment 2 - search for conserved domains"
teaching: 0
exercises: 60
questions:
- Functional annotation based on conserved domains
objectives:
- Build a command line for HMMER
- Read the help from bioinformatic tools and understand how different arguments impact the output.
- Interpret HMMER results.
keypoints:
- Choose tools’ arguments according to the output you would like to obtain
- Not all results are biologically relevant and thresholds must be used to keep significant hits only
- Proteins may contain more than one conserved domain
---

#  HMMER
In this exercise, you will determine **if proteins from *Z. tritici* contain conserved domains that might be involved in specific functions**. There are many databases and tools to search for conserved domains. One of the most used databases is PFAM (fully integrated in InterPro since January 2023) (https://pfam-legacy.xfam.org). It contains families that are represented by multiple sequence alignments and derived Hidden Markov Models (HMMs). To search this database, you need to use hmmscan (search an hmm database with sequence query) from the HMMER tool suite.

The PFAM database consists of HMMs and was prepared using the command below:
~~~
$ hmmpress /path/to/Pfam-A.hmm
~~~
{: .bash}

It is available on the server (**~/data/fungalgenomics\_collemare/day1/Pfam-A.hmm**).

Using the HMMER3 manual (http://eddylab.org/software/hmmer3/3.1b2/Userguide.pdf) or the hmmscan help, prepare a suitable hmmscan command line to search the PFAM database with *Z. tritici* proteins as query. Follow the questions to guide you towards the needed command line.

Execute a test HMMSCAN search with the minimum number of arguments and using the file **~/data/fungalgenomics\_collemare/day1/Mycgr3.100proteins.fasta** and PFAM database. Save the results in a text file (the run should last about one minute). Open the result file. List the information you find.

> ## Question 1
> 
> Given what you learnt with BLAST, which features do you think are important to consider when searching for similarities with conserved domains? 
>
>> ## Solution
>> 
>> For each query sequence, a table present the conserved domains that present domain hits, with a summary of significance values (E-value, score, and bias applied to the score to consider sequence composition) and domain description. Then, for each conserved domain is shown the alignment between the domain and query sequence. Below are presented statistics summary of the analysis.
>> Description of the conserved domains, reliability of the results given by e-values and scores; suitable format of the output: one line per protein.
>> 
> {: .solution}
{: .challenge}


> ## Question 2
> 
> Should you output only the best hit as you did in the BLAST analysis?
>
>> ## Solution
>> 
>> In the BLAST search, we limited the results to the best hit because we were looking at homologous proteins. A protein can consist in several conserved domains and thus it is important to keep all significant hits because they might provide different useful information about the putative biological activity.
>> 
> {: .solution}
{: .challenge}


> ## Question 3
> 
> Which argument can you use to output the results in a tabular format?
>
>> ## Solution
>> 
>> --tblout: note that the --tblout argument prints the tabular results in a file. But the main standard output can still be saved in another file using the -o argument.
>> 
> {: .solution}
{: .challenge}

Test your command line and save the results in a file.

> ## Question 4
> 
> Which argument can you use to not include hits with low support?
>
>> ## Solution
>> 
>> Several arguments can be used to keep very significant results only (reporting thresholds, inclusion thresholds, model-specific thresholds). The model-specific thresholds are certainly good to use when using curated databases like PFAM (--cut\_ga argument), but the default reporting threshold (-E argument) could be used with different databases in order to use the same threshold for all.
>> Here, we choose the -E argument for a 0.0001 threshold, which was chosen for stringent results
>> 
> {: .solution}
{: .challenge}


dbCAN (<http://bcb.unl.edu/dbCAN2/>) is a more specific database dedicated to carbohydrate-active enzymes. This is an important family of proteins that play crucial roles in the adaptation of fungi to grow on certain carbon sources. Similar to PFAM, dbCAN contains HMMs that can be used to annotate proteins. You will use this second database to compare results with the PFAM database.
 
The dbCAN database was prepared using the same command as for PFAM and is available on the server (**~/data/fungalgenomics\_collemare/day1/dbCAN-fam.hmm**).

Execute a HMMSCAN search using the file **~/data/fungalgenomics\_collemare/day1/Mycgr3.100proteins.fasta** and the dbCAN database, and save the results in a text file.

Compare the results you obtained with the PFAM and dbCAN databases.
In order to analyse the results more easily, it is possible to modify the HMMSCAN output to see all domains for a given gene on the same line. For this, execute the following command on the PFAM and dbCAN output:
~~~
$ grep -v '^#' output.file | awk '{y=$3;$3="";a[y]=a[y]"\t"$1}END{for(y in a)print y,a[y]}' | sort -k 1 > day1_hmm_oneline.txt
~~~
{: .bash}

> ## Question 5
> 
> Are the predictions identical? How do you explain differences?
>
>> ## Solution
>> 
>> The search in the dbCAN database gives much fewer results, which is logical given that only proteins that are related to carbohydrate-active enzymes are reported (8 in the case of this dataset). While for most proteins, predictions are consistent, for two, the predictions do not seem to agree (Mycgr3|100000, Mycgr3|100584). In the case of Mycgr3|100000, it could be that the prediction with dbCAN is not fully robust because the e-value is much lower than with PFAM. In the case of Mycgr3|100584, it could be similar, but the dbCAN search could also provide more precise information compared to the results with PFAM. It shows that it is good to perform the same analysis using different databases.
>> 
> {: .solution}
{: .challenge}


{% include links.md %}
