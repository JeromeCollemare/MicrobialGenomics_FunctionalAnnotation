---
title: "Functional annotation: Assignment 4 - compare the gene content between species"
teaching: 0
exercises: 90
questions:
- Do all fungi carry the same number of proteins?
- How can we interpret shared and specific proteins? 
- Can we easily infer traits from proteome size?
objectives:
- Compare the proteome size of different related species
- Use different comparisons to identify proteins that could be involved in the pathogenicity process
- Compare the gene content between species
keypoints:
- Fungal genomes encode many specific proteins
- Specific proteins tend to not have any functional prediction
- The gene content is similar between species with different lifestyles
- A spreadsheet can be useful to sort data and compare functional annotations
---

#  Orthologous protein families

Different fungi exhibit different host specificity or growth, which can be related to their
gene complement. **Genes that are shared across all fungi are expected
to be involved in common biological processes, while genes specific to
certain fungi are more likely to explain their specificity.** Tools exist
to compare gene sequences between species and classify them according to
their similarity to form protein families. One such tool is OrthoFinder
(https://github.com/davidemms/OrthoFinder/releases),
for which you can find the results on the server
(**~/data/fungalgenomics\_collemare/day2/Runld\_31\_counts.txt**).
Open the file in Excel (or similar spreadsheet software) or using the Terminal. OrthoFinder groups proteins in clusters. A cluster shared in different species contain orthologous proteins. Within a given species, a cluster can comprise several proteins, which are then called paralogues (and belong to a
so-called protein family).

> ## Question 1
> 
> What does the total number of genes/proteins represent for the *Mycosphaerellaceae*?
>
>> ## Solution
>> 
>> the pan genome/proteome.
>>
> {: .solution}
{: .challenge}

Determine the proteome size of each species.

> ## Question 2
> 
> How do you explain the differences?
>
>> ## Solution
>> 
>> The size varies from 9,739 to 14,127 predicted proteins. Several reasons can explain the observed differences:
>> 1) quality of the sequencing and assemblies, which may result in certain genomes to be less complete than others;
>> 2) quality of the gene prediction, depending on which predictor used and parameters, the number of genes can vary;
>> 3) evolution, certain genes are present in only certain species. See results_exercise1.xlsx bottom of the first tab
>>
> {: .solution}
{: .challenge}

To facilitate further analyses when using a spreadsheet software, create a new column which contains for each cluster (row) the number of species which have proteins belonging to this cluster. When using the Terminal, a command line with awk for example allows filtering the data without creating such a new column.

> ## Question 3
> 
> What is the **core** proteome shared by all these fungi? In which biological processes could it be involved?
>
>> ## Solution
>> 
>> The core proteome corresponds to proteins that are conserved in all species and are likely involved in basic biological processes. It means that it corresponds to clusters with at least 1 protein in each species.
>>
> {: .solution}
{: .challenge}

Determine the size of the core proteome \(number of clusters\).

For each row, you can summarize the number of species having a cluster using the formula: =SUM(IF(B2>0; 1; 0); IF(C2>0; 1; 0); IF(D2>0; 1; 0); IF(E2>0; 1; 0); IF(F2>0; 1; 0); IF(G>0; 1; 0))

In the Terminal, counting can be done using this command line:
~~~
$ awk 'BEGIN{FS="\t"};{if($2>0 && $3>0 && $4>0 && $5>0 && $6>0 && $7>0) print $1}' RunId_31_counts.txt | wc -l
~~~
{: .bash}

> ## Question 4
> 
> Does the core proteome have a similar size in each species (number of proteins)? Which fraction of the total proteome of each species does the core proteome represent?
>
>> ## Solution
>> 
>> The core proteome is made of 5,768 clusters. It represents between 6,379 to 7,568 proteins, and 53 to 65% of the overall proteome. See results_exercise1.xlsx bottom of the first tab.
>>
> {: .solution}
{: .challenge}


Now, compare the size of the **specific** proteomes.

