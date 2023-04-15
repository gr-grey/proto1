---
title: 'Coding example: index assited approximate matching: kmer, sorted list, pigeonhole principle and subsequence'
date: 2023-04-15
permalink: /posts/2023/04/coding-example-index-assisted-matching/
tags:
  - ngs
  - programming
  - algorithms
  - python
---

Here we implement the index assisted mathcing algorithm by recording substrings/subsequences of text T into a sorted list, and implement the approximate matching according to pigeonhole principle. 

Play around with the codes in this [Google Colab](https://colab.research.google.com/github/gr-grey/genomic-courses/blob/main/index_assisted_match_approximate_pigeonhole_kmer_and_subsequences.ipynb) file.  

## Coding problems:

Download the genome file (part of human chromosome 1) and address the questions below.

### Kmer index in sorted list and approximate matching

Implement the pigeonhole principle using Index to find exact matches for the partitions. Assume P always has length 24, and that we are looking for approximate matches with up to 2 mismatches (substitutions). We will use an 8-mer index. 

Write a function that, given a length-24 pattern P and given an Index object built on 8-mers, finds all approximate occurrences of P within T with up to 2 mismatches. Insertions and deletions are not allowed. Don't consider any reverse complements.

Q1.How many times does the string GGCGCGGTGGCTCACGCCTGTAAT, which is derived from a human Alu sequence, occur with up to 2 substitutions in the excerpt of human chromosome 1?  (Don't consider reverse complements here.)

- Note: Multiple index hits might direct you to the same match multiple times, but be careful not to count a match more than once.
  
Q2. Using the instructions given in Question 4, how many total index hits are there when searching for occurrences of GGCGCGGTGGCTCACGCCTGTAAT with up to 2 substitutions in the excerpt of human chromosome 1?  (Don't consider reverse complements here.)  


```python
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

# creat sorted indices and query method
import bisect # for binary search
class Index(object): 
    def __init__(self, t, k): # kmer index for t
        self.k = k
        self.index = []
        for i in range(len(t) - k + 1):
            self.index.append((t[i: i+k], i)) # each element is (kmer_string, offset of kmer string)
        self.index.sort()
    
    def query(self, p):
        kmer = p[:self.k]
        i = bisect.bisect_left(self.index, (kmer, -1)) # i is the position in the sorted index list where we find the first /most left hit
        hits = []
        while i < len(self.index): # keep going to the right to record all hits
            if self.index[i][0] != kmer:
                break
            hits.append(self.index[i][1])
            i += 1
        return hits

def queryIndex(p, t, index):
    k = index.k
    offsets = []
    for i in index.query(p): # check if each kmer match/hit is an exact match, i.e. if the rest of string mataches
        if i + len(p) > len(t): # p goes out of bound
            continue
        if p[k:] == t[i+k: i + len(p)]: 
            offsets.append(i)
    return offsets

def approximate_match_index(p, t, index, n): # need k to build a kmer index obj
    k = index.k

    # divide p to n+1 segments
    seg_len = int(round(len(p) /  (n+1)))
    all_matches = set() # avoid recording the same match from multiple segment at same alignment/offset position

    num_hits = 0

    for i in range(n+1): # loop trough each segment p[i * seg_len, (i+1) * seg_len], last segment edge case
        start = i * seg_len
        end = min( (i+1) * seg_len, len(p) ) # the last segment end should not surpass len(p)
        seg = p[start:end]

        seg_matches = queryIndex(seg, t, index) 
        num_hits += len(seg_matches)

        # check if rest of p matched within the allowed mistake counts
        for m in seg_matches:
            if m - start < 0 or m - start + len(p) > len(t):
                continue # p goes out of t in this alignment
            mismatches = 0
            for j in range(start): # check p[:start]
                if p[j] != t[m - start +j]:
                    mismatches += 1
                    if mismatches > n:
                        break
            for j in range(end, len(p)): # check p[end:] 
                if p[j] != t[m - start +j]:
                    mismatches += 1
                    if mismatches >n:
                        break
            if mismatches <= n:
                all_matches.add(m - start)
    return list(all_matches), num_hits

p2 = 'GGCGCGGTGGCTCACGCCTGTAAT'
index = Index(genome, 8)

print(f'Number of matches with upto 2 mismatches: {len(approximate_match_index(p2, genome, index, 2)[0])}')
print(f'Total index hits: {approximate_match_index(p2, genome, index, 2)[1]}') 
```

## Q3 Subsequence Index

Let's examine whether there is a benefit to using an index built using subsequences of T rather than substrings, as we discussed in the "Variations on k-mer indexes" video.  We'll consider subsequences involving every N characters.  For example, if we split ATATAT into two substring partitions, we would get partitions ATA (the first half) and TAT (second half).  But if we split ATATAT into two  subsequences  by taking every other character, we would get AAA (first, third and fifth characters) and TTT (second, fourth and sixth).

Another way to visualize this is using numbers to show how each character of P is allocated to a partition.  Splitting a length-6 pattern into two substrings could be represented as 111222, and splitting into two subsequences of every other character could be represented as 121212.

Implement a SubseqIndex class that build index with every Nth (ival) character, which would be a more general implementation of Index.

Write a function that, given a length-24 pattern P and given a SubseqIndex object built with k = 8 and ival = 3, finds all approximate occurrences of P within T with up to 2 mismatches.

Q3. When using this function, how many total index hits are there when searching for GGCGCGGTGGCTCACGCCTGTAAT with up to 2 substitutions in the excerpt of human chromosome 1?  (Again, don't consider reverse complements.)


```python
import bisect
   
class SubseqIndex(object):
    """ Holds a subsequence index for a text T """
    
    def __init__(self, t, k, ival):
        """ Create index from all subsequences consisting of k characters
            spaced ival positions apart.  E.g., SubseqIndex("ATAT", 2, 2)
            extracts ("AA", 0) and ("TT", 1). """
        self.k = k  # num characters per subsequence extracted
        self.ival = ival  # space between them; 1=adjacent, 2=every other, etc
        self.index = []
        self.span = 1 + ival * (k - 1)
        for i in range(len(t) - self.span + 1):  # for each subseq
            self.index.append((t[i:i+self.span:ival], i))  # add (subseq, offset)
        self.index.sort()  # alphabetize by subseq
    
    def query(self, p):
        """ Return index hits for first subseq of p """
        subseq = p[:self.span:self.ival]  # query with first subseq
        i = bisect.bisect_left(self.index, (subseq, -1))  # binary search
        hits = []
        while i < len(self.index):  # collect matching index entries
            if self.index[i][0] != subseq:
                break
            hits.append(self.index[i][1])
            i += 1
        return hits

def approx_query_subseq(p, t, subseq_ind, n):
    k = subseq_ind.k

    num_index_hits = 0
    offsets = set()
    for i in range(k): # need to query p[0:], p[1:], ..., p[k-1:]
        hits = subseq_ind.query(p[i:]) 
        num_index_hits += len(hits)
        for h in hits: # check if the entire p matches
            start = h - i
            end = h - i + len(p)
            if start < 0 or end > len(t):
                continue
            mismatch = 0
            for j in range(len(p)):
                if p[j] != t[start + j]:
                    mismatch += 1
                    if mismatch > n:
                        break
            if mismatch <= n:
                offsets.add(start)
    return offsets, num_index_hits


read = 'GGCGCGGTGGCTCACGCCTGTAAT'
subseq_ind = SubseqIndex(genome, 8, 3)

occurrences, num_index_hits = approx_query_subseq(read, genome, subseq_ind, 2)
print(f'Total index hits: {num_index_hits}')
```
