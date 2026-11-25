---
title: "Functional annotation: Assignment 5 - case study with expression data"
teaching: 0
start: true
exercises: 60
questions:
- How can we use expression data to prioritize candidate genes to further study?
objectives:
- Find genes that are specifically up-regulated in a specific condition.
- Find genes that are the most significantly differentially expressed. 
keypoints:
- Transcriptomics is a strong approach to identify genes of interest.
- Gene expression is a dynamic process.
- A spreadsheet is convenient to explore transcriptomics data
---

#  Background

While comparative functional genomics is useful to identify candidate genes, it is usually not sufficient to make robust hypotheses and restrict the selection to a number of genes that is manageable for experimental validation. Additional analyses are therefore needed, which can be of very diverse kind. In this exercise, you will see how gene expression and functional annotation can lead to the identification of genes and functions that could be involved in plant pathogenicity.

**Genes involved in virulence are expected to be specifically expressed during the infection process**, and not *in vitro*. Here, you will identify categories of genes in *Z. tritici* that are expressed during infection of wheat as they may be involved in pathogenicity. The RNAseq data you previously used is available on the server (**~/data/fungalgenomics\_collemare/day2/Expression.txt**).

In Excel (or similar spreadsheet software) or using the Terminal, combine the file that contain FPKM values from *in vitro* and *in planta* conditions (**~/data/fungalgenomics\_collemare/day2/Expression.txt**) with the functional annotation file of *Z. tritici* (**~/data/fungalgenomics\_collemare/day2/_funannot.Mycgr3.txt**).

NB: pay attention to the order of the genes in each file. delete lines that do not start with a gene id.

> ## Question 1
> 
> What is the total number of genes predicted in the genome?
>
>> ## Solution
>> 
>> *Z. tritici* genome contains 10,933 predicted genes.
>>
> {: .solution}
{: .challenge}

Sort the genes to identify those that are significantly differentially regulated *in planta* and compare the gene content in each category (significantly up- or down-regulated, non-significantly differentially expressed). Follow the questions for your analysis.

NB: to facilitate analyses, it is recommended to create new tabs with selected sets of genes.

> ## Question 2
> 
> Which column should you use to sort the data and differentiate between significantly and non-significantly regulated genes? How many genes are significantly up-regulated *in planta*? How many genes are significantly down-regulated *in planta*?
>
>> ## Solution
>> 
>> Sort the data according to the significance of the q value. Out of 10,933 predicted genes, 1,548 and 1,611 are significantly up-regulated and down-regulated, respectively. Thus, the vast majority of the genes, 7,774, are not significantly differentially regulated. About the same number of genes are up or down regulated in planta compared to in vitro. See results_exercise3.xlsx second tab.
>>
> {: .solution}
{: .challenge}

Let’s focus on genes that are commonly associated with virulence, secreted proteins.

> ## Question 3
> 
> Which criteria do you need to use to identify putatively secreted proteins? What is the size of the predicted secretome? Compare the number of predicted secreted proteins in each category (significantly up- and down-regulated).
>
>> ## Solution
>> 
>> Proteins that are most likely secreted should contain a predicted signal peptide and no transmembrane domain (sort according to q value significance, signal peptide and number of TM domains). 728 genes encode putative secreted proteins. Out of those, 123 and 99 are significantly up- and down-regulated, respectively. See results_exercise3.xlsx second tab.
>>
> {: .solution}
{: .challenge}


> ## Question 4
> 
> What are the characteristics of SSPs? How many of the predicted up-regulated or down-regulated genes encode SSPs?
>
>> ## Solution
>> 
>> SSPs are secreted proteins that are shorter than 300 amino acids (by convention) and contains several cysteine residues (this data is not available for you, so it is not considered here). Add an additional sorting according to the protein length. There is a total of 327 SSPs encoded in the genome, out of which 56 and 52 are significantly up- and down-regulated, respectively.
>>
> {: .solution}
{: .challenge}


> ## Question 5
> 
> Which functions are represented in the up- or down-regulated SSPs?
>
>> ## Solution
>> 
>> 35 out of 56 up-regulated SSPs do not have any conserved domain, which is a very common feature of SSPs. It is difficult to know what could be their role. Other interesting functions are 2 hydrophobins (Mycgr3|95491 and Mycgr3|96543), 2 proteases (Mycgr3|30802 and Mycgr3|91252), two putative toxic effectors (Mycgr3|111636 and Mycgr3|88451), one chloroperoxidase (Mycgr3 67060) and 3 carbohydrate-active enzymes (Mycgr3|105871, Mycgr3|74453 and Mycgr3|30121), which are functions that have been reported in the virulence of other plant pathogens.*
>> 33 out of 52 down-regulated SSPs do not have any conserved domain. 1 hydrophobin (Mycgr3|48129), 3 carbohydrate-active enzymes (Mycgr3|33493, Mycgr3|104658, Mycgr3|71724) and 2 proteases (Mycgr3|76021, Mycgr3|49854) are also down-regulated and thus are unlikely involved in virulence. Surprisingly, proteins known to play a role in virulence are also down-regulated here: cerato-platanin toxin (Mycgr3|39947), CFEM-domain effectors (Mycgr3|94274, Mycgr3|98580) and LysM effector (Mycgr3|111221).
>>
> {: .solution}
{: .challenge}


> ## Question 6
> 
> Based on these first results, can you formulate any hypothesis on potential virulence factors? Can you identify limitations that makes conclusions difficult to make?
>
>> ## Solution
>> 
>> The significantly up-regulated SSPs are good candidates to further study. When analysing such results, it is important to keep in mind that you are using only one time point during infection of wheat. But pathogenicity is a dynamic process and different set of genes are up-regulated at different time points during infection.
>>
> {: .solution}
{: .challenge}

Another way to analyse this data is to look at the differentially regulated genes with the most significant difference between the two conditions, and considering the genes that exhibit the highest expression on wheat while very low or not expressed *in vitro*. Sort the data according to these criteria and focus on the first 50 most significantly expressed genes.

> ## Question 7
> 
> How many SSPs are in this list? Can you identify interesting functions that might be important in the pathogenicity of *Z. tritici*
>
>> ## Solution
>> 
>> 3 SSPs are particularly highly expressed in planta (Mycgr3|77055, Mycgr3|102996, Mycgr3|111505; Mycgr3|78594 is actually not an SSP because it is predicted to be anchored in the membrane). Three secreted chloroperoxidase enzymes are highly expressed (Mycgr3|74298, Mycgr3|43487, Mycgr3|63538). Several putative transcription factors are highly up-regulated (Mycgr3|104366, Mycgr3|40797, Mycgr3|67865) and could be involved in regulating sets of pathogenicity genes. See results_exercise3.xlsx third tab.
>>
> {: .solution}
{: .challenge}


{% include links.md %}
