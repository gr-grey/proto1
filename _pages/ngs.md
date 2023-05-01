---
title: "NGS"
permalink: /ngs/
author_profile: true

---

{% include base_path %}

As I am curious about genomic and bioinformatics related research.
I took [Genomic Data Science Sepcialization](https://www.coursera.org/specializations/genomic-data-science) courses offered by Johns Hopkins University on Coursera.

Courses include **Introduction to Genomics Technologies**, **Command Line Tools for Genomics**, **Python for Genomics** and **Algorithms for DNA Sequencing**.
Certificates are listed at the [end of the page](#certificates).

# Course Notes

## [Introduction to Genomics](/courses/2023/03/introduction-to-genomics/)

* A summary of genomic-related topics, containing notes  I took from the [Introduction to Genomics](https://www.coursera.org/learn/introduction-genomics) course from JHU, and [Applied Computational Genomics](https://github.com/quinlan-lab/applied-computational-genomics) course from Dr. Aaron Quinlan at U of Utah, with a splash of my understandings on these topics.

## [Common File Formats and Software in Genomics](/courses/2023/03/common-file-formats-and-software-in-genomics/)

* A review of common formats in genomics: SAM/BAM, BED, GFF3/GFF/GTF, VCF/BCF, FastA/Q, as well as 3 course projects (adapted for better clarity).

    - [Project 1](/courses/2023/03/common-file-formats-and-software-in-genomics/#seg9): Unix basic (grep, cut, awk, comm, sort, uniq)
    - [Project 2](/courses/2023/03/common-file-formats-and-software-in-genomics/#seg10): Process SAM/BAM/BED/GTF files
    - [Project 3](/courses/2023/03/common-file-formats-and-software-in-genomics/#seg11): Align FastQ reads with bowtie2 and variant calling with bfctools

The course material came out in 2014 and some of the software it uses are obsolete now - it made me realize how fast things are moving in computational genomics, where the ability to learn and keep up with new algorithms and tools are crucial.

## Python for Genomics Data Science

* The course gives a brief introduction to python, and some broad advice of coding styles.
* I keep a [python note](/dev-logs/2023/04/python-notes/) for quick look ups.

<a name="algorithm"></a>
## Algorithms for DNA Sequencing 

* The course covers the math and practical implementation of algorithms for DNA sequence alignment and assembly. It was hands down the most fun I've had in the course series.

* [Naive exact matching algorithm](/posts/2023/04/naive-exact-matching-algorithm/) & [coding project](https://colab.research.google.com/github/gr-grey/genomic-courses/blob/main/naive_exact_match.ipynb): simple and straight forward.
* [Boyer-Moore matching algorithm](/posts/2023/04/boyer-moore-matching-algorithm/) & [coding project](https://colab.research.google.com/github/gr-grey/genomic-courses/blob/main/boyer_moore_matching.ipynb): the art of giving up.
* [Indexing the genome: index assisted matching](/posts/2023/04/index-assisted-matching/) with sorted list & hash table; subsequences vs substrings.
* [Approximate matching:](/posts/2023/04/approximate-matching-hamming-and-edit-distance-pigeonhole-principle) hamming and edit distance; pigeonhole principle.
    - [Coding example](/posts/2023/04/coding-example-index-assisted-matching/): kmer, sorted list, binary search, pigeonhole principle and subsequence.
* [Edit distance: recursive algorithm](/posts/2023/04/edit-distance-calculation/): calculate edit distance by reducing the problem to smaller sub-problems of edit distance, and calculate recursively.
* [Smith-Waterman dynamic programming](/posts/2023/04/dynamics-programming-for-edit-distance/): remembering the answer of the smaller sub-problems to massively reduce repetitive calculations.

<!-- Courses include [**Introduction to Genomics Technologies**](/files/course-introduction-to-genomics-technologies.pdf), [**Command Line Tools for Genomics**](/files/course-certificate-command-line-tools-for-genomic-data-science.pdf), [**Python for Genomics**](/files/course-certificate-python-for-genomic-data-science.pdf) and [**Algorithms for DNA Sequencing**](/files/course-certificate-algorithms-for-dna-sequencing.pdf) (the links take you to certificates). -->

# Certificates

## Introduction to Genomic Technologis

<object data="/files/course-certificate-introduction-to-genomic-technologies.pdf" width="1000" height="835" type='application/pdf'></object>

## Command Line Tools for Genomic Data Science

<object data="/files/course-certificate-command-line-tools-for-genomic-data-science.pdf" width="1000" height="835" type='application/pdf'></object>

## Python for Genomic Data Science

<object data="/files/course-certificate-python-for-genomic-data-science.pdf" width="1000" height="835" type='application/pdf'></object>

## Algorithms for DNA Sequencing

<object data="/files/course-certificate-algorithms-for-dna-sequencing.pdf" width="1000" height="835" type='application/pdf'></object>
