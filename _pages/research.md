---
layout: archive
title: "Research & Publications"
permalink: /research/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

## Graduate Research at University of Pennsylvania

- **Computational Protein Design** (2 projects • 18 designed proteins • 16 confirmed to fold correctly)

	1. Designed a wide variation of charges for a short peptide of 29 amino acid residues (~3600 Da).
		- 17 peptide sequences calculated, each with distinguished charges from -8, -7, -6, …, +6, +7, +8 (17 charge states). 
		- 15 of the 17 sequences folded as desired (88% success rate).
		- 13 of the 18 exterior residues (72%) in the peptide were modified without disturbing the folded structures.
	1. Designed a super-positive variant of human carbonic anhydrase II enzyme with net charge of +21 (natural enzyme has -1 charge).    
		 - This is the first example of a computationally designed super-positively charged enzyme.
		 - 20 out of the enzyme’s 70 exterior residues (28%) were mutated, without compromising the enzyme’s folded structure or eliminating functionality (81% activity retained).
		 - The super-positive enzyme variant can be directly encapsulated within a ferritin cage (negatively charged), without adding a super-positively charged fusion partner (previous encapsulation method). 
		 - Encapsulation leads to enhanced thermal stability and activity (63% enhanced at 40 °C). 

- **Molecular dynamics (MD) simulations** (uncovering unstable interactions in problematic sequences.)

	- For sequences that did not fold correctly in synthesis, MD Simulations revealed unstable sidechain interactions near the peptides’ N-terminus. MD supported redesigns which eliminated the found interactions folded correctly in experiments.

# Publications

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
