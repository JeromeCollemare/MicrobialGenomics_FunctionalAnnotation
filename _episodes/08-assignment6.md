---
title: "Functional annotation: Assignment 6 - case study for genome mining"
teaching: 0
exercises: 45
questions:
- How can we identify the biosynthetic genes involved in the production of a metabolite of interest? 
objectives:
- Predict biosynthetic gene clusters encoded in a fungal genome
- Identify similar biosynthetic genes
- Explore biosynthetic genes encoded at the locus
keypoints:
- Biological knowledge is needed to elaborate hypotheses for the mining
- Genome mining is a step-by-step approach that requires different bioinformatics tools
---

#  Background

In this exercise, we want to identify a candidate biosynthetic gene cluster (BGC) for the production of [taladerxine C](files/talaroderxineC.pdf) by the fungus *Periconia macrospinosa*.

This molecule is an aromatic polyketide with three benzene rings, of which one is reduced, it is a dimer and it is structurally related to the [viriditoxin](files/viriditoxin.pdf) compound for which the BGC is characterized in *Paecilomyces variotii*

Based on these observations, we can elaborate several hypotheses that will guide the genome mining:
1) The biosynthesis involves a non-reducing polyketide synthase (nrPKS)
2) A laccase is involved in making the dimer
3) A short chain dehydrogenase/reductase is involved in reducing the ring
4) The three genes in *P. macrospinosa* are homologs of the viriditoxin BGC genes

#  Genome mining

To start the genome mining, we first need to retrieve the assembly and gene prediction. On the NCBI genome repository, download the assembly file (.fasta) and gene prediction (annotation features) file (.gff) for *Periconia macrospinosa*

The second step is to use fungiSMASH (https://fungismash.secondarymetabolites.org/#!/start) to identify genomic regions that encode putative BGCs. Upload the assembly and gff files. Select extra features: “MIBiG cluster comparison”, “Cluster Pfam analysis”, “Pfam-based GO term annotation”. It will take about 30 minutes

While fungiSMASH is running, go back to the NCBI homepage and search for the vdtA
(viriditoxin nrPKS) and vdtB (viriditoxin laccase) proteins in the NCBI Protein repository. Copy the protein sequences in fasta format in a text file.

Run BlastP on NCBI (https://blast.ncbi.nlm.nih.gov/Blast.cgi) using the vdtA and vdtB sequences as query, and choose as Organism (in Choose search set box) Periconia macrospinosa (taxid:97972).

Click on the accession number of the best hit for each search to access information about the *P. macrospinosa* protein and retrieve information about the CDS (“locus tag” and “coded by” which corresponds to gene IDs and scaffold number and genome coordinates).

> ## Question 1
> 
> Are the best hit for each protein encoded at the same locus in *P. macrospinosa*?
>
>> ## Solution
>> 
>> VdtA homolog is located and VdtB homolog is located. THey are on the same scaffold and separated by xxx kb. We can conclude that they are located at the same locus in the genome. 
>>
> {: .solution}
{: .challenge}

Go to the fungiSMASH results.

> ## Question 2
> 
> How many regions encoding PKSs are present in the genome (some regions encode different types and are indicated as hybrids)?
>
>> ## Solution
>> 
>> xxx
>>
> {: .solution}
{: .challenge}

nrPKSs are characterized by the following conserved domains: SAT-KS-AT-PT-TE, while
reducing PKSs start immediately with the KS domain. Such organization is found in the
NRPS/PKS domains tab.

> ## Question 3
> 
> How many nrPKSs are encoded in the regions identified by
fungiSMASH?
>
>> ## Solution
>> 
>> xxx
>>
> {: .solution}
{: .challenge}

Use the information from the Blast analysis to identify the corresponding fungiSMASH result
and look at the locus. An overview of the predicted conserved domains are found in the Pfam
domains tab.

> ## Question 4
> 
> Which other enzymatic functions are encoded at the locus? Can you find a gene that encodes a short chain dehydrogenase/reductase?
>
>> ## Solution
>> 
>> xxx
>>
> {: .solution}
{: .challenge}

> ## Question 5
> 
> What is the next step to link this BGC to the production of Talaroderxine C?
>
>> ## Solution
>> 
>> The genome mining identified a candidate BGC only. A possible next step is to look at gene expression during the production of Talaroderxine C, and if possible, compare expression levels to a condition where the compounds is not produced. Other approaches like targeted gene deletion or heterologous expression could also be used, but these experiments are usually performed once the candidate BGC is supported with expression data.
>>
> {: .solution}
{: .challenge}


{% include links.md %}
