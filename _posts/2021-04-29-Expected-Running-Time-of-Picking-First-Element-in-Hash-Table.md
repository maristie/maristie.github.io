---
title: "Expected Running Time of Picking First Element in Hash Table"
tags: [IT, Hashing]
---

It is widely known that hash tables support dictionary operations including insertion, search and deletion in constant time on average. Sometimes we want to pick *any* element from the hash table. Without loss of generality, we constrain ourselves by considering the special case of picking the first element in a hash table, since the problem of finding *any* element can always be reduced to the one of finding *first* element, and hence the latter one is no easier and provides an upper bound in time complexity.

## Brief Introduction to Hash Table

Before we delve into the detailed proof, let's walk through the basics of hash tables.

### Motivation

The most direct motivation for such a data structure originates in access to sparsely distributed data in a relatively large domain. For dense data, we allocate an array and store them in a consecutive manner, which might incur a reasonable waste of space. But if the actual amount of data in an application is far less than its domain (possibly an infinite set), there is no point in preallocating space for all possible values.

For example, suppose that we have to maintain a dynamic set of $n$ elements within a domain of $0, 1, \ldots, U - 1$. Suppose furthur that the elements are uniformly distributed. If $n = 10U$ (allowing duplicates), it's reasonable to support dictionary operations by a straightforward array. When $n = \frac{U}{10}$, it's generally a bad idea to leave large parts of the array unused. To make things worse, if we want to store a limited number of strings from Latin alphabet without a strict bound on the length, it is impossible to allocate a directly-indexed array to hold all possible values.

### Intuition

The major difficulty is that the total number of elements in a dictionary may not match the cardinality of the domain, while we hope to maintain the random access property of an array. Given that the space for storage should be in linear in the size of the dictionary at best, an intuitive try is to map the values in domain to $0, 1, \ldots, n - 1$, where $n$ denotes the size of dictionary. Each value in the domain is mapped to an element in $0, 1, \ldots, n - 1$, so by nature there would be multiple values mapped to the same element in codomain.

In one extreme case, we can construct a linked list for all data which takes exactly $\Theta (n)$ space. The disadvantage is that search and deletion operations can take $O(n)$ time in the worst case, which is undesirable. Ideally, all the data should form a one-to-one mapping to the index of an array with length $n$, providing $O(1)$ mapping computation, $O(1)$ random access and dictionary operations without any redundance in space.

### Issues

We usually call such a mapping process as **hashing**, and the function such that $\forall a \in S, h(a) = k, k \in \{0, 1, \ldots, n - 1\}$ as **hash function**. We have several issues to solve before it can be put into practice.

#### Deal with collision

As noted above, the hash function is many-to-one in essence, therefore it's inevitable to consider such cases that $a \in S, b \in S, a \neq b \land h(a) = h(b) $. For simplicity, we assume that the elements in the dictionary are distinct (i.e., a normal set).

Such elements should be distinguished even if they have identical hash values. A natural approach is to create a linked list for each index in the array, and store the elements which are mapped to the same hash value in the linked list. In this way, we access an element first by its hash value as index in the array, followed by a trivial linear search in the linked list. Insertion, search and deletion operations are analogous to normal linked list.

