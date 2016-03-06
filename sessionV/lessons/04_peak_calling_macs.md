---
title: "Peak calling with MACS2"
author: "Meeta Mistry"
date: "Thursday, March 3rd, 2016"
---

Contributors: Meeta Mistry

Approximate time: 90 minutes

## Learning Objectives

* Learning how to use MACS2for peak calling
* Understanding different components of the MACS2 algorithm
* Interpretation of results from the MACS2 peak caller

## MACS2

Another popular tool for identifying transcript factor binding sites is named [Model-based Analysis of ChIP-Seq (MACS)](https://github.com/taoliu/MACS). The [MACS algorithm](http://genomebiology.biomedcentral.com/articles/10.1186/gb-2008-9-9-r137) captures the influence of genome complexity to evaluate the significance of enriched ChIP regions. MACS improves the spatial resolution of binding sites through combining the information of both sequencing tag position and orientation. MACS can be easily used for ChIP-Seq data alone, or with control sample with the increase of specificity.


### Modeling the shift size
As we showed previously, the tag density around a true binding site should show a bimodal enrichment pattern. MACS takes advantage of this bimodal pattern to empirically model the shifting size to better locate the precise binding sites.

![model](../img/model.png)

Given a sonication size (`bandwidth`) and a high-confidence fold-enrichment (`mfold`), MACS slides 2bandwidth windows across the genome to find regions with tags more than mfold enriched relative to a random tag genome distribution. MACS randomly samples 1,000 of these high-quality peaks, separates their Watson and Crick tags, and aligns them by the midpoint between their Watson and Crick tag centers. The distance between the modes of the Watson and Crick peaks in the alignment is defined as 'd', and MACS shifts all the tags by d/2 toward the 3' ends to the most likely protein-DNA interaction sites.


### Peak detection

For experiments in which sequence depth differs between input and treatment samples, MACS linearly scales the total control tag count to be the same as the total ChIP tag count. Also, MACS allows each genomic position to contain no more than one tag and removes all the redundancies. 

To model the background noise, MACS uses a dynamic local Poisson distribution in which the lambda parameter is deduced by taking the maximum value λlocal = max(λBG, [λ1k,] λ5k, λ10k). Fold enrichment is then comuputed as the density of tags in a given peak compared to background λlocal parameter.


## Running MACS2

We will be using the newest version of this tool, MACS2. The underlying algorithm for peak calling remains the same, but it comes with some enhancements in functionality. One major difference on the computational end, is that for MACS the FDR for peaks were empirically determined whereas with MACS2 the FDR is computed using the Benjamini-Hochberg method. 

To run MACS2, we will first need to load the moule:

	$ module load seq/macs/2.1.0
	

There are seven [major functions](https://github.com/taoliu/MACS#usage-of-macs2) available in MACS serving as sub-commands. We will only cover `callpeak` in this lesson, but if interested you can use `macs2 COMMAND -h`  to find out more.



***
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*