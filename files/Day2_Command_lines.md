*Microbial genomics, Functional annotation, Day 2*

Command lines
===
**Assignment 4: orthologous protein families**

What is the **core** proteome for these fungi? Determine the size of the core proteome \(number of clusters\). Does it have a similar size in each species \(number of proteins\)? Which fraction of the total proteome of each species does the core proteome represent?

> In Excel: =SUM(IF(B2>0; 1; 0); IF(C2>0; 1; 0); IF(D2>0; 1; 0); IF(E2>0; 1; 0); IF(F2>0; 1; 0); IF(G>0; 1; 0))
> In Terminal:
~~~
$ awk 'BEGIN{FS="\t"};{if($2>0 && $3>0 && $4>0 && $5>0 && $6>0 && $7>0) print $1}' RunId_31_counts.txt | wc -l
~~~

How many proteins are not assigned any function (*i.e.* no PFAM) in the core and specific proteomes? How many proteins are assigned a function (*i.e.* with a PFAM domain)? What is your conclusion?

> Core proteome:
~~~
$ awk 'BEGIN{FS="\t"};{if($2>0 && $3>0 && $4>0 && $5>0 && $6>0 && $7>0) print $8}' RunId_31_counts.txt | sort | uniq -c | sort -r | head -n 10
~~~
> Specific proteome (example for Clafu):
~~~
$ awk 'BEGIN{FS="\t"};{if($2>0 && $3==0 && $4==0 && $5==0 && $6==0 && $7==0) print $8}' RunId_31_counts.txt | sort | uniq -c | sort -r | head -n 10
~~~

** **

**Assignment 4: compare the gene content between different species**

Start with general statistics. Write a bash command that makes use
of **wc** to count the total number of proteins in each annotation file. Then, write a bash
command that makes use of **awk** and **wc** to count the number of
proteins with transmembrane domains, the number of proteins that are
predicted to be secreted.

> total number of proteins:
~~~
$ wc -l funannot.*.txt
~~~
> transmembrane proteins:
~~~
$ awk '{if($3>=1 && $4=="N") print $0}' funannot.*.txt | wc -l
~~~
> predicted secreted proteins:
~~~
$ awk '{if($4=="Y" && $3==0) print $0}' funannot.*.txt | wc -l
~~~

How many SSPs are encoded in each genome? Adapt the bash command
with **awk** and **wc** to automatically count the number of SSPs.

~~~
$ awk '{if($4=="Y" && $3==0 && $2<=300) print $0}' funannot.*.txt | wc -l
~~~

For counting secreted peptidases/proteases, adapt the bash command
using **grep** to retrieve the genes with the terms “peptidase” or
“protease” description in the functional annotation.

~~~
$ awk '{if($4=="Y" && $3==0) print $0}' funannot.*.txt | grep -c "peptidase\|protease"
~~~

Adapt the previous bash command to retrieve secreted CAZymes
(use the terms “Glyco\_hydro”, “Glyco\_transf” and “CBM” pfam domains in the
functional annotation).

~~~
$ awk '{if($4=="Y" && $3==0) print $0}' funannot.*.txt | grep -c " Glyco_hydro\|Glyco_transf\|CBM"
~~~

Adapt the following bash command to retrieve PKS and NRPS proteins,
which are very large proteins, using the signature domains PF02801 (PKS)
and PF00668 (NRPS)

~~~
$ grep "PF02801\|PF00668" funannot.*.txt | awk '{if($2>1000) print $0}' | wc -l
~~~