Another problem that ensues from this approach is that once too many elements fall into the same index, the data structure will degenerate to a trivial linked list. Thanks to the [simple uniform hashing assumption](https://en.wikipedia.org/wiki/SUHA_(computer_science)), we can prove that such a hash table indeed achieves constant dictionary operations in the sense of probabilistic expectation, and it works extremely well in practice.

As an aside, there is another approach called *open addressing* to solve collisions in addition to *chaining* with a linked list. However, this method presents a significant difficulty in deleting elements, for which it is not adopted as widely as chaining. For more explanations and mathematical proofs, see *CLRS 11.2 & 11.4*.

#### Choice of hash function

The very first problem is to choose an appropriate hash function. Recall that the hash function should deterministically hash $\forall a \in S$ to any value in $\{0, 1, \ldots, n - 1\}$. (Or we have no means to access an element again if it's hashed randomly.) We have discussed approaches toward collisions, so in the first place the hash function should cause as less collisions as possible. Besides, computationally expensive hash functions are not desirable.

In principle, the hash function should map $\forall a \in S$ to any value in the function range with equal probability. It is not contradictory with the deterministic mapping requirement, since it is not based on a single element, but the entire domain $S$. And it should hold independent of any preceding sequence of hashing.

Although it is impossible to know the probability distribution of hashing sequence beforehand, there are some hash functions that work quite satisfactorily in practice. One of them is called $division method$, namely $h(a) = a \mod m, m \in \mathbb{N}$. As a thumb of rule, $m$ should be a prime number that is unlikely to depend on the distribution of sequence in domain $S$, and should avoid specific pitfalls near a power of 2.

Another approach is multiplication method. The basic idea is to multiply the key by a fraction, thereafter furthur mapping the decimal part uniformly to $\{0, 1, \ldots, n - 1\}$. It is usually done by multiplication and bit shifting in practice for performance reasons. In general, any method that satisfy the above requirement is all right, but the performance differs under certain distributions. The implementation varies greatly in standard libraries of programming languages and applications, depending on specific use cases. Refer to *CLRS 11.3* for further reading.

#### Dynamic table

One minor issue is to expand or contract the hash table on demand. It is hard to keep the size of hash table array exactly equal to the number of elements in the dictionary efficiently, as memory allocation is by ad hoc paging and thus non-consecutive. For this reason, we must tolerate some degree of unused space. The ratio of $\alpha = \frac {card(S)}n$ is called *load factor*. Intuitively, the higher load factor, the more collisions; on the contrary, the lower load factor, the more wasted space.

This is why we want to keep the load factor varying in a controlled range. *CLRS 17.4* gives an excellent illustration on a more general issue: maintenance of an array-based data structure of dynamic size. In conclusion, the expansion and contraction of such a dynamic table can be accomplished in amortized constant time, thereby causing no change to the asymptotic running time of operations on the data structure. In the case of hash tables, we have to make a trade-off, and the decision depends on the use case.

## Expected Running Time of Picking First Element in Hash Table

Finally, let's come back to the original problem: the expected running time of picking the first element in a hash table. In general, hash table implementations wouldn't maintain a pointer or record on the slot with smallest index that holds a non-empty linked list. Thus we usually access the first element by scan from the head of array linearly, checking each slot until finding one that does hold some element. This element is then returned.

As an example in Java, suppose that we have a non-empty `HashSet s`. We find the first element by calling `s.iterator().next()`, which performs exactly the above steps.

Based on the simple uniform hashing assumption, we claim that the expected running time of picking first element is constant (i.e., $O(1)$). I will give a rigorous proof as follows.

First, let random variable $X$ denote the distance from the head of hash table to the first non-empty slot after randomly and independently inserting $s = |S|$ elements into a hash table of size $n$, and $X_i$ denote the distance from head to the *i*th inserted element. It is easy to see that the running time $T(s, n) = \Theta (X + 1)$, where $1$ is to ensure that the time cost is positive even $X = 0$.

Observe that from the definition of $X$ and $X_i$, we have

$$ \begin{aligned}
Pr\{X \geq k\}
&= Pr\{X_1 \geq k \land X_2 \geq k \ldots \land X_n \geq k\} \\
&= Pr\{X_1 \geq k\} \cdot Pr\{X_2 \geq k\} \cdot \ldots \cdot Pr\{X_n \geq k\} \\
&= (\frac{n - k}n)^s
\end{aligned}
$$

Note that the second line holds by independence of $X_i, i = 0, 1, \ldots, s - 1$.

We want to prove $E[X] = O(1)$. From the definition of expectation, it's easy to prove that

$$
E[X] = Pr\{X \geq 1\} + Pr\{X \geq 2\} + \ldots + Pr\{X \geq n - 1\}
$$

for a discrete distribution in $\{0, 1, \ldots, n - 1\}$. Thus,

$$ \begin{aligned}
E[X]
& = Pr\{X \geq 1\} + Pr\{X \geq 2\} + \ldots + Pr\{X \geq n - 1\} \\
& = \sum_{k = 1}^{n - 1} {(\frac{n - k}n)^s} \\
& = \sum_{k = 1}^{n - 1} {(1 - \frac{k}n)^{\alpha n}}
\end{aligned}
$$

where  $ \alpha = \frac{s}{n}$, the load factor.

It is trivial to prove that the derivative of $f(n) = (1 - \frac kn)^n$ is positive, i.e., $f'(n) > 0, \forall n > k$. Further observe that

$$
\lim_{n \to \infty} f(n) = \frac{1}{e^k}
$$

Thus we have $f(n) < \frac{1}{e^k}, \forall n > k$.

$$ \begin{aligned}
E[X]
& = \sum_{k = 1}^{n - 1} {(1 - \frac{k}n)^{\alpha n}} \\
& < \sum_{k = 1}^{n - 1} {\frac 1{e^{\alpha k}}} \\
& < \sum_{k = 1}^{\infty} {\frac 1{e^{\alpha k}}} \\
& = \frac 1 {e^\alpha - 1}
\end{aligned}
$$

which is a constant as long as $\alpha$ is a positive constant. We thus conclude that $E[X] = O(1)$, which completes our proof.