At this point, we're ready to think about cryptosystems in some generality. We'll introduce some abstractions that encompass all of the types of ciphers we've seen so far, but the point is not the generality; the goal in this section is to see what it means to say that "the one-time pad achieves perfect secrecy," and to prove this. 

This section may be a little challenging, especially if you haven't done much with proofs before; focus on understanding the definitions and theorem statements at first, and you can return the proofs later if you like.

Let's first introduce the general framework. From Eve's perspective, it makes sense to consider Alice's choice of a plaintext message as a random variable $M$ whose set of values $\mathscr{M}$ represents all possible plaintext messages. Note that, by saying that $M$ is a random variable, we're introducing probabilities associated to every possible plaintext message, and we assume that Eve knows this probability distribution. For example, $\mathscr{M}$ might be the set of all strings in the letters `A`--`Z`, and we might deem the plaintext `QUIZ` to be less probable than the plaintext `THEN` on the basis that the former uses much more infrequent letters than the latter. Alternatively, we could assign probabilities based on bigram frequencies instead of single-letter frequencies. It doesn't matter how we assign probabilities; the point is just that any plaintext message has a probability assigned to it. 

We can model known-plaintext attacks in at least two ways. Suppose, for example, that Eve knows that the plaintext must contain the word `LONDON`. The first way to model a known-plaintext attack is to assign a probability of 0 to every plaintext message that *doesn't* contain the `LONDON` as a substring. The second way is to simply eliminate all strings that don't contain `LONDON` from our set of values $\mathscr{M}$. It is convenient to use the latter method of modeling known-plaintext attacks, since it allows us to assume that
$$ P[M = m] \neq 0 $$
for all $m \in \mathscr{M}$, and so we will assume this from here on out. 

This brings us to a the following definition of a cryptosystem. 

<div class="element">
<span class="label">Definition</span>
A *cryptosystem* (on $M$) consists of the following data: 

* A random variable $K$ whose set of values $\mathscr{K}$ represents the set of all possible keys.[^same]
* A set $\mathscr{C}$ of all possible ciphertexts. 
* An encryption function $E : \mathscr{K} \times \mathscr{M} \to \mathscr{C}$, which means that $E$ takes as input a key $k \in \mathscr{K}$ and a plaintext message $m \in \mathscr{M}$ and outputs a corresponding ciphertext $E(k, m)$. 
* A decryption function $D : \mathscr{K} \times \mathscr{C} \to \mathscr{M}$, which means that $D$ takes as input a key $k \in \mathscr{K}$ and a ciphertext $c \in \mathscr{C}$ and outputs a corresponding plaintext message $D(k, c)$. 

We require that 
$$ D(k, E(k, m)) = m $$
for any key $k \in \mathscr{K}$ and any plaintext message $m \in \mathscr{M}$, ie, that a plaintext can be recovered from its encryption. Once we've specified the above data, we also define the random variable $C = E(K, M)$ to model an observation of the ciphertext. 
</div>

[^same]: If you're thinking of a random variable as a function, the probability space on which $K$ is defined should be the same as the one on which $M$ is defined. In other words, it is not enough to describe the distribution that $K$ induces on the key space $\mathscr{K}$ --- this is merely a marginal distribution, and one needs to specify the joint distribution of $K$ and $M$ (or equivalently, if the distribution of $M$ is treated as a given, it is sufficient to specify the conditional distribution of $K$). We do not assume at the outset that $K$ and $M$ are independent, but under this additional assumption, it would be sufficient to specify the marginal distribution of $K$. 

For example, consider the Caesar cipher. The sets $\mathscr{M}$ and $\mathscr{C}$ are both equal to the set of all possible strings in the capital letters `A` through `Z`, and the key space $\mathscr{K}$ is the set of all possible shifts, ie, $\mathscr{K} = \{0, 1, \dotsc, 25\}$. Given $k \in \mathscr{K}$ and $m \in \mathscr{M}$, the string $E(k, m)$ is the result of applying the Caesar cipher with shift $k$ to $m$, ie, by adding $k$ mod 26 to the numbers 0--25 corresponding to each letter `A`--`Z` in $m$. To fully specify the cryptosystem, we should also assign probabilities to each of the keys; for example, we might assume that $K$ is uniform. 

