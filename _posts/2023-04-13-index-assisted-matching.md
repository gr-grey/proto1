---
title: 'Indexing the genome - index assisted pattern matching, sorted list & hash table, subsequences vs substrings'
date: 2023-04-13
permalink: /posts/2023/04/index-assisted-matching/
tags:
  - ngs
  - programming
  - algorithms
---

The [Boyer-Moore](https://gr-grey.github.io/posts/2023/04/boyer-moore-matching-algorithm/) matching algorithm uses the bad character and the good suffix rules to skip futile alignments and speed up the search.
It preprocesses the pattern P to build up look up tables for the two rules.

Another way to speed up the search is to preprocess the text T.
Just like indices at the end of a book, the purpose of indexing/preprocessing T is to help us quickly locate the content of interest (pattern P) in T.

Matching/searching algorithms where T is preprocessed is called an **offline search**, otherwise it's called an **online search**.

Note that this definition does not care if P is preprocessed or not, as algorithms that preprocesses P usually do it on the fly, whereas the preprocessing of T is usually done before hand and the indices stored somewhere for a quick look up or search. 
For example, the Boyer-Moore algorithm is an online searching algorithm, as it does not preprocess the text T.

In our read alignment problem, many sequencing reads need to be aligned to the same reference genome, so even it might take a long time to index the genome, the cost vanishes over billions of alignments.

## Indexing approaches: sorted lists, hash tables

To index the reference genome, which is a string of text T we want to align the pattern P to, we need to pick a length of substring and chop T up to get all possible substrings in T, each substring is called a **key**, we need to store the keys and their locations/offsets for **query** the index.

If length of each key is k, it's called a **k-mer** index. Here's an example of building a 5-mer index for T, which is to get all possible substrings of T with length 5.

 <pre>
   0123456    
T: CGTGCGTGCTT
   <span style="color:crimson;text-decoration:underline">CGTGC</span>GTGCTT 
   C<span style="color:darkorange;text-decoration:underline">GTGCG</span>TGCTT
   CG<span style="color:goldenrod;text-decoration:underline">TGCGT</span>GCTT
   CGT<span style="color:green;text-decoration:underline">GCGTG</span>CTT
   CGTG<span style="color:darkcyan;text-decoration:underline">CGTGC</span>TT
   CGTGC<span style="color:dodgerblue;text-decoration:underline">GTGCT</span>T
   CGTGCG<span style="color:blueviolet;text-decoration:underline">TGCTT</span>

Index of T
CGTGC: <span style="color:crimson">0</span>, <span style="color:darkcyan">4</span>
GTGCG: <span style="color:darkorange">1</span>
TGCGT: <span style="color:goldenrod">2</span>
GCGTG: <span style="color:green">3</span>
GTGCT: <span style="color:dodgerblue">5</span>
TGCTT: <span style="color:blueviolet">6</span>
</pre>

 Two common structures for building indices are sorted lists and hash tables.
 
### Sorted list

- Sort keys alphabetically for quick searches/queries.
- A sorted list allows us to perform a binary search and find the key in Log2(N) time.
- The sorted list can be implemented in an array or a linked list, with each element of the list consisting of a key and a pointer to the corresponding data item. 
<pre>
Sort keys alphabetically
CGTGC: <span style="color:crimson">0</span>, <span style="color:darkcyan">4</span>
GCGTG: <span style="color:green">3</span>
GTGCG: <span style="color:darkorange">1</span>
GTGCT: <span style="color:dodgerblue">5</span>
TGCGT: <span style="color:goldenrod">2</span>
TGCTT: <span style="color:blueviolet">6</span>
</pre>

### Hash tables

- The keys can also be stored as a hash table/ hash map/ dictionary.
- The data structure allows constant O(1) look up time.

## Index assisted exact search: query & verification

Once a k-mer index is built for the text T, for each pattern P, a match can be found by 2 steps.
1. Take the first k characters of P (`P[:k]`), it has to match to one of the keys in the index. The matches/hits can be retrieved by querying the index.
2. For each hit, verify if that the rest of P (`P[k:]`) is also matched with T. Say a hit has offset h, we need to compare whether `P[k:]` matches `T[h+k: h+k+len(p)]`

Note: it doesn't have to be the first kmer in P for the query, the second, third, or any kmer in P would work the same, but for each hit, we still need to check whether the remaining part (all characters outside of that kmer we query) matches T.

## Not all k-mers required: every nth kmer

When we record all k-mers (substrings) in the index, we can query using *any* (the 1st, 2nd, 3rd, etc.) k-mer of P, which indicates there may be some redundancy to the k-mer library - if we can utilize multiple kmers from P.

### Every other k-mer

What if instead of every k-mer, we only record every other k-mer in T? 

It means the k-mers are taken form either even offsets (0, 2, 4, 6, ...) or odd offsets (1, 3, 5, 7, ...).
Let's take the even offsets as an example.

- When recording all k-mers of T (t_kmer), for each P, we can take any k-mer of P (p_kmer) and query and verify if it's a match.
- If the index only has k-mers with even offset, for a p_kmer, we lose the hits where p_kmer matches a t_kmer with an odd offset.
- However, for each case where p_kmer matches a t_kmer with odd offset and eventually leads to a *full match*, the kmer right next to the p_kmer in P (say p_kmer') would match to the kmer right next to t_kmer - which would be a t_kmer with *an even offset*.
- So if we query both p_kmer and p_kmer' (the kmer right next to it) against t_kmers with even offset, we can cover the matches yielded from p_kmer matching an odd offset t_kmer. 

Note that p_kmer' doesn't need to be right next to p_kmer, just one of them needs to have a even offset (in P), and another to have an odd offset.

### Every nth k-mer

In the generalized case of recording every nth k-mer in T, we need to check n different kmers in P, the offset of those p_kmers need to cover every module of n, i.e., there needs to be a p_kmer with offset % n equals to 0, another with offset % n equals to 1, etc., all the way to offset % n equals n - 1.

For example, we can take every 3rd t_3mer, and for each P, we need to query 3 p_3mers with offset mod 3 equals to 0, 1, 2, respectively, there we chose 3mers starting at offset 3, 1 and 5, respectively. 

<pre>
Every 3rd k-mer
       T: <span style="color:dodgerblue">∎∎∎∎∎∎∎∎∎∎∎∎∎∎</span>
   Index: <span style="color:dodgerblue">∎∎∎   ∎∎∎</span>   
every 3rd    <span style="color:dodgerblue">∎∎∎   ∎∎∎</span>

       P: <span style="color:forestgreen">∎∎∎∎∎∎∎∎</span>
              <span style="color:forestgreen">∎∎∎</span> 0 mod 3
   Query:  <span style="color:forestgreen">∎∎∎</span> 1 mod 3
                <span style="color:forestgreen">∎∎∎</span> 2 mod 3
</pre>

Taking only every nth k-mer will make the index smaller and faster to query.

## Subsequences vs substrings

So far we've taken strings - continuous chunks of k-mers.

More generally the index can be sliced with any pattern, like the following pattern of extracting characters at offset `[0-1, 3, 5-6]`.

<pre>
   012345678    
T: CGTGCGTGCTT
   <span style="color:dodgerblue;text-decoration:underline">CG</span>T<span style="color:dodgerblue;text-decoration:underline">G</span>C<span style="color:dodgerblue;text-decoration:underline">GT</span>GCTT 
   C<span style="color:dodgerblue;text-decoration:underline">GT</span>G<span style="color:dodgerblue;text-decoration:underline">C</span>G<span style="color:dodgerblue;text-decoration:underline">TG</span>CTT
   CG<span style="color:dodgerblue;text-decoration:underline">TG</span>C<span style="color:dodgerblue;text-decoration:underline">G</span>T<span style="color:dodgerblue;text-decoration:underline">GC</span>TT
   ...
Sorted index of T
CGGGT: 0
CGGTT: 4
GCTCT: 3
GTCTG: 1
TGGGC: 2

T: CGTGCGTGCTT
P: <span style="color:dodgerblue;text-decoration:underline">GC</span>G<span style="color:dodgerblue;text-decoration:underline">T</span>A<span style="color:dodgerblue;text-decoration:underline">CT</span>
</pre>

In this case we need to slice out the same pattern in P for query, like shown above.

Similarly, we can skip to take every nth pattern in T, and check every offset with every mod of n in P

Using subsequences instead of substrings allows us to match different locations in P at the same time, which leads to a higher successful rate during the verification steps.
