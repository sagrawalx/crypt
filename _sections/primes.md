Here is a definition that you may have seen before:

<div class="element">
<span class="label">Definition</span>
A positive integer $p \geq 2$ is *prime* if its only positive divisors are 1 and itself. An integer $n \geq 2$ that is not prime is called *composite*.
</div>

For example, 5 is prime since 1 and 5 are its only divisors. 4, on the other hand, is not prime, since 2 is a divisor of 4 (in addition to 1 and 4).


<div class="element">
<span class="label">Exercise</span>
The "twin prime conjecture" is a famous open problem which says that there are infinitely many "twin primes," ie, pairs of primes that are 2 apart. For example, 3 and 5 are twin primes, as are 5 and 7, or 11 and 13. Give five more examples of twin primes. 
</div>

<div class="element">
<span class="label">Exercise</span>
There is a version of the twin prime conjecture which says that every even integer can be written as the difference of consecutive primes in infinitely many ways. For example, we have:
$$ 6 = 29 - 23 = 137 - 131 = 599 - 593 = \cdots $$
Express the integer 10 as the difference of two consecutive primes in five different ways. 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why any composite integer $n \geq 2$ must have a prime factor $p$ such that $p \leq \sqrt{n}$. Then use this fact to determine whether or not 701 is prime. 
</div>

### Ubiquity of Primes

There is a very important sense in which primes are "ubiquitous" --- namely, that every positive integer $n \geq 2$ can be written (uniquely!) as a product of primes. For example, $18 = 2 \cdot 3^2$, and both 2 and 3 are prime. An expression of an integer $n \geq 2$ as a product of primes is called a *prime factorization* of $n$. 

<div class="element">
<span class="label">Fundamental Theorem of Arithmetic</span>
Any positive integer $n \geq 2$ has a unique prime factorization. In other words, there exists an expression
$$ n = p_1^{e_1} \cdots p_r^{e_r} $$
where $p_1, \dotsc, p_r$ are primes and $e_1, \dotsc, e_r$ are positive integers, and this expression is unique up to reordering the indices.
</div>

For example, $60 = 2^2 \cdot 3^1 \cdot 5^1$ is a prime factorization of 60. It is the only one: we could instead write $60 = 5^1 \cdot 2^2 \cdot 3^1$, but besides this kind of reordering of the factors, there is nothing else. 

<div class="element">
<span class="label">Exercise</span>
Find the prime factorizations of 1231 and of 1232. 
</div>

<div class="element">
<span class="label">Exercise</span>