This is not necessarily the only way to fit the Caesar cipher into the above framework. For example, if you like, you can fix an upper bound on the length of the strings involved so that $\mathscr{M}$ and $\mathscr{C}$ are finite. You could also decide that the key space is just $\{1, \dotsc, 25\}$, if you don't want to consider a shift of 0 to be a valid key. You could also choose a non-uniform distribution for the random variable $K$. 

<div class="element">
<span class="label">Exercise</span>
Use this definition to model a Polybius square cipher where the key box has our usual row and column labels `ADFGVX`. What is $\mathscr{M}$ in this setting? What is $\mathscr{C}$? What is $\mathscr{K}$, and how many elements does it have?
</div>

<div class="element">
<span class="label">Exercise</span>
How would you use this definition to model rectangular transposition? More specifically, what would you take $\mathscr{K}$ to be? 

*Note*. There isn't a unique "right" answer here! The answer you might write down if you've seen group theory before may be different than the answer you might write down if you haven't seen any group theory. 
</div>

<div class="element">
<span class="label">Proof Exercise</span>
For any cryptosystem, show that the number of ciphertexts must be at least as large as that of the number of plaintext messages. 

*Hint*. The question is about comparing the cardinalities of the sets $\mathscr{M}$ and $\mathscr{C}$. To do this, fix some $k_0 \in \mathscr{K}$ and consider the function $E(k_0, -) : \mathscr{M} \to \mathscr{C}$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Construct an example to show that it is possible to have strictly more ciphertexts than plaintext messages, even when every ciphertext is in fact in the range of the encryption function. 
</div>

Here is the central definition of this section: 

<div class="element">
<span class="label">Definition</span>
A cryptosystem *achieves perfect secrecy* if $M$ and $C$ are independent random variables. 
</div>

In other words, for any plaintext message $m$ and any encrypted message $c$, we should have
$$ P[M = m \mid C = c] = P[M = m]. $$
Heuristically, this means that observing $C = c$ provides Eve with no additional information whatsoever about $M$ --- the best guesses that she might have made about the plaintext message before she intercepted the ciphertext do not change even after she intercepts the ciphertext!

The attacks on cryptosystems we've seen so far exploit the fact that those cryptosystems *fail* to achieve perfect secrecy. When we conduct frequency analysis on simple substitution, for example, we are using the fact that certain plaintexts become more likely after observing a given ciphertext: eg, the plaintexts in which the letter `E` appears in the same positions in which the most frequent letter of the ciphertext appears. 

How do we achieve perfect secrecy? Well, the first observation to make is that achieving perfect secrecy requires having *a lot* of keys. More precisely, the statement is the following: 

<div class="element">
<span class="label">Lemma</span>
Suppose a cryptosystem achieves perfect secrecy. Then the number of keys must be at least as large as the number of "possible" ciphertexts (ie, ciphertexts $c \in \mathscr{C}$ such that $P[C = c] \neq 0$). 
</div>

