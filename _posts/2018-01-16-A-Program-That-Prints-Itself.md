---
title: A Program That Prints Itself
tags: [IT, Computability Theory]
redirect_from:
    - /blog/a-program-that-prints-itself/
    - /2018/01/A-Program-Printing-Itself
---

Is it possible to find a program that prints itself?

This is a common thought that would occur to everyone when we begin to learn programming. You can find lots of sample code written in C or some language else on Google. But here we will not confine this problem to a certain programming language and discuss how the existence of this program is ensured.

## Turing machine

First, it is necessary to formalize programs with a general computational model. Consider a machine with following features.

1. It consists of a tape of infinite length, a two-way read-write head and a state register
2. An alphabet of symbols, a finite set of states and a set of state transition rules are predefined
3. It halts when the state in the register is either accepting or rejecting

This is a basic deterministic single-tape Turing machine, the most widely used computational model ever. It is equivalent to many variants and other computational models such as
- multi-tape Turing machine
- nondeterministic Turing machine[^ntm_def]
- two-stack PDA
- lambda calculus

Now the problem is abstracted as finding a TM that prints the description of itself.

## [Recursion theorem](https://en.wikipedia.org/wiki/Kleene%27s_recursion_theorem)

We use $$ \langle T \rangle $$ to denote the description of a TM $$ T $$. Suppose there is a single-tape TM $$ P_w $$ that reads any input and output the string $$ w $$. $$ \langle P_w \rangle $$ denotes the description above. And then define another TM $$ Q $$:

1. Read string $$ w $$ as input
2. Print $$ \langle P_w \rangle $$ on the tape

Obviously $$ Q $$ is a legitimate TM. After introducing the TMs above, come back to the problem and consider whether there exists a TM $$ R $$ that prints the description of itself on the tape.

Construct a TM $$ S $$.

1. Read $$ \langle T \rangle $$ as the description of TM $$ T $$
2. Run $$ Q $$ on $$\langle T \rangle $$ and get $$ P_{\langle T \rangle} $$
3. Print $$ P_{\langle T \rangle} $$ and $$ \langle T \rangle $$

Finally, it is able to construct the TM $$ R $$ that we need.

1. Reads string $$ w $$ as input
2. Run $$ Q $$ on $$ \langle S \rangle $$ and get $$ \langle P_{\langle S \rangle} \rangle $$
3. Print $$ \langle P_{\langle S \rangle} \rangle $$ and $$ \langle S \rangle $$

Here $$ \langle R \rangle = \langle P_{\langle S \rangle}S \rangle $$.

In fact it is a conclusion derived from the famous [recursion theorem](https://en.wikipedia.org/wiki/Kleene%27s_recursion_theorem). It ensures that self-reference is allowed in a TM. Therefore it is possible to construct a TM or program that prints itself.

## Footnotes

[^ntm_def]: It differs from deterministic one by multiple possible states after a state transition.