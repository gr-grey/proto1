---
title: 'Boyer-Moore Matching Algorithm: The Art of Giving Up'
excerpt: "For searches that won't yield a match, givng up faster saves us time, the same principle applies to life too :)"
date: 2023-04-12
permalink: /posts/2023/04/boyer-moore-matching-algorithm/
header:
  overlay_color: black
  overlay_image: /banners/2023-04-17-algorithm-for-genomics-banner.png
tags:
  - ngs
  - programming
  - algorithms
  - python
---

Developed by Robert S. Boyer and J Strother Moore in 1977, the Boyer-Moore matching algorithm is widely used for pattern matching.
It's a common benchmark for exact matching.

In [naive exact matching](https://gr-grey.github.io/posts/2023/04/naive-exact-matching-algorithm/), the pattern P slides/shifts from left to right, and at each alignment, we check for matching from *left to right* for each substring of length P.

In Boyer-Moore matching, P also slides from left to right, but at each alignment, the matching is checked from *right to left*.

More importantly, when a mismatch is found, it uses two tricks to skip forward many alignments that do not need to checked. 

This means instead of shifting P one character to the right and check every possible alignments like in naive exact matching, P is often shifted many steps, skipping many alignments that we know for sure will not be a match.

The two tricks are **the bad character rule** and **the good suffix rule**.

## The bad character rule:

To get a sense of this rule, imagine the following case in naive exact matching.

<pre>
P: word
T: Hello world word!
---------<span style="color:green">wor</span><span style="color:red">d</span>-------
---0123456----------
</pre>

As P slides to offset 6, the navie exact matching checks if P "word" matches with the substring "worl" at the current alignment, and found a mismatch of "l" in T and "d" in P.

Since the character "l" does not exist in P at all, instead of sliding P one offset to the right and check the next substring of T, we can slide the entirely of P pass this character - since any cases where "l" overlapping with P will NOT be a match.

<pre>
P: word
T: Hello world word!
---------<span style="color:green">wor</span><span style="color:red">d</span>-------
-------------word---
---01234567890------
</pre>

Instead of sliding the pattern "word" from offset 6 to offset 7, we now slide it directly to offset 10, skipping 3 steps/alignments (7, 8, 9) in between.

**The bad character rule** states that upon finding a mismatch, skip alignments (sliding P forward, to the right) until (a) this mismatch becomes a match, or the entirely of P slides past the mismatched character.

It makes sense, since for a match to happen, either this particular character needs to be matched (when P is overlapping with the character), or P moves pass it entirely (no longer overlapping with the character).

Note that Boyer-Moore checks matching from *right to left*, so in the previous example, it would first check "l" in T and "d" in P, which is a mismatch, and the bad character rule would slide P entirely pass this "bad character".

## The good suffix rule:

The good suffix rule makes more sense with a case where P is longer.

<pre>
T: CGTGC<span style="color:red">C</span><span style="color:green">TAC</span>TAACTTACTTACTTACGCGAA
P: CTTAC<span style="color:red">T</span><span style="color:green">TAC</span>
</pre>

In the case above, we are checking P for a match at offset 0 from right to left, and found a mismatch of "C" in the text and "T" in the pattern.
Because Boyer Moore checks from *right to left*, when found a mismatched character, the substring after that character will be matched - the "good suffix", in our case it's "TAC".
As we slide P forward to find an alignment that would result in a match, this "good suffix" will need to stay matched somewhere in P, for our case, P will be skipped forward until another "TAC" matches this good suffix.

<pre>
T: CGTGC<span style="color:red">C</span><span style="color:green">TAC</span>TAACTTACTTACTTACGCGAA
P: CT<u>TAC</u><span style="color:red">T</span><span style="color:green">TAC</span>
P:     CT<span style="color:green; text-decoration:underline">TAC</span>TTAC
</pre>

Here, instead of shifting P one offset to the right for the next alignment, we shift 4 offsets to make the good suffix "TAC" stay matched, skipping 3 alignments in the process.

 **The good suffix rule** is that upon finding a mismatched character, the "good suffix" (the substring from right after the character to the end of P) will be used to skip alignments - skip until (a) another part of P matches the good suffix, or (b) P move past the good suffix.

## Look up table and time complexity

The number of skips we can perform for the bad character and good suffix rules is *entirely depended on the pattern P*.

In practice, P is preprocessed to build a look up table for both rules, so upon a mismatch, we will know now many steps to skip in constant time by checking the table.  

If the length of pattern P is m, and length of text T is n,
Naive exact match has a time complexity of O(mn).

For Boyer-Moore, the worst-case time complexity is also O(mn), when there are many mismatches.
However, in practice, it'll often skip many alignments and be faster than the naive exact match, the best-case time complexity for Boyer-Moore is O(m/n).

## Implementing Boyer-Moore matching in Python

An example of implementing Boyer-Moore matching algorithm in Python, and use it to match short DNA reads back to a genome can be found in this [Google Colab](https://colab.research.google.com/github/gr-grey/genomic-courses/blob/main/boyer_moore_matching.ipynb) file.

Very briefly, we have a class "BoyerMoore" that preprocesses a pattern P, building look up tables for the bad character rule and the good suffix rule. 

Then similar to exact match, we slide/align P at different offsets and compare P to substrings of T from *right to left*, upon finding a mismatch, we check the skips we can perform according to the bad character rule and the good suffix rule, and uses the bigger skip of the two.

```python
# Preprocessing part is not shown here
# The bm_preproc.py file contains a BoyerMoore class
# that builds the bad character rule look up table
# and the good suffix rule look up table
from bm_preproc import *

def boyer_moore(p, p_bm, t):
    i = 0 # current alignemnt/ offset of checking
    occurrences = []
    while i < len(t) - len(p) + 1: # the last checking point before p goes out of bounds
        shift = 1
        mismatched = False
        for j in range(len(p) - 1, -1, -1): # j goes from the index of last char in p, then second last, all the way to 0
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

    return occurrences
```

The original paper:

Boyer, RS and Moore, JS. "A fast string searching algorithm." Communications of the ACM 20.10 (1977): 762-772