> ## Question 5
> 
> Does it have a similar size in each species? Which fraction of the total proteome of each species does it represent? How do you explain these differences?
>
>> ## Solution
>> 
>> Specific proteomes correspond to proteins that are found in only one species and are expected to explain specific phenotypes of the organism. They correspond to clusters that are present in only one of the species. The number of specific proteins vary between 711 and 3,135 proteins, representing 7 to 24% of the total proteome. See results_exercise1.xlsx bottom of the first tab.
>> The difference can be explained by different factors:
>> 1) like for the total number of proteins, assemblies and predictions could affect the numbers;
>> 2) real evolution, with some species experiencing gene family extension or gene losses for example;
>> 3) technical: we are comparing species that diverged from each other at different times. Sepmu and Seppo have fewer specific proteins because the two species are very closely related and did not diverge long ago. A 2 by 2 comparison actually gives you a hint about the evolutionary distance between the species: the more they have in common, likely the closer they are. Based on the number of specific proteins, you can see that Sepmu/Seppo and Clafu/Dotse are pairs of close relatives. See results_exercise1.xlsx bottom of the first tab.
>>
> {: .solution}
{: .challenge}


As we are interested in protein functions, we can now compare which functions are found in the core and specific proteomes. For each cluster, the predicted PFAM conserved domains are indicated. For the following questions, using the Terminal and awk is easier, though it is also possible to do it in the spreadsheet.


> ## Question 6
> 
> How many proteins are not assigned any function (*i.e.* no PFAM) in the core and specific proteomes? How many proteins are assigned a function (*i.e.* with a PFAM domain)? What is your conclusion?
>
>> ## Solution
>> 
>> proteins without function do not have any conserved domain and the cluster is annotated with "NO PFAM (100%)". A possible command line to count the number of PFAM domains is:
>> ~~~
>> $ awk 'BEGIN{FS="\t"};{if($2>0 && $3>0 && $4>0 && $5>0 && $6>0 && $7>0) print $8}' RunId_31_counts.txt | sort | uniq -c | sort -r | head -n 10
>> ~~~
>> {: .bash}
>> Example of command line for specific proteome focusing on Clafu:
>> ~~~
>> $ awk 'BEGIN{FS="\t"};{if($2>0 && $3==0 && $4==0 && $5==0 && $6==0 && $7==0) print $8}' RunId_31_counts.txt | sort | uniq -c | sort -r | head -n 10
>> ~~~
>> {: .bash}
>> Out of the 5,768 clusters in the core proteome, 774 are not assigned any function (13% of the core proteome) and thus 4,994 proteins are assigned a function. In the specific proteomes, the numbers of proteins without function vary from 606 in Seppo to 2914 in Mycfi, but they represent between 85 and 95% of the specific proteomes.
>> A major difference between the core and specific proteomes is that specific proteins tend to have no functional annotation, while they are expected to contribute to the specificities of each pathogen. See results_exercise1.xlsx bottom of the second tab.
>>
> {: .solution}
{: .challenge}


> ## Question 7
> 
> What are the most abundantly represented functions in the core and specific proteomes (focus on the 10 most abundant)?
>
>> ## Solution
>> 
>> The most abundant functions in the core proteome are related to signalling (kinase), protein interactions (WD40), energy (mitochondrial carrier protein), transcription (transcription factors and LSM), MFS transporters, RNA-binding proteins, cytochrome P450s, and protein degradation (ubiquitin-conjugating enzymes). The functions in the different specific proteomes are quite different and very diverse. However, it is difficult to determine which function could be important for the pathogenicity on each host/organ. See results_exercise1.xlsx bottom of the second tab.
>>
> {: .solution}
{: .challenge}



#  Compare the gene content between different species

In this exercise, you will use a **targeted approach because certain
classes of proteins are known to play crucial roles in the adaptation of
fungi to different ecological niches: secreted-small proteins, secreted
proteases, CAZymes, secondary metabolites**. This targeted approach is
commonly used as a basis for the analysis of any fungal genome. It is
usually good to include contrasting reference genomes as controls; thus,
functional annotations are provided for the baker’s yeast *Saccharomyces
cerevisiae* (Sacce1) and the saprobe *Aspergillus nidulans* (Aspnid1).

