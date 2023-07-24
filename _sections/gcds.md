<div class="element">
<span class="label">Definition</span>
The *greatest common divisor* (or *GCD*) of two integers $a$ and $b$ that are not both zero is denoted $\gcd(a, b)$ and is defined to be the largest integer which is both a divisor of $a$ and a divisor of $b$. 
</div>

For example, suppose we're interested in $\gcd(14, 21)$. The factors of 14 are 1, 2, 7, and 14. The factors of 21 are 1, 3, 7, and 21. The largest one that both have in common is 7, so $\gcd(14, 21) = 7$. 

What we just did, while intuitive, is actually not the best way of finding GCDs. Finding the factors of a number is, in general, very difficult; in fact, we'll see later that the difficulty of factoring can be used to create cryptosystems that are secure even by modern standards! But calculating GCDs is not difficult, thanks to the euclidean algorithm, which we will get to shortly. 

<div class="element">
<span class="label">Exercise</span>
Suppose $a$ is a nonzero integer. What is $\gcd(a, 0)$? 

*Note*. Make sure that your answer works for negative values of $a$ also!
</div>

### Euclidean Algorithm {#euclidean}

The euclidean algorithm for computing GCDs relies on the following observation. 

<div class="element">
<span class="label">Lemma</span> Let $n$ be a positive integer and $a \equiv b \pmod{n}$. Then $\gcd(a, n) = \gcd(b, n)$. 
</div>

*Proof*. Define $c = \gcd(a, n)$ and $d = \gcd(b, n)$. Let $k$ be an integer such that $a - b = nk$. Since $c$ is a factor of both $a$ and $n$, it is also a factor of $a - nk = b$. Thus $c$ is a common factor of both $b$ and $n$ as well, so $c \leq d$ by definition of $d$. On the other hand, the same logic shows that $d$ is a common factor of both $a$ and $n$, so $d \leq c$. Thus $d = c$.  <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Corollary</span> Let $n$ be a positive integer and let $r$ be the remainder when an integer $a$ is divided by $n$. Then $\gcd(a, n) = \gcd(r, n)$. 
</div>

<div class="element">
<span class="label">Euclidean Algorithm</span> 
Suppose $a$ and $b$ are two positive integers, and assume without loss of generality that $b \geq a$. To find $\gcd(a, b)$, do the following: 

Divide $b$ by $a$ and let $r$ be the remainder. Then: 

- If $r = 0$, output $a$. 
- Otherwise, replace $b$ with $a$ and $a$ with $r$. Then repeat. 
</div>

Let's work out an example. Say we want to compute $\gcd(115, 35)$. We divide the bigger number by the smaller one and we get

$$ 115 = 3 \cdot 35 + 10. $$

The remainder is nonzero, so we'll divide again, but this time, we'll divide the dividend (35) by the remainder (10). We get 

$$ 35 = 3 \cdot 10 + 5. $$

The remainder is nonzero again, so we divide again: 

$$ 10 = 2 \cdot 5 + 0. $$

This time, the remainder is 0, so we output the dividend: ie, 5. We have just calculated that $\gcd(115, 35) = 5$. We did not have to factor 115 or 35 to do this; all it took was three divisions!

<div class="element">
<span class="label">Exercise</span> Calculate the following GCDs using the euclidean algorithm: 

a. $\gcd(180, 120)$. 
b. $\gcd(180, 81)$. 
c. $\gcd(121, 77)$. 
</div>

