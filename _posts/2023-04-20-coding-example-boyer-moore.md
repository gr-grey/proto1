---
title: 'Coding example: Boyer-Moore Matching Algorithm'
date: 2023-04-20
excerpt: "Writing Boyer-Moore machtching from scratch in Python, implementing the bad character rule and the good suffix rule."
permalink: /posts/2023/04/coding-example-boyer-moore/
tags:
  - ngs
  - programming
  - algorithms
  - python
---

Play around with the codes in this [Google Colab](https://colab.research.google.com/github/gr-grey/genomic-courses/blob/main/boyer_moore_matching.ipynb) file.

We'll implement the Boyer-Moore algorithm and use it to match short DNA reads to a genome. 

We will compare Boyer-Moore and naive exact match by checking
>  (a) the number of character comparisons performed and 
>
>  (b) the number of alignments tried. 

These measurements indicate the amount of calcualtion/time needed for the algorithms.

-------

## Coding project

Download the genome file (part of human chromosome 1) and address the questions below.

Q1. How many alignments does Boyer-Moore try when matching the string GGCGCGGTGGCTCACGCCTGTAATCCCAGCACTTTGGGAGGCCGAGG (derived from human Alu sequences) to the excerpt of human chromosome 1?

Q2. How many alignments does the naive exact matching algorithm try when matching the string GGCGCGGTGGCTCACGCCTGTAATCCCAGCACTTTGGGAGGCCGAGG (derived from human Alu sequences) to the excerpt of human chromosome 1?

Q3. How many character comparisons does the naive exact matching algorithm try when matching the string GGCGCGGTGGCTCACGCCTGTAATCCCAGCACTTTGGGAGGCCGAGG (derived from human Alu sequences) to the excerpt of human chromosome 1?

Files needed:

The Python module for Boyer-Moore preprocessing:

http://d28rh4a8wq0iu5.cloudfront.net/ads1/code/bm_preproc.py

This module provides the BoyerMoore class, which encapsulates the preprocessing info used by the boyer_moore function above. Second, download the provided excerpt of human chromosome 1:

The reference genome:

http://d28rh4a8wq0iu5.cloudfront.net/ads1/data/chr1.GRCh38.excerpt.fasta

```python
# get files needed

# the module that preprocessing P,
# builds the look up table
# for the bad character rule and good suffix rule

!wget http://d28rh4a8wq0iu5.cloudfront.net/ads1/code/bm_preproc.py

# genome file
!wget http://d28rh4a8wq0iu5.cloudfront.net/ads1/data/chr1.GRCh38.excerpt.fasta

# read and store genome

def readGenome(fastAfile): 
    genome = ''
    with open(fastAfile, 'r') as f:
        for line in f:
            if not line[0] == '>':
                genome += line.rstrip()
    return genome

genome = readGenome('./chr1.GRCh38.excerpt.fasta')
```

## Q1 Number of alignments for Boyer-Moore

```python
from bm_preproc import *
def boyer_moore_with_counts(p, p_bm, t):
    i = 0 # current alignemnt/ offset of checking
    occurrences = []
    num_alignments, num_char_compred = 0, 0 
    while i < len(t) - len(p) + 1: # the last checking point before p goes out of bounds
        num_alignments += 1
        shift = 1
        mismatched = False
        for j in range(len(p) - 1, -1, -1): # j goes from the index of last char in p, then second last, all the way to 0
            num_char_compred += 1
            if p[j] != t[i+j]: # found mismatch
                shift_bc = p_bm.bad_character_rule(j, t[i+j]) # bad character rule
                shift_gs = p_bm.good_suffix_rule(j) # good suffix rule
                shift = max(shift_bc, shift_gs)
                mismatched = True
                break
        if not mismatched: # entire p matched
            occurrences.append(i)
            shift_gs = p_bm.match_skip()
            shift = max(shift, shift_gs)

        i += shift

    return occurrences, num_alignments, num_char_compred

p = 'GGCGCGGTGGCTCACGCCTGTAATCCCAGCACTTTGGGAGGCCGAGG'
p_bm = BoyerMoore(p)
print(f'Number of alignments for Boyer Mooer: {boyer_moore_with_counts(p, p_bm, genome)[1]}')
```

## Q2,3 Nuber of alignments & character comparison for naive exact matching

```python
def naive_with_counts(p,s): # exact match of p in s
    occurrences = []
    num_alignment, num_char_compared = 0, 0
    for i in range(len(s) - len(p) + 1): # i indiates the alignment
        num_alignment += 1
        match = True
        for j in range(len(p)):
            num_char_compared += 1
            if not p[j] == s[i+j]: # find mismatch
                match = False
                break
        if match:
            occurrences.append(i)
    return occurrences, num_alignment, num_char_compared

p = 'GGCGCGGTGGCTCACGCCTGTAATCCCAGCACTTTGGGAGGCCGAGG'
print(f'Number of alignments in naive exact matching {naive_with_counts(p, genome)[1]}')
print(f'Number of chars compared in naive exact matching {naive_with_counts(p, genome)[2]}')
```

-------

Appendix: Boymer_Moore Class Implementation
========

Showing the content of the bm_preproc.py for reference

Creating a p_bm object that preprocesses pattern p,
implementing the look up tables for the bad character rule and the good suffix rule.

```python
#!/usr/bin/python3

"""bm_preproc.py: Boyer-Moore preprocessing."""

__author__ = "Ben Langmead"

def z_array(s):
    """ Use Z algorithm (Gusfield theorem 1.4.1) to preprocess s """
    assert len(s) > 1
    z = [len(s)] + [0] * (len(s)-1)

    # Initial comparison of s[1:] with prefix
    for i in range(1, len(s)):
        if s[i] == s[i-1]:
            z[1] += 1
        else:
            break

    r, l = 0, 0
    if z[1] > 0:
        r, l = z[1], 1

    for k in range(2, len(s)):
        assert z[k] == 0
        if k > r:
            # Case 1
            for i in range(k, len(s)):
                if s[i] == s[i-k]:
                    z[k] += 1
                else:
                    break
            r, l = k + z[k] - 1, k
        else:
            # Case 2
            # Calculate length of beta
            nbeta = r - k + 1
            zkp = z[k - l]
            if nbeta > zkp:
                # Case 2a: zkp wins
                z[k] = zkp
            else:
                # Case 2b: Compare characters just past r
                nmatch = 0
                for i in range(r+1, len(s)):
                    if s[i] == s[i - k]:
                        nmatch += 1
                    else:
                        break
                l, r = k, r + nmatch
                z[k] = r - k + 1
    return z


def n_array(s):
    """ Compile the N array (Gusfield theorem 2.2.2) from the Z array """
    return z_array(s[::-1])[::-1]


def big_l_prime_array(p, n):
    """ Compile L' array (Gusfield theorem 2.2.2) using p and N array.
        L'[i] = largest index j less than n such that N[j] = |P[i:]| """
    lp = [0] * len(p)
    for j in range(len(p)-1):
        i = len(p) - n[j]
        if i < len(p):
            lp[i] = j + 1
    return lp


def big_l_array(p, lp):
    """ Compile L array (Gusfield theorem 2.2.2) using p and L' array.
        L[i] = largest index j less than n such that N[j] >= |P[i:]| """
    l = [0] * len(p)
    l[1] = lp[1]
    for i in range(2, len(p)):
        l[i] = max(l[i-1], lp[i])
    return l


def small_l_prime_array(n):
    """ Compile lp' array (Gusfield theorem 2.2.4) using N array. """
    small_lp = [0] * len(n)
    for i in range(len(n)):
        if n[i] == i+1:  # prefix matching a suffix
            small_lp[len(n)-i-1] = i+1
    for i in range(len(n)-2, -1, -1):  # "smear" them out to the left
        if small_lp[i] == 0:
            small_lp[i] = small_lp[i+1]
    return small_lp


def good_suffix_table(p):
    """ Return tables needed to apply good suffix rule. """
    n = n_array(p)
    lp = big_l_prime_array(p, n)
    return lp, big_l_array(p, lp), small_l_prime_array(n)


def good_suffix_mismatch(i, big_l_prime, small_l_prime):
    """ Given a mismatch at offset i, and given L/L' and l' arrays,
        return amount to shift as determined by good suffix rule. """
    length = len(big_l_prime)
    assert i < length
    if i == length - 1:
        return 0
    i += 1  # i points to leftmost matching position of P
    if big_l_prime[i] > 0:
        return length - big_l_prime[i]
    return length - small_l_prime[i]


def good_suffix_match(small_l_prime):
    """ Given a full match of P to T, return amount to shift as
        determined by good suffix rule. """
    return len(small_l_prime) - small_l_prime[1]


def dense_bad_char_tab(p, amap):
    """ Given pattern string and list with ordered alphabet characters, create
        and return a dense bad character table.  Table is indexed by offset
        then by character. """
    tab = []
    nxt = [0] * len(amap)
    for i in range(0, len(p)):
        c = p[i]
        assert c in amap
        tab.append(nxt[:])
        nxt[amap[c]] = i+1
    return tab


class BoyerMoore(object):
    """ Encapsulates pattern and associated Boyer-Moore preprocessing. """

    def __init__(self, p, alphabet='ACGT'):
        # Create map from alphabet characters to integers
        self.amap = {alphabet[i]: i for i in range(len(alphabet))}
        # Make bad character rule table
        self.bad_char = dense_bad_char_tab(p, self.amap)
        # Create good suffix rule table
        _, self.big_l, self.small_l_prime = good_suffix_table(p)

    def bad_character_rule(self, i, c):
        """ Return # skips given by bad character rule at offset i """
        assert c in self.amap
        assert i < len(self.bad_char)
        ci = self.amap[c]
        return i - (self.bad_char[i][ci]-1)

    def good_suffix_rule(self, i):
        """ Given a mismatch at offset i, return amount to shift
            as determined by (weak) good suffix rule. """
        length = len(self.big_l)
        assert i < length
        if i == length - 1:
            return 0
        i += 1  # i points to leftmost matching position of P
        if self.big_l[i] > 0:
            return length - self.big_l[i]
        return length - self.small_l_prime[i]

    def match_skip(self):
        """ Return amount to shift in case where P matches T """
        return len(self.small_l_prime) - self.small_l_prime[1]
```