Using the annotation files, compare the predicted proteome size of each
plant pathogens to the one of *A. nidulans* and *S. cerevisiae*. Use the
different annotation files to extract the following categories and
sub-categories, and follow the questions to guide your analysis:

Secretome: *Secreted-Small Proteins (SSPs) | Secreted CAZymes | Secreted peptidases/proteases*

Secondary metabolite genes: *Polyketide Synthases (PKSs) | Non-Ribosomal Peptide Synthetases (NRPSs)*

Start with general statistics. Write a bash command that makes use of **wc** to count the total number of proteins in each annotation file. Save the results in a table.
~~~
$ wc -l funannot.*.txt
~~~
{: .bash}

Then, write a bash command that makes use of **awk** and **wc** to count the number of proteins with transmembrane domains and the number of proteins that are predicted to be secreted. Save the results in the table.
~~~
$ awk '{if($3>=1 && $4=="N") print $0}' funannot.*.txt | wc -l
~~~
{: .bash}
~~~
$ awk '{if($4=="Y" && $3==0) print $0}' funannot.*.txt | wc -l
~~~
{: .bash}

Adapt the bash command with **awk** and **wc** to automatically count the number of SSPs. Save the results in the table.
~~~
$ awk '{if($4=="Y" && $3==0 && $2<=300) print $0}' funannot.*.txt | wc -l
~~~
{: .bash}

For counting secreted peptidases/proteases, adapt the bash command using **grep** to retrieve the genes with the terms “peptidase” or “protease” description in the functional annotation. Save the results in the table.
~~~
$ awk '{if($4=="Y" && $3==0) print $0}' funannot.*.txt | grep -c "peptidase\|protease"
~~~
{: .bash}

Adapt the previous bash command to retrieve secreted CAZymes (use the terms “Glyco\_hydro”, “Glyco\_transf” and “CBM” pfam domains in the functional annotation). Save the results in the table.
~~~
$ awk '{if($4=="Y" && $3==0) print $0}' funannot.*.txt | grep -c " Glyco_hydro\|Glyco_transf\|CBM"
~~~
{: .bash}

Adapt the bash command to retrieve PKS and NRPS proteins, which are very large proteins, using the signature conserved domains PF02801 (PKS) and PF00668 (NRPS). Save the results in the table.
~~~
$ grep "PF02801\|PF00668" funannot.*.txt | awk '{if($2>1000) print $0}' | wc -l
~~~
{: .bash}


> ## Question 8
> 
> Compare the numbers between all different species. Do you find significant differences between certain species? Does it correlate with their lifestyle? Can you suggest complementary approaches that would allow making more precise assumptions? 
>
>> ## Solution
>> 
>> The variation in the total number of genes had been discussed previously. Aspnid have a similar number of genes, but Sacce has significantly less genes. This fungus experienced massive gene losses which explains this low number.
>> The number of transmembrane proteins is very similar in all species. The number of secreted proteins is also very similar although Sacce has fewer and Clafu has more, which for the latter is either explained by a different gene prediction or specific function for the lifestyle of this fungus. The same discussion can be done about SSPs. For secreted proteases and CAZymes, all numbers are very similar, except again for Sacce that contains much fewer genes. In regard to secondary metabolite enzymes, Aspnid has more genes than the other fungi and Sacce contains many less. Thus, although SSPs, peptidases, CAZymes, secondary metabolites are known virulence factors, the comparative genomics approach indicates that these categories of proteins likely play other functions in non-pathogenic fungi like Aspnid. They likely function in colonizing particular ecological niches, including plants.
>> These analyses provide a starting point for further investigations because certain genes could specifically play important functions in particular processes like pathogenicity. However, it is important to keep in mind that additional information is needed to go further, such as transcriptomics, proteomics, metabolomics or population genetics data.
>>
> {: .solution}
{: .challenge}


{% include links.md %}
