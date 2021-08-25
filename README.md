[36372985.zip](https://github.com/yzw19990124/AlgoProblems/files/7048389/36372985.zip)
# AlgoProblems
A simple diff algorithm
In this exercise you will create an application (in Python 3 or Java or C++) implementing and
using the sequence alignment algorithm learned as part of the material on dynamic programming.
This sequence alignment algorithm is described in Section 6.6 of Algorithm Design. It computes
the optimal alignment between two sequences, s1 and s2, when some penalties are given for deletions
and mismatches. Each sequence will be a list of strings. Assume that the penalty for deleting a list
element is 1, and the penalty for mismatch (the distance d(a, b) between two unequal strings a, b)
is ∞ (so mismatch is not allowed). Then our algorithm finds the minimal number of deletion and
insertion operations that transforms list s1 into list s2 (insertion into s1 corresponds to deletion
from s2). The format of the deletion and insertion operations is described in the comment section
of the program applyDiff.py provided, see below.
The line sizes in the example will have a constant upper bound (say 256 characters). Your code
must run in O(n1n2) time, where ni
is the length of si
. We’ve set rough upper bounds on runtime
through the autograder; if your code exceeds these, you will see the limit and your code’s running
time. The autograder will also compute the total penalty (the sum of sizes in the deletion and
insertion operations) and compare it to the optimum—a program that simply deletes the whole list
s1 and inserts the whole list s2 will probably be far from optimal. . .
