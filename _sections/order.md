Here is a basic definition that we will need in order to describe our next cryptosystems. 

<div class="element">
<span class="label">Definition</span>
Fix a positive integer $n$. If $a$ is an integer with $\gcd(a, n) = 1$, the *order* of $a$ mod $n$, denoted $\mathrm{ord}_n(a)$, is the smallest positive integer $k$ such that $a^k \equiv 1 \pmod{n}$. 
</div>

For example, suppose $n = 7$ and $a = 2$. We then compute: 
$$ \begin{aligned}
2^2 &= 4 \pmod{7} \\
2^3 &= 8 \equiv 1 \pmod{7}
\end{aligned} $$
Thus 3 is the smallest positive exponent such that raising 2 to that power gives us something congruent to 1 mod 7, which means that $\mathrm{ord}_7(2) = 3$. 

### Order Lemmas

Note that $\phi(7) = 6$, and $\mathrm{ord}_7(2) = 3$ happens to be a divisor of 6. This is no coincidence! 

<div class="element">
<span class="label">First Lemma About Orders</span>
Fix a positive integer $n$ and an integer $a$ with $\gcd(a, n) = 1$. If $m$ is an integer with $a^m \equiv 1 \pmod{n}$, then $\mathrm{ord}_n(a)$ divides $m$. In particular, $\mathrm{ord}_n(a)$ divides $\phi(n)$. 
</div>

*Proof*. Let $k = \mathrm{ord}_n(a)$. By Euclid's Division Lemma, we can write $m = kq + r$ where $0 \leq r < k$. Then
$$ 1 \equiv a^m = a^{kq + r} = (a^k)^q a^r \equiv 1^q \cdot a^r = a^r \pmod{n}. $$
If $r$ was nonzero, it would be a positive exponent strictly smaller than $k$ which gives a number congruent to 1 mod $n$, which contradicts the definition of $\mathrm{ord}(a)$. Thus it must be that $r = 0$, ie, that $k$ divides $m$. 

By Euler's theorem, we know that $a^{\phi(n)} \equiv 1 \pmod{n}$. By what we have just shown, we therefore know that $\mathrm{ord}_n(a)$ divides $\phi(n)$. <span style="float: right;">$\Box$</span>

The First Order Lemma makes it a bit easier to compute the order of an element. Suppose, for example, that we are interested in $n = 7$ and $a = 3$. The lemma guarantees that $\mathrm{ord}_7(3)$ must be a divisor of $\phi(7) = 6$, so it can only be 1, 2, 3, or 6. We check:
$$ \begin{aligned}
3^1 &\not\equiv 1 \pmod{7} \\
3^2 &= 9 \equiv 2 \not\equiv 1 \pmod{7} \\
3^3 &= 27 \equiv 6 \not\equiv 1 \pmod{7} 
\end{aligned} $$
Thus $\mathrm{ord}_7(3)$ cannot be 1, 2, or 3, so we know that it must be 6. 

<div class="element">
<span class="label">Exercise</span>
Calculate the following orders.

a. $\mathrm{ord}_5(2)$
b. $\mathrm{ord}_9(4)$
c. $\mathrm{ord}_{10}(3)$
d. $\mathrm{ord}_{11}(7)$
e. $\mathrm{ord}_{13}(1)$
</div>

<div class="element">
<span class="label">Second Lemma About Orders</span>
Fix a positive integer $n$ and an integer $a$ with $\gcd(a, n) = 1$ and let $k = \mathrm{ord}_n(a)$. Then $a^i \equiv a^j \pmod{n}$ if and only if $i \equiv j \pmod{k}$. In particular, the numbers $a^0, a^1, a^2, a^3, \dotsc, a^{k-1}$ are all incongruent mod $n$. 
</div>

*Proof*. First suppose that $i \equiv j \pmod{k}$. Then there exists an integer $q$ such that $i = kq + j$, so
$$ a^i = a^{kq + j} = (a^k)^q a^j \equiv 1^q a^j = a^j \pmod{n} $$
since $a^k \equiv 1 \pmod{n}$. Conversely, suppose $a^i \equiv a^j \pmod{n}$ and assume without loss of generality that $i \geq j$. By multiplying both sides by $a^{-1}$ mod $n$ a total of $j$ times, we find that $a^{i-j} \equiv 1 \pmod{n}$, so the First Lemma About Orders tells us that $k$ divides $i - j$. In other words, $i \equiv j \pmod{k}$. 

For the final assertion, note that $0, 1, 2, \dotsc, k-1$ are all incongruent mod $k$, so $a^0, a^1, a^2, \dotsc, a^{k-1}$ must all be incongruent mod $n$. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">SageCell</span>
Sage can find orders mod $n$. The following computes $\mathrm{ord}_5(2)$, for example. 
<div class="sage">
<script type="text/x-sage">
Mod(2, 5).multiplicative_order()
</script>
</div>
</div>

<div class="element">
<span class="label">Proof Exercise</span>

a. If $m$ is an integer and $p$ is an odd prime divisor of $m^4 + 1$, show that $p \equiv 1 \pmod{8}$. *Hint*. First show carefully that $m$ has order 8 mod $p$. 
b. Show that there are infinitely many primes that are congruent to 1 mod 8. *Hint*. Suppose for a contradiction that there are only finitely many such primes $p_1, \dotsc, p_r$ and then apply the part (a) to the integer $n = (2p_1 \cdots p_r)^4 + 1$. 
</div>

### Primitive Roots and Discrete Logarithms

The First Lemma About Orders tells us that $\phi(n)$ is the largest possible order mod $n$ that any integer could have, since the order must always be a divisor of $\phi(n)$. The situation when this maximum is achived gets a special name: 