*Proof*. Fix $m_0 \in \mathscr{M}$ and recall our assumption that $P[M = m_0] \neq 0$. Suppose $c \in \mathscr{C}$ is a "possible" ciphertext. We then have 
$$ P[M = m_0 \mid C = c] = P[M = m_0] \neq 0, $$
so there must exist a key $k(c) \in \mathscr{K}$ such that $E(k(c), m_0) = c$. If $c' \in \mathscr{C}$ is another "possible" ciphertext and $k(c) = k(c')$, then  
$$ c = E(k(c), m_0) = E(k(c'), m_0) = c' $$
so $c \mapsto k(c)$ defines an injective function from possible ciphertexts into $\mathscr{K}$. Thus the number of keys must be at least as large as the number of possible ciphertexts. <span style="float: right;">$\Box$</span>

Having observed this, here is a result that gives us a way to achieve perfect secrecy: 

<div class="element">
<span class="label">Perfect Secrecy Theorem</span>
A cryptosystem achieves perfect secrecy if it satisfies all of the following conditions: 
 
* $K$ is uniform. 
* $K$ and $M$ are independent. 
* For every plaintext message $m \in \mathscr{M}$ and every ciphertext $c \in \mathscr{C}$, there exists a unqiue $k \in \mathscr{K}$ such that $E(k, m) = c$. 
</div>

*Proof*. Fix a ciphertext $c_0 \in \mathscr{C}$ and observe that 
$$ \begin{aligned}
P[C = c_0] &= P[E(K, M) = c_0] \\
&= \sum_{\substack{k \in \mathscr{K}, m \in \mathscr{M}, \\ E(k, m) = c_0}} P[K = k \text{ and } M = m] \\
&= \sum_{\substack{k \in \mathscr{K}, m \in \mathscr{M}, \\ E(k, m) = c_0}} P[K = k] P[M = m], 
\end{aligned}$$
where we use independence of $K$ and $M$ for the last equality. For any plaintext message $m \in \mathscr{M}$, we know by assumption that there exists a unique key $k_0(m)$ such that $E(k_0(m), m) = c_0$. Thus the above sum can be re-written
$$ P[C = c_0] = \sum_{m \in \mathscr{M}} P[M = m] P[K = k_0(m)]. $$
Since $K$ is uniform, it has finitely many values, and if $N$ is the number of values, we have $P[K = k_0(m)] = 1/N$ for all $m \in \mathscr{M}$, so 
$$ P[C = c_0] = \frac{1}{N} \sum_{m \in \mathscr{M}} P[M = m] = \frac{1}{N} $$
since the events $M = m$ as $m \in \mathscr{M}$ varies form a partition of the probability space. 

Now fix a plaintext $m_0 \in \mathscr{M}$. Observe that 
$$ \begin{aligned} 
P[M = m_0 \text{ and } C = c_0] &= \sum_{k \in \mathscr{K}} P[K = k \text{ and } M = m_0 \text{ and } C = c_0] \\
&= \sum_{k \in \mathscr{K}} P[K = k \text{ and } M = m_0 \text{ and } E(k, m_0) = c_0]. 
\end{aligned} $$
There exists a unique key $k_0 \in \mathscr{K}$ such that $E(k_0, m_0) = c_0$, so we have
$$ P[K = k \text{ and } M = m_0 \text{ and } E(k, m_0) = c_0] = 0 $$ 
unless $k = k_0$. In other words, the above summation collapses to just a single term and we have 
$$ \begin{aligned} 
P[M = m_0 \text{ and } C = c_0] 
&= P[K = k_0 \text{ and } M = m_0 \text{ and } E(k, m_0) = c_0] \\
&= P[K = k_0 \text{ and } M = m_0] \\
&= P[K = k_0] P[M = m_0] \\
&= \frac{1}{N} \cdot P[M = m_0] \\
&= P[M = m_0]P[C = c_0]
\end{aligned} $$
where we have used independence of $K$ and $M$ for the third step, and the fact that $P[C = c_0] = 1/N$ for the final step. <span style="float: right;">$\Box$</span>

Our next order of business is to show that "the one-time pad achieves perfect secrecy." Remember that the one-time pad is a special case of the Vignère cipher, so let's first think about how we can formally model the Vignère cipher using the framework we've introduced here. Fix a period $p$ and let $\mathscr{K}$ be the set of all sequences $(a_1, \dotsc, a_p)$ where $a_1, \dotsc, a_p$ are all numbers 0--25. Also, let's fix a message length $r$ and let $\mathscr{M}$ and $\mathscr{C}$ be the set of all strings in the letters `A`--`Z` of length $r$. Note that, if Eve intercepts a ciphertext of length $r$ that she knows to be encrypted using a Vignère cipher, she also knows the plaintext must have had length $r$, so she can assign probability 0 to all messages of other lengths; in other words, it is reasonable to eliminate all messages of other lengths from our message spaces. 

We then have the Vignère encryption and decryption functions $E : \mathscr{K} \times \mathscr{M} \to \mathscr{C}$ and $D : \mathscr{K} \times \mathscr{C} \to \mathscr{M}$. Applying Vignère encryption to a message of length $r$ will always just use the first $\min\{r, p\}$ of the $p$ numbers in our key sequence, so might as well replace $p$ with $\min\{r, p\}$ in order to assume that $p \leq r$. 

<div class="element">
<span class="label">Lemma</span>
Using the notation we've just introduced, we have $p = r$ if and only if, for every plaintext message $m \in \mathscr{M}$ and every ciphertext $c \in \mathscr{C}$, there exists a unqiue $k \in \mathscr{K}$ such that $E(k, m) = c$.
</div>

*Proof*. Let's formally prove one direction: assume that for any $m \in \mathscr{M}$ and $c \in \mathscr{C}$, there is a unique $k \in \mathscr{K}$ such that $E(k, m) = c$. If we fix some message $m_0 \in \mathscr{M}$, our assertion is that the function $E(-, m_0) : \mathscr{K} \to \mathscr{C}$ is bijective, so $\mathscr{K}$ and $\mathscr{C}$ must have the same number of elements. But $\mathscr{K}$ has $26^p$ elements and $\mathscr{C}$ has $26^r$ elements, and $26^p = 26^r$ implies $p = r$. 

For the converse, let's sketch the idea of the proof on the context of an example. I will leave it to you to generalize this sketch if you are interested. Let's suppose that $p = r = 3$. We then want to show that, for any $m \in \mathscr{M}$ and $c \in \mathscr{C}$, there is a unique $k \in \mathscr{K}$ such that $E(k, m) = c$. Suppose for example that $m$ is `ZEE` and that $c$ is `TOE`. If we convert these messages to their corresponding number sequences, we have `ZEE` corresponding to $(25, 4, 4)$ and `TOE` corresponding to $(19, 14, 4)$. Then consider
$$ k = (25, 4, 4) - (19, 14, 4) \bmod{26} = (6, -10, 0) \bmod{26} = (6, 16, 0). $$
This is the only key sequence with the property that encrypting `ZEE` with this key results in `TOE`! <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Proof Exercise</span>
Formalize the sketchy part of the argument for the above lemma. 
</div>

Let's recall some of the assumptions we made when we were discussing the one-time pad when we first introduced it: 

* We asked that the key sequence be "totally random." We can now formalize this by saying that we want our random variable $K$ be uniform. 
* We also asked that the key sequence be "unrelated to the plaintext." We can now formalize this by saying that $K$ and $M$ be independent random variables. 
* We asked that our key sequence be at least as long as the plaintext. In the notation we just introduced, this is the requirement that $p \geq r$. But we already have assumed $p \leq r$, so in fact this is equivalent to saying $p = r$, and in the lemma above, we saw that this is equivalent to saying that, for any $m \in \mathscr{M}$ and $c \in \mathscr{C}$, there exists a unique $k \in \mathscr{K}$ such that $E(k, m) = c$. 

In other words, these three assumptions we made coincide exactly with the conditions that appear in the Perfect Secrecy Theorem! We have now proved what we want to prove: 

<div class="element">
<span class="label">Corollary</span>
The one-time pad achieves perfect secrecy.
</div>

<div class="element">
<span class="label">Harder Exercise</span>
If you look back to when we described the one-time pad, you'll notice that, besides the three assumptions we mentioned above, we also had a fourth assumption: that the key never be reused. Suppose that Eve intercepts two ciphertexts $c_0$ and $c_1$ of length $r$ and that she knows that both were encrypted using a Vignère cipher with the *same key* of length $r$. Describe a strategy that Eve might use to extract information about the corresponding plaintexts. 
</div>

<div class="element">
<span class="label">Exercise</span>
Our definition of "cryptosystem" does not include the assumption that $K$ and $M$ are independent. This is slightly nonstandard, but it was also intentional. Read about the [autokey cipher](https://en.wikipedia.org/wiki/Autokey_cipher), and then explain how to formally model the autokey cipher as a "cryptosystem" in two different ways: one in which $K$ and $M$ *aren't* independent, and one in which $K$ and $M$ *are* independent. 
</div>

<div class="element">
<span class="label">Harder Exercise</span>
Suppose a cryptosystem achieves perfect secrecy. 

a. Is it true that $K$ and $M$ must be independent? 
a. Is it true that $K$ and $C$ must be independent? 
</div>