a. Find all prime factors of $50!$. (Just a list of the prime factors is sufficient; you don't need to find the exponents of the prime factorization for this part.)

b. Find the prime factorization of $10!$. 

c. Find the prime factorization of $11!/2^8$. 
</div>

The fundamental theorem of arithmetic is likely deeply ingrained in your mathematical intuition, but it is in fact a "theorem" --- it is something that can and should be proved. The proof makes use of some of the machinery we've already seen, so let's prove it!

*Proof of the fundamental theorem of arithmetic*. The existence statement is a great example of a proof by "strong induction." We start by noticing that 2 is prime, so it is its own prime factorization. Now suppose that $n \geq 2$ and that every integer $m$ such that $2 \leq m < n$ has a prime factorization. If $n$ itself is prime, it is again its own prime factorization and we are done. If $n$ is composite, by definition it has a divisor $m$ such that $2 \leq a < n$. This means that $n/a = b$ is an integer with $2 \leq b < n$ as well. By assumption, both $a$ and $b$ have prime factorizations; since $n = ab$, multiplying those prime factorizations together gives us a prime factorization of $n$. 

For the uniqueness statement, suppose for a contradiction that there is an integer greater than or equal to 2 that can be written as a product of primes in two different ways. But then there must be a *smallest* such integer; let's call that integer $n$. By assumption, we have $n = p_1 \cdots p_t = q_1 \cdots q_u$. Since $p_1$ divides the left-hand side, it must also divide the right hand side. We must have $\gcd(p_1, q_i) = 1$ or $p_1$, but it cannot be equal to 1 for all $i$ by Euclid's lemma. Thus there exists some $i$ such that $\gcd(p_1, q_i) = p_1$, which means that $p_1$ divides $q_i$. Since $q_i$ is also prime, we must have $p_1 = q_i$, so then we can cancel $p_1 = q_i$ from both sides to see that $p_2 \cdots p_t = q_1 \cdots q_{i-1} q_{i+1} \cdots q_u$. But now we have two distinct prime factorizations of a number that's strictly smaller than $n$, which is a contradiction! <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Proof Exercise</span>
Suppose $a$ and $b$ are positive integers. Let $p_1, \dotsc, p_r$ be a list of distinct primes that contains every prime factor of both $a$ and $b$. 

a. Explain why there exist non-negative integers $e_1, \dotsc, e_r, f_1, \dotsc, f_r$ such that $a = p_1^{e_1} \cdots p_r^{e_r}$ and $b = p_1^{f_1} \cdots p_r^{f_r}$. 
    
b. Show that $\gcd(a, b) = p_1^{\min\{e_1, f_1\}} \cdots p_r^{\min\{e_r, f_r\}}$. 
</div>

<div class="element">
<span class="label">Proof Exercise</span>
It is a fact that there are infinitely many prime numbers. Prove this by contradiction in two different ways: 

a. Suppose there are only finitely many primes $p_1, \dotsc, p_n$, and then consider $a = p_1 \cdots p_n + 1$. Find a contradiction. 

b. Suppose there are only finitely many primes and let $n$ be an integer that's greater than all of them. Then consider $a = n!+1$. Find a contradiction. 
</div>

### Scarcity of Primes

Having made the observation that primes are "ubiquitous," we now turn to two important senses in which primes are "scarce." The first sense is a literal sense: as a proportion of all integers, very few integers end up being prime!

<div class="element">
<span class="label">SageCell</span>
Here is a SageCell to convince you that primes are scarce. Hit "Run." What you see is a plot of a function $f$ where, for any real number $x \geq 2$, the corresponding $y$-value $f(x)$ is the proportion of integers less than or equal to $x$ that are prime, for any real number $x \geq 2$. Notice how, in the long run, very few numbers are actually prime! Try changing the upper endpoint 1000 and investigate how the graph changes. 
<div class="sage">
<script type="text/x-sage">
plot(lambda x: prime_pi(x)/x, 2, 1000)
</script>
</div>
</div>

Let me also just mention in passing that the scarcity of primes we see in the plot above is formalized by the statement of "[Prime number theorem](https://en.wikipedia.org/wiki/Prime_number_theorem)," which is a difficult but landmark theorem in analytic number theory. 

### Difficulty of Factoring

The second sense in which primes are "scarce" is that prime factorizations are extremely difficult to actually calculate, even for moderately large numbers. 

The naive way to find a prime factorization is to just start going through to see what's a factor. Let's say, for example, that we want to factor 75. We check first that 2 does not divide 75. We then see that 3 does divide 75, and we're left with 25. Now 3 no longer divides 25. 4 also does not divide 25 (which we anyway knew since we already saw that 2 didn't divide 75), but 5 does divide 25, so we divide again and we're left with 5. And 5 divides itself again, which leaves a 1 in the end and we can stop. This procedure tells us that
$$ 75 = 3^1 \cdot 5^2. $$
As you might imagine, this procedure gets extremely slow as the number gets larger --- or more specifically, as the prime factors of the number get larger. 

Computational number theorists have worked out lots of deep, sophisticated, and clever techniques (which we do not have time to discuss this quarter) to speed this process up, but no one has yet found a factorization algorithm for classical computers that is *substantially* better than the naive method we just described. If you've seen some computational complexity theory, you might know what it means to say that there is no known factorization algorithm which is polynomial in the number of digits in the input. 

<div class="element" id="sagecell-hill">
<span class="label">SageCell</span>
Here is a SageCell to convince you that prime factorization is difficult. The Sage function `factor` will factor integers. It tries to use as much sophisticated math as possible to make this as fast as possible. It's certainly much faster than a human being, but it starts becoming infeasibly slow as the size of the prime factors of the numbers grow. Hit `Run` to see Sage factor a number with several "small" prime factors. 
<div class="sage">
<script type="text/x-sage">
factor(9845181225634534546543656534524552443341112317013333179953)
</script>
</div>
Then try factoring:

```
98451812256345345465436565462354534524552443341112317013333179953
```

Sage will take a long time to get back to you, but it should get back to you eventually; be patient. Then try adding or subtracting digits to the number on your own whim to see what happens! Do you see patterns in when Sage returns an answer quickly, or when Sage seems to struggle? 
</div>

We'll see below that the difficulty of factoring can be leveraged to build modern cryptosystems that are in widespread use today. 

Incidentally, you may have noticed two things in the discussion above. First, we said only that no substantially better algorithm is *known*, not that there is no such algorithm. We do not have a proof that prime factorization cannot be done "quickly" (ie, in polynomial time), and it is an open problem in mathematics and/or theoretical computer science to prove this. (Or to disprove it, but given how much time and money people have put into trying to find a quick factorization algorithm, this seems unlikely...!)

The second thing you may have noticed is the phrase "classical computers." In fact, we do know efficient factorization algorithms for *quantum* computers! Quantum computing hardware has not yet caught up to our theoretical knowledge, so our modern cryptosystems are still safe for now, but this state of affairs won't last for much longer. We will soon need new cryptosystems that are secure against quantum computers.

The difficulty of factoring suggests that the function $\mu$, which takes as input two prime numbers $p$ and $q$ and outputs the product $$ \mu(p, q) = pq $$
is our first example of a one-way function. It's very easy to compute the product of two primes; but, if the primes are large, it'll be very hard to invert this function (ie, to find the prime factors of some given number that has two large prime factors). This is the one-way function on which RSA is based, as we will see soon. But first, we need to develop still more theory. 

