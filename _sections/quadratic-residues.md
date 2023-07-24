A familiar feature of the real numbers is that some numbers do not have square roots (namely, the negatives). The same thing happens when you work mod an integer. For example, let $n = 5$. We know that integer is congruent to 0, 1, 2, 3, or 4 mod 5. This means that any square must be congruent to $0^2 = 0, 1^2 = 1, 2^2 = 4, 3^2 \equiv 4 \pmod{5}$, or $4^2 \equiv 1 \pmod{5}$. In other words, only 0, 1, or 4 have square roots mod 5 --- and 2 and 3 do not!

<div class="element">
<span class="label">Definition</span>
Fix a positive integer $n$. We say that an integer $a$ is a *quadratic residue mod $n$* if it has a square root mod $n$, ie, if there exists an integer $x$ such that $x^2 \equiv a \pmod{n}$.  
</div>

<div class="element">
<span class="label">Exercise</span>
Find all quadratic residues mod the following integers:

a. $n = 3$
b. $n = 7$
c. $n = 11$
</div>

We'll see below that it will be useful to quickly determine whether an integer $a$ is a quadratic residue mod a prime $p \geq 3$. It turns out that there is a good way to do this! Let us first introduce the following notation:

<div class="element">
<span class="label">Definition</span>
Let $p \geq 3$ be prime. For any integer $a$, define the *Legendre symbol* $(a/p)$ by
$$ \left( \frac{a}{p} \right) = \begin{cases} 0 & \text{if } a \equiv 0 \pmod{p} \\
1 & \text{if $a \not\equiv 0 \pmod{p}$ and $a$ is a quadratic residue mod $p$} \\
-1 & \text{if $a \not\equiv 0 \pmod{p}$ and $a$ is not a quadratic residue mod $p$} \end{cases} $$
</div>

For example, we saw above that 4 is a quadratic residue mod 5, so
$$ \left( \frac{4}{5} \right) = 1 $$
and we saw that 2 is not a quadratic residue mod 5, so
$$ \left( \frac{2}{5} \right) = -1. $$
We can now rephrase our goal: we would like a quick way of computing Legendre symbols. This is provided to us by combining binary exponentiation with the following: 

<div class="element">
<span class="label">Euler's Criterion</span>
Let $p \geq 3$ be prime. For any integer $a$, 
$$ \left( \frac{a}{p} \right) \equiv a^{(p-1)/2} \pmod{p}. $$
</div>

*Proof*. There are three cases to consider: 

(1) $a \equiv 0 \pmod{p}$;
(2) $a \not\equiv 0 \pmod{p}$ and $a$ is a quadratic residue mod $p$; and 
(3) $a \not\equiv 0 \pmod{p}$ and $a$ is *not* a quadratic residue mod $p$. 

In case (1), the statement we're trying to prove is clear since both sides of the congruence are 0 mod $p$. In case (2), there exists an integer $x$ such that $x^2 \equiv a \pmod{p}$. Since $p$ does not divide $a$, it also does not divide $x^2$, so it cannot divide $x$ either by Euclid's Lemma. Thus $\gcd(x, p) = 1$, and Euler's Theorem implies that
$$ a^{(p-1)/2} \equiv (x^2)^{(p-1)/2} = x^{p-1} = x^{\phi(p)} \equiv 1 = \left( \frac{a}{p} \right) \pmod{p}. $$