<div class="element">
<span class="label">Definition</span>
Fix an integer $n \geq 2$. An integer $g$ with $\gcd(g, n) = 1$ and $\mathrm{ord}_n(g) = \phi(n)$ is called a *primitive root* mod $n$. 
</div>

For example, we saw above that $\mathrm{ord}_7(3) = 6 = \phi(7)$, so 3 is a primitive root mod 7. 

The Second Lemma About Orders tells us that $3^0, 3^1, 3^2, 3^3, 3^4, 3^5$ are all incongruent mod 7, but there are only 6 nonzero congruence classes mod 7, so the fact that all of the nonzero congruence classes mod 7 must be represented among the integers $3^0, 3^1, 3^2, 3^3, 3^4, 3^5$. Let's check this explicitly: 
$$ \begin{aligned}
3^0 &\equiv 1 \pmod{7} \\
3^1 &\equiv 3 \pmod{7} \\
3^2 &\equiv 2 \pmod{7} \\
3^3 &\equiv 6 \pmod{7} \\
3^4 &\equiv 4 \pmod{7} \\
3^5 &\equiv 5 \pmod{7}
\end{aligned} $$
All of the nonzero remainders mod 7 appear in this list. This generalizes: 

<div class="element">
<span class="label">Existence of Discrete Logarithms Lemma</span>
Fix an integer $n \geq 2$ and suppose $g$ is a primitive root mod $n$. If $\gcd(a, n) = 1$, then there exists a unique integer $k$ such that $0 \leq k \leq \phi(n)$ and $g^k \equiv a \pmod{n}$. This integer $k$ is called the *discrete log base $g$ of $a$ mod $n$* and is denoted $\log_g(a \bmod{n})$. 
</div>

Our calculations above show that the discrete log base 3 of 6 mod 7 is 3, because $3^3 \equiv 6 \pmod{7}$.  

<div class="element">
<span class="label">Exercise</span>
For each of the following, determine whether or not the proposed value of $g$ is actually a primitive root mod $n$. 

a. $n = 11, g = 2$
b. $n = 11, g = 3$
c. $n = 11, g = 4$
</div>

<div class="element">
<span class="label">Exercise</span>
For each of the following values of $n$, find *all* of the primitive roots mod $n$. 

a. $n = 5$
b. $n = 7$
c. $n = 11$
</div>

<div class="element">
<span class="label">Exercise</span>
For each of the following, find the discrete log base $g$ of $a$ mod $n$. 

a. $n = 7, g = 3, a = 5$
b. $n = 5, g = 2, a = 4$
c. $n = 11, g = 2, a = 3$
</div>

<div class="element">
<span class="label">Exercise</span>
Explain in your own words why the Second Lemma About Orders immediately implies the Existence of Discrete Logarithms Lemma. 
</div>

<div class="element">
<span class="label">SageCell</span>
Sage can find discrete logarithms. It is of course much faster than trying to find discrete logarithms by hand, but it is important to note for what's coming next that finding discrete logarithms seems to be an inherently hard problem: there is no fast way of doing it, and Sage will struggle as soon as the numbers get moderately large, just as Sage struggles with finding prime factorizations for moderately large numbers. Here is the syntax for finding $\log_3(6 \bmod{7})$. 
<div class="sage">
<script type="text/x-sage">
discrete_log(Mod(6,7), Mod(3,7), operation="*")
</script>
</div>
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose $p \geq 3$ is prime and $g$ is a primitive root mod $p$. Explain why 
$$\log_g(-1 \bmod{p}) = \frac{p-1}{2}. $$ 
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose $p \geq 3$ is prime and $g$ is a primitive root mod $p$. Explain why 
$$ 1 + g + g^2 + \cdots + g^{p-1} \equiv 0 \pmod{p}. $$
</div>

### Existence of Primitive Roots

We haven't yet shown that primitive roots always exist, and in fact, it is not true that primitive roots always exist. Here is the statement: 

<div class="element">
<span class="label">Primitive Root Theorem</span>
Fix an integer $n \geq 2$. Then there exists a primitive root mod $n$ if and only if $n = 2, 4, p^k$, or $2p^k$ for an odd prime $p$ and a positive integer $k$. In particular, there always exists a primitive root mod a prime. 
</div>

Proving this theorem would require a somewhat lengthy tangent, so I will omit it.

<div class="element">
<span class="label">Exercise</span>
Verify explicitly (ie, without using the Primitive Root Theorem) that there is no primitive root mod 8. In other words, verify that none of the integers 1, 3, 5, or 7 have order $\phi(8) = 4$ mod 8. 
</div>

<div class="element">
<span class="label">Exercise</span>
Use the Primitive Root Theorem to find the 5 smallest integers $n \geq 2$ such that there does *not* exist a primitive root mod $n$.
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Do one of the following: 

a. (Harder) Prove the Primitive Root Theorem yourself.
b. (Easier) Read through a proof of the Primitive Root Theorem. (For example, see section 2.5 in Stein's "Elementary Number Theory," or sections 8.2--3 in Burton's "Elementary Number Theory," or find another reference you like!) Then summarize the proof strategy that you read about in your own words. 
</div>

<div class="element">
<span class="label">SageCell</span>
The Sage function `primitive_root` returns the smallest primitive root for the input argument. For example: 
<div class="sage">
<script type="text/x-sage">
primitive_root(11)
</script>
</div>
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Find the proportion of primes $p < 1000$ such that 2 is a primitive root mod $p$. 
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Find a prime $p$ such that the smallest primitive root mod $p$ is $g = 37$. 
</div>


