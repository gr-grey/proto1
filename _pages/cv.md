---
layout: archive
title: "CV - Rui Guo"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Skills
======
* Programming: intermediate C++ & python (STL data structures, object orienting programing, python libs - numpy, pandas, matplotlib)  *\[6+years\]*
* Developer tools: Linux Command Line & Git, high-performance cluster maintenance  *\[6+years\]*
* Computational Protein Engineering 
	* Extensive knowledge in protein design (sequence prediction) problem.  *\[6+years\]*
	* Practice of implementing an entropy based statistical algorithm in C++ program to solve the protein design problem.  *\[6+years\]*
	* Using MD (molecular dynamics) simulations (NAMD) to investigate protein interactions.  *\[6+years\]*
	* Protein visualization and animation with VMD & Pymol (render materials, lighting, electrostatic surfaces, RMSD & heatmap analysis, movie making).  *\[6+years\]*
	* Collaboration with protein experimentalists, applying theoretical models to interpret data (Mass, CD, melting temperature, PI, charge-PH relations).  *\[5+years\]*
* Bioinformatics/ Genomics Courses - Introduction to Genomics Technologies; Python for Genomic Data Science, Algorithms for DNA sequences. 

Education
======
### Ph.D., University of Pennsylvania, *May 2023* (expected) 
* Physical Chemistry - Advisor: **Dr. Jeffery Saven**; - GPA: 3.90/4.0 
* Thesis: Computational engineering of protein features: charge variation, host-guest assembly, and structural motif.

### B.S., University of Science and Technology of China, *2016* 
* Chemistry - Advisor: **Dr. Tongwen Xu**; GPA: 3.63/4.3 
    * Awarded Outstanding Student Scholarship for 4 years 
	    * Grade 1 (top 3%) in 2015; Grade 2 (top 10%) in 2013 & 2014; Grade 3 (top 25%) in 2012
  
Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Research Experience
======
### **2017 - 2023: Graduate Research Associate**, University of Pennsylvania
Applying a statistical model to solve the protein design problem, i.e., calculating amino acid sequences that when synthesized, fold into target 3D protein structures, to create proteins that possess desired functionalities, such as high thermal stability, enzyme activity or host-guest assemblies

* **Computational Protein Design** (2 projects • 18 designed proteins • 16 confirmed to fold correctly)
	1. Designed a wide variation of charges for a short peptide of 29 amino acid residues (~3600 Da).
		- 17 peptide sequences calculated, each with distinguished charges from -8, -7, -6, …, +6, +7, +8 (17 charge states). 
		- 15 of the 17 sequences folded as desired (88% success rate).
		- 13 of the 18 exterior residues (72%) in the peptide were modified without disturbing the folded structures.
	1. Designed a super-positive variant of human carbonic anhydrase II enzyme with net charge of +21 (natural enzyme has -1 charge).    
		 - This is the first example of a computationally designed super-positively charged enzyme.
		 - 20 out of the enzyme’s 70 exterior residues (28%) were mutated, without compromising the enzyme’s folded structure or eliminating functionality (81% activity retained).
		 - The super-positive enzyme variant can be directly encapsulated within a ferritin cage (negatively charged), without adding a super-positively charged fusion partner (previous encapsulation method). 
		 - Encapsulation leads to enhanced thermal stability and activity (63% enhanced at 40 °C). 

- **Molecular dynamics (MD) simulations** (Uncovering unstable interactions in problematic sequences.)
	- For sequences that did not fold correctly in synthesis, MD Simulations revealed unstable sidechain interactions near the peptides’ N-terminus. MD supported redesigns which eliminated the found interactions folded correctly in experiments.
 
### **2014 - 2016: Undergraduate Researcher**, University of Science and Technology of China
- Synthesized 3 microporous anion exchange membranes (crucial component of fuel cells) with exceptional high hydroxide conductivity and chemical stability. At the time of publication, one of the membranes demonstrated the highest conductivity at 80 °C reported in the field.
  
Leadership, Teaching and Outreach
======