Now consider case (3). The idea is to calculate $(p-1)!$ mod $p$ in two different ways, by "pairing off" the numbers in two different ways. The first way is to pair them off into pairs $x, y$ where $xy \equiv 1 \pmod{p}$. There are 2 "degenerate" pairs where $x = y$, namely, $x = y = 1$ and $x = y = p-1$ (cf. Lemma 4.4.1), but the remaining numbers $2, \dotsc, p-2$ are all paired off with a different number in the same range and the product of each of those "nondegenerate" pairs is 1, so the Modular Arithmetic Theorem implies that
$$ (p-1)! = 1 \cdot 2 \cdot 3 \cdots (p-3) (p-2) (p-1) \equiv 1 \cdot (p-1) \equiv -1 \pmod{p}. $$
(This fact is sometimes called *Wilson's Theorem*.) On the other hand, we can also pair the numbers off into pairs $x, y$ where $xy \equiv a \pmod{p}$. Since we are in case (3) and $a$ is *not* a quadratic residue mod $p$, we know that there is no solution to this congruence where $x = y$, ie, there are no "degenerate" pairs in this case. There are $(p-1)/2$ pairs, and the product of each pair is $a$, so the Modular Arithmetic Theorem again guarantees that 
$$ (p-1)! = 1 \cdot 2 \cdots (p-2) \cdot (p-1) \equiv a^{(p-1)/2} \equiv{p}. $$
Thus we have
$$ a^{(p-1)/2} \equiv (p-1)! \equiv -1 = \left( \frac{a}{p} \right) \pmod{p} $$
and we are done. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Proof Exercise</span>
Here is a fact we tacitly made use of *twice* in the above proof. Suppose $p$ is prime and $a$ is an integer that is not divisible by $p$. For any integer $1 \leq x \leq p-1$, there is a *unique* integer $1 \leq y \leq p-1$ such that $xy \equiv a \pmod{p}$. 

a. Prove this fact. 
b. Find the two places in the above proof where this fact was used. 
</div>

Euler's Criterion means that we have an efficient algorithm for determining whether something is a quadratic residue: we simply use binary exponentiation to compute $a^{(p-1)/2} \bmod{p}$ and we can read off the answer! For example, suppose we want to know if $a = 37$ is a quadratic residue mod $p = 97$. We have $(p-1)/2 = 96/2 = 48$, so we compute $a^{(p-1)/2} = 37^{48} \bmod{97}$ using binary exponentiation, and we find that it is $96 \equiv -1 \pmod{97}$. Euler's Criterion says that
$$ \left( \frac{37}{97} \right) \equiv 37^{(97-1)/2} = 37^{48} \equiv -1 \pmod{97}, $$
which means that 37 is *not* a quadratic residue mod 97. 

<div class="element">
<span class="label">Exercise</span>
Use Euler's Criterion to determine whether or not the following integers $a$ are quadratic residues mod $p = 19$. 

a. $a = 3$
b. $a = 5$
c. $a = 11$
</div>

<div class="element">
<span class="label">Harder Proof Exercise</span>
Suppose $p \geq 3$ is prime. 

a. Show that there are exactly $(p+1)/2$ quadratic residues mod $p$. 
b. Conclude that, if $p$ is large, then the probability that a randomly chosen integer in $[0, p)$ happens to be a quadratic residue is approximately 50%.
</div>

<div class="element">
<span class="label">SageCell</span>
Sage can compute Legendre symbols using the `kronecker_symbol` function: 
<div class="sage">
<script type="text/x-sage">
kronecker_symbol(33, 97)
</script>
</div>
When the Legendre symbol is not $-1$, Sage can also compute a modular square root: 
<div class="sage">
<script type="text/x-sage">
Mod(33, 97).sqrt()
</script>
</div>
</div>

<div class="element">
<span class="label">Random Exercise</span>
What does Sage output for `Mod(k, n).sqrt()` when $k$ is *not* a quadratic residue mod $n$? If you know what the words "splitting field" and "finite field" mean, can you explain this behavior in terms of these words? 
</div>

There is a lot more that can be said about quadratic residues, though this will be roughly enough for our purposes. If you are interested in reading more, you should look up the [Tonelli-Shanks algorithm](https://en.wikipedia.org/wiki/Tonelli%E2%80%93Shanks_algorithm) for computing square roots mod $p$. You might also read more about [Jacobi symbols](https://en.wikipedia.org/wiki/Jacobi_symbol#Calculating_the_Jacobi_symbol), [Kronecker symbols](https://en.wikipedia.org/wiki/Kronecker_symbol), and [quadratic reciprocity](https://en.wikipedia.org/wiki/Quadratic_reciprocity). 