This algorithm [dates back](https://en.wikipedia.org/wiki/Euclidean_algorithm#Historical_development) to Euclid's *Elements* (c. 300 BCE), but it's incredibly efficient: it's still used in modern programming libraries to implement GCD calculations! In particular: 

<div class="element">
<span class="label">SageCell</span>
Sage uses the euclidean algorithm to implement GCD calculations. And it's *fast*; you can use it to calculate GCDs of some very large numbers! Try it!
<div class="sage">
<script type="text/x-sage">
gcd(30414093201713378043612608166064768844377641568960512, 391640858566920)
</script>
</div>
</div>

### Bézout's Theorem {#bezout}

Here's a theorem that looks a little strange at first glance, but turns out to be surprisingly useful.

<div class="element">
<span class="label">Bézout's Theorem</span>
Suppose $a$ and $b$ are integers not both 0. Then $\gcd(a, b)$ can be written as an *integer linear combination* of $a$ and $b$, ie, it can be written as $ax + by$ for some integers $x$ and $y$. Integers $x$ and $y$ such that $\gcd(a, b) = ax + by$ are called *Bézout coefficients*.
</div>

 In fact, the euclidean algorithm tells us how to find Bézout coefficients! Let's work this out in an example. Suppose we're interested in $\gcd(115, 35)$ again. We saw above that the following sequence of divisions showed us that $\gcd(115, 35) = 5$. 

$$ \begin{aligned}
115 &= 3 \cdot 35 + 10 \\
35 &= 3 \cdot 10 + 5 \\
10 &= 2 \cdot 5 + 0
\end{aligned} $$

If we rearrange the second equation, it tells us that 
$$ 5 = 35 - 3 \cdot 10. $$
The first equation tells us that $10 = 115 - 3 \cdot 35$, and we can plug this into the equation above to get
$$ 5 = 35 - 3 \cdot (115 - 3 \cdot 35) = (-3) \cdot 115 + 10 \cdot 35. $$
We've just written the GCD of 115 and 35 as an integer linear combination of these two numbers! 

Let's do another longer example. Suppose we're interested in $\gcd(240, 46)$. Here is the sequence of divisions we would perform when running the euclidean algorithm: 

$$ \begin{aligned}
240 &= 5 \cdot 46 + 10 \\
46 &= 4 \cdot 10 + 6 \\
10 &= 1 \cdot 6 + 4 \\
6 &= 1 \cdot 4 + 2 \\
4 &= 2 \cdot 2 + 0
\end{aligned} $$

This tells us that $\gcd(240, 46) = 2$. To compute Bézout coefficients, we work backwards: 

$$ \begin{aligned} 
2 &= 6 - 1 \cdot 4 \\
&= 6 - 1 \cdot (10 - 1 \cdot 6) \\
&= 2 \cdot 6 - 10 \\
&= 2 \cdot (46 - 4 \cdot 10) - 10 \\
&= 2 \cdot 46 - 9 \cdot 10 \\
&= 2 \cdot 46 - 9 \cdot (240 - 5 \cdot 46) \\
&= 47 \cdot 46 - 9 \cdot 240.
\end{aligned} $$

We start by rewriting the division for the last nonzero remainder. We then alternate between substitution for the remainder directly above, and simplifying. 

This process, of "working backwards" through the sequence of divisions we did during the euclidean algorithm, always works: the result will always be the GCD written as a linear combination of the two numbers we started with. This process is called the *extended euclidean algorithm*. 

<div class="element">
<span class="label">Exercise</span> Calculate Bézout coefficients for the following GCDs using the extended euclidean algorithm: 

a. $\gcd(180, 120)$. 
b. $\gcd(180, 81)$. 
c. $\gcd(121, 77)$. 
</div>

It's worth remarking that the extended euclidean algorithm gives us *one* pair of Bézout coefficients, but will be others; Bézout coefficients are not unique!

<div class="element">
<span class="label">Exercise</span> Observe that $\gcd(42, 12) = 6$. Show that the pairs $(-1, 4)$ and $(1, -3)$ are both Bézout coefficients for 42 and 12. 
</div>

<div class="element">
<span class="label">SageCell</span>
Sage can calculate Bézout coefficients using the function `xgcd`. The output is a triple where the first element is the GCD, and the second and third are the Bézout coefficients. Again, it's very fast, even with huge numbers: 
<div class="sage">
<script type="text/x-sage">
xgcd(30414093201713378043612608166064768844377641568960512, 391640858566920)
</script>
</div>
</div>

Here is a statement that might feel intuitive, but requires proof. The proof is a nice application of Bézout's theorem.

<div class="element">
<span class="label">Euclid's Lemma</span>
Suppose $a$ and $b$ are integers and that $d$ is a divisor of $ab$ such that $\gcd(a, d) = 1$. Then $d$ is a divisor of $b$. 
</div>

*Proof*. By Bézout's theorem, there exist $x$ and $y$ such that $1 = \gcd(a, d) = ax + dy$. Multiplying this equation by $b$, we see that
$$ b = abx + bdy. $$
Observe that $d$ divides $bdy$, and it also divides $abx$ since it divides $ab$. Thus $d$ divides the sum $abx + bdy = b$. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Exercise</span>
Explain why Euclid's lemma as stated above implies the following: if $a$ and $b$ are integers, $p$ is prime, and $p$ divides $ab$, then either $p$ divides $a$ or $p$ divides $b$. 
</div>

### Modular Inversion {#modular-inversion}

Before discussing modular inversion, let's take a step back. How do we solve an equation like the following for $z$?

$$ 5z = 7 $$

We divide both sides by $5$, of course! Stated differently, we multiply both sides by $1/5$. And why do we do that? It's because $(1/5) \cdot 5 = 1$, so by multiplying both sides by $1/5$, we are able to cancel out the 5 that appears on the left-hand side of our equation and isolate the variable $z$. 

Modular inversion gives us a way of recreating this process with congruences. Before jumping into the definitions, let's look at an example. Suppose we're interested in solving the congruence 

$$ 5z \equiv 7 \pmod{11}. $$

It doesn't make sense to directly "divide both sides by $5$," because congruences only make sense when both sides of the congruence are integers and $7/5$ is not an integer. But if we can find an *integer* $x$ with the property that $5x \equiv 1 \pmod{11}$, then we could multiply both sides of our congruence by $x$ and eliminate the $5$ on the left-hand side just like we did earlier (using the Modular Arithmetic Theorem!). And in fact there is such an integer $x$ -- consider, for example, $x = 9$. Then $5x = 9 \cdot 5 = 45 \equiv 1 \pmod{11}$, so if we multiply both sides of our congruence by $9$, we get 

$$ z = 1 \cdot z \equiv (5 \cdot 9) z = 9 \cdot (5 z) \equiv 9 \cdot 7 \pmod{11}. $$

Thus 

$$ z \equiv 9 \cdot 7 = 63 \equiv 8 \pmod{11} $$

and we've solved our congruence! The solution is $z \equiv 8 \pmod{11}$. 

In this example, we magicked the number $x = 9$ out of thin air, but there is in fact a systematic way of finding this number which we will get to shortly. Let us first make some relevant definitions and observations. 

<div class="element">
<span class="label">Definition</span>
Fix a positive integer $n$. An integer $a$ is *invertible* mod $n$ (or a *unit* mod $n$) if there exists another integer $x$ such that $ax \equiv 1 \pmod{n}$. The number $x$ is then called an *inverse* of $a$ mod $n$, and in symbols, one writes $a^{-1} \equiv x \pmod{n}$. 
</div>

For example, we saw above that $9 \equiv 5^{-1} \pmod{11}$, because $5 \cdot 9 \equiv 1 \pmod{11}$. 

It is important to note that inverses don't always exist. 

<div class="element">
<span class="label">Exercise</span>
Explain why 2 is not invertible mod 4. 
</div>

The following theorem characterizes exactly when inverses exist, and also tells us how to find inverses! 

<div class="element">
<span class="label">Modular Inversion Theorem</span>
Fix a positive integer $n$ and another integer $a$. Then $a$ is invertible mod $n$ if and only if $\gcd(a, n) = 1$. Moreover, if $\gcd(a, n) = 1$ and $x$ and $y$ are Bézout coefficients for $a$ and $n$, then $x$ is an inverse of $a$ mod $n$. 
</div>

*Proof*. Suppose first that $a$ is invertible mod $n$. Then there exists an integer $x$ such that $ax \equiv 1 \pmod{n}$. This means that $ax - 1$ is a multiple of $n$, so there exists an integer $y$ such that $ax - 1 = ny$, or, in other words, $ax - ny = 1$. Since $a$ and $n$ are both divisible by $\gcd(a, n)$, the left-hand side of the equation $ax - ny = 1$ is divisible by $\gcd(a, n)$, so the right-hand side must be divisible by $\gcd(a, n)$ also. But the only positive factor of 1 is 1 itself, so $\gcd(a, n) = 1$. 

Conversely, suppose $\gcd(a, n) = 1$. By Bézout's theorem, there exist $x$ and $y$ such that $ax + ny = 1$. This means that $ax - 1$ is a multiple of $n$, so $ax \equiv 1 \pmod{n}$. Thus $x$ is an inverse of $a$. <span style="float: right;">$\Box$</span>

In other words, this theorem tells us that the euclidean algorithm is again what we use to figure out if a number is invertible mod $n$, and to figure out what the inverse is. 

For example, suppose we're interested in the inverse of 7 mod 23. We use the euclidean algorithm to compute $\gcd(23, 7)$. 

$$ \begin{aligned}
23 &= 3 \cdot 7 + 2 \\
7 &= 3 \cdot 2 + 1 \\ 
2 &= 2 \cdot 1 + 0 
\end{aligned} $$

This tells us that $\gcd(23, 7) = 1$, so 7 is in fact invertible mod 23. Moreover, working backwards, we see that

$$ \begin{aligned}
1 &= 7 - 3 \cdot 2 \\
&= 7 - 3 \cdot (23 - 3 \cdot 7) \\
&= 10 \cdot 7 - 3 \cdot 23
\end{aligned} $$

so the Modular Inversion Theorem tells us that 10 is the inverse of 7 mod 23. 

<div class="element">
<span class="label">Exercise</span>
For each of the following, determine whether $a$ is invertible mod $n$. If it is, find an inverse of $a$ mod $n$. 

a. $a = 14, n = 21$
b. $a = 3, n = 7$
c. $a = 41, n = 50$
</div>

<div class="element">
<span class="label">Exercise</span>
Solve the following congruences for $z$. 

a. $2z \equiv 3 \pmod{11}$
b. $3z \equiv 2 \pmod{7}$
c. $5z \equiv 3 \pmod{15}$
d. $5z \equiv 17 \pmod{101}$
</div>

We remarked earlier that Bézout coefficients aren't unique, and inverses aren't strictly unique either. Notice, for example, that $3 \cdot 2 \equiv 1 \bmod{5}$ and $8 \cdot 2 \equiv 1 \bmod{5}$, so 8 and 3 are both inverses of 2 mod 5. However, notice that $8 \equiv 3 \bmod{5}$. In other words, inverses are "kind of" unique when they exist -- they are unique mod $n$. This is what the following result says: 

<div class="element">
<span class="label">Lemma</span>
Fix a positive integer $n$ and suppose $a$ is invertible mod $n$. If $x$ and $x'$ are both inverses of $a$ mod $n$, then $x \equiv x' \pmod{n}$.  
</div>

*Proof*. By definition of inverse, we know that $ax \equiv 1 \bmod{n}$. Multiplying both sides by $x'$ and applying the Modular Arithmetic Theorem, we see that 
$$ x = 1 \cdot x \equiv (x' a)x = x'(ax) \equiv x' \cdot 1 = x' \pmod{n}, $$
which is what we wanted to show. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">SageCell</span>
Sage can calculate modular inverses using the function `inverse_mod`. You have to pass it two arguments: the number you want the inverse of, and then the modulus. For example, the following computes $7^{-1} \bmod{23}$. Again, it's fast: try it with big numbers!
<div class="sage">
<script type="text/x-sage">
inverse_mod(7, 23)
</script>
</div>
</div>

That's plenty of modular arithmetic for now. Let's get back to describing some more ciphers! 

