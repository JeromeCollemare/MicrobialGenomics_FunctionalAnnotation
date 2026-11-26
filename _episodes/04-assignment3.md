---
title: "Functional annotation: Assignment 3 - search for conserved motifs"
teaching: 0
exercises: 60
questions:
- Functional annotation based on conserved motifs
objectives:
- Compare predictions from different tools and different versions
keypoints:
- Combining different tools is a good approach to validate a prediction
- Different versions of the same tool will generate different results
---

#  Background

In this exercise, you will determine **if proteins from *Z. tritici* contain conserved motifs**. These motifs or patterns often indicate specific protein structures or sub-cellular localization. Again, there are many different tools, but you will test three that are commonly used: signalP and tmhmm predicts signal peptides and transmembrane domains respectively, while targetP predict sub-cellular localizations.

Using SignalP (https://services.healthtech.dtu.dk/services/SignalP-4.1/), tmhmm (https://services.healthtech.dtu.dk/services/TMHMM-2.0/), and TargetP (https://services.healthtech.dtu.dk/services/TargetP-2.0/) manuals or help, prepare suitable commands.

Execute the three searches with the minimum number of arguments (if any) and using the file **~/data/fungalgenomics\_collemare/day1/Mycgr3.100proteins.fasta**. Save the results in text files.
NB: help of tmhmm is not easy to find, for a short output, use the parameter --short

Open the result files.

> ## Question 1
> 
> Which information is the most interesting in each case?
>
>> ## Solution
>> 
>> SignalP 4.1 predicts signal peptides using neural networks. For each sequence, SignalP summarises in column “?” if a signal peptide is present (Y) or not (N), according to the given discriminative score (D) for neural networks
>> tmhmm 2.0 predicts transmembrane helices using hidden Markov models. In the short output, the most informative column is "PredHel", which summarizes the number of predicted transmembrane helices, i.e. transmembrane domains. The "Topology" information is useful to know the position of the predicted helices in the protein.
>> TargetP 2.0 uses machine learning to predict mitochondrial target peptide (mTP), signal peptide (SP; and in this case the cleavage site CS), or other location (OTHER). The summary of the results is in the column "Prediction"
>>
> {: .solution}
{: .challenge}

Compare the results from the three tools.

> ## Question 2
> 
> Are they all consistent? Start by finding the proteins predicted to contain a signal peptide and compare the predictions with the other tools. Then continue with transmembrane predictions and compare signalP and tmhmm.
>
>> ## Solution
>> signalP predicts a signal peptide in 10 proteins. In addition, signalP predicts 12 proteins to contain an N-terminal transmembrane domain. TMHMM predicts transmembrane domains in 26 proteins. The prediction of SignalP and tmhmm are mostly consistent apart for Mycgr3|100022, which is predicted to contain one TM by SignalP, but none by tmhmm. Also, Mycgr3|100062 and Mycgr3|100559 are predicted to contain both a signal peptide and transmembrane domains, suggesting that these proteins may enter the secretory pathway but are unlikely to be secreted. TargetP predicts 9 proteins with a putative signal peptide and 0 protein is predicted to be located in mitochondria. TargetP did not report Mycgr3|100062 as a protein entering the secretory pathway. In general, only SignalP and transmembrane domain (TMHMM) predictions are used in annotation pipelines
>>
> {: .solution}
{: .challenge}

You used SignalP 4.0, tmhmm 2.0 and TargetP 2.0. Updates or new tools are regularly released and often lead to more accurate predictions. SignalP 6.0 and deepTMHMM 1.0 are newer versions of the tools you used and they are available online (https://services.healthtech.dtu.dk/services/SignalP-6.0/ and https://services.healthtech.dtu.dk/services/DeepTMHMM-1.0/).

Using the file **~/data/fungalgenomics\_collemare/day1/Mycgr3.100proteins.fasta**, run these tools with the online interface wiht parameters for Eukaryotes and short outputs.

> ## Question 3
> 
> For each tool, compare the results between the version you used and the later version. Are they all consistent?
> Can you identify an example where different predictions could have a strong implication on the prediction of the sub-cellulare localization? How do you evaluate the accuracy of the different predictions?
>
>> ## Solution
>> SignalP 6.0 predicts 9 proteins with a signal peptide. Mycgr3|100062 was predicted to have a signal peptide with verison 4.0 but not by TargetP, and a transmembrane domain was found by TMHMM. The results from 6.0 version appear unambiguous, and the previous predictiopn by version 4.0 was likely incorrect.
>> DeepTMHMM 1.0 predicts 25 proteins with transmembrane domains, but the results are the same with TMHMM 2.0 for 23 proteins only.  Mycgr3|100264 (2 TMs), Mycgr3|100498 (1 TM) and Mycgr3|100559 (1 TM) were predicted by TMHMM only, while Mycgr3_100383 (1 TM) and Mycgr3_100473 (2 TMs) were predicted by DeepTMHMM only. It is important to notice that Mycgr3|100559 is predicted to contain a signal peptide by both versions of SignalP, and thus predicting the presence or absence of a transmembrane domain has an important consequence on the prediction where this protein may be localized (secreted outside the cell or retained in a membrane). A more striking difference between TMHMM and DeepTMHMM is the number of predicted transmembrane domains. Out of 23, only 6 proteins (Mycgr3_100057, Mycgr3_100094, Mycgr3_100344, Mycgr3_100409, Mycgr3_100510 and Mycgr3_100550) are predicted with the same topology. The number of predicted transmembrane domains can vary greatly between the two tools, with the extreme case of Mycgr3_100515 in which TMHMM predicts 2 domains, but DeepTMHMM reports 10. In contrast to SignalP, there is no easy way to evaluate the accuracy of the transmembrane domain prediction. The precise position of these helices in the proteins should be considered very cautiously.
>>
> {: .solution}
{: .challenge}


{% include links.md %}
