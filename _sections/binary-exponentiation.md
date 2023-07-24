With Euler's theorem in hand, we can now compute large powers in modular arithmetic very quickly. We saw this briefly above, but let's now explain the process systematically. Suppose we have a positive integer $n$ and an integer $a$ with $\gcd(a, n) = 1$, and that we want to compute $a^m \bmod{n}$ for some large number $n$. 

The first step is to calculate $r = m \bmod{\phi(n)}$. Indeed, if we do this, then we have $m = \phi(n)q + r$, so
$$ a^m = a^{\phi(n)q + r} = (a^{\phi(n)})^q a^r \equiv 1^q a^r = a^r \pmod{n} $$
using Euler's Theorem (and the Modular Arithmetic Theorem). This immediately reduces the power we have to compute substantially. 

We can then compute $a^r \bmod{n}$ using a technique called "binary exponentiation." The idea is fairly straightforward in examples, though it can becomes hard to understand in the abstract, so let's start by considering an example.

Suppose we find that $r = 25$. How can we compute $a^r \bmod{n}$? One naive thing we could do is multiply $a$ by itself mod $n$ repeatedly; this would require 25 multiplications mod $n$. Here's another idea. Let's start squaring repeatedly:

* Find $a^2 \bmod{n}$.
* Find $a^4 = (a^2)^2 \bmod{n}$ by squaring the result of the previous step. 
* Find $a^8 = (a^4)^2 \bmod{n}$ by squaring the result of the previous step. 
* Find $a^{16} = (a^8)^2 \bmod{n}$ by squaring the result of the previous step. 

If we square again, we'll go past $r = 25$, so we'll stop there. We now want to figure out how to find $a^{25}$ using the powers of $a$ we've already computed. Notice that $25 = 16 + 8 + 1$, so 
$$ a^{25} = a^{16+8+1} = a^{16} a^8 a^1, $$
and we already know $a^{16}, a^8$, and $a$ mod $n$, so we can compute $a^{25} \bmod{n}$ by multiplying these three values together mod $n$. Notice that this required only 6 multiplications total --- much less than the 25 of the naive method!

Let's see this entire process in action before describing the binary exponentiation part in the abstract. Suppose we want to compute 
$$ 3^{4398391} \bmod{80}. $$
We first compute $\phi(80) = 32$, and that $4398391 \equiv 23 \bmod{32}$. Thus $3^{4398391} \equiv 3^{23} \bmod{80}$ by Euler's Theorem. We now use binary exponentiation: 
$$ \begin{aligned}
3^2 &= 9 \\
3^4 &= (3^2)^2 = 9^2 = 81 \equiv 1 \bmod{80} \\ 
3^8 &= (3^4)^2 \equiv 1^2 = 1 \bmod{80} \\
3^{16} &= (3^8)^2 \equiv 1^2 = 1 \bmod{80} \\
3^{23} &= 3^{16 + 4 + 2 + 1} = 3^{16} 3^4 3^2 3 \equiv 1 \cdot 1 \cdot 9 \cdot 3 = 27 \bmod{80}
\end{aligned} $$

<div class="element">
<span class="label">Exercise</span>
Compute $3^{293423948903859017} \bmod{50}$. 
</div>

The "repeated squaring and then multiplying together" part of this process is very closely related to finding binary representations of integers. Here is the definition: 

<div class="element">
<span class="label">Definition</span>
Let $r$ be a non-negative integer. The *binary representation* of $r$ is a string $b_k \cdots b_1 b_0$ where each $b_i$ is either 0 or 1, and where
$$ r = b_0 + b_1 2 + b_2 2^2 + \cdots + b_k 2^k. $$
The number $b_i$ is called the $i$th *bit* of $r$. We call $b_0$ the *rightmost bit* and $b_k$ the *leftmost bit*. 
</div>

For example, the binary representation of 25 is 11001 because
$$ 1 + 0 \cdot 2 + 0 \cdot 2^2 + 1 \cdot 2^3 + 1 \cdot 2^4 = 1 + 8 + 16 = 25. $$
It is not clear at all from the definition how to *find* this binary representation, but here is the algorithm for doing this!

<div class="element">
<span class="label">Algorithm for Finding Binary Representations</span>
Let $r$ be a non-negative integer. To find the binary representation of $r$, divide $r$ by 2, and then divide the quotient by 2, and then divide that quotient by 2, and so forth, until you hit a quotient of 0. The remainders of these divisions are the binary representation, with the last remainder corresponding to the leftmost bit. 
</div>

For example, suppose we want to calculate the binary representation of 193. We divide 193 by 2, and then repeatedly divide the quotient by 2: 
$$ \begin{aligned}
193 &= 96 \cdot 2 + 1 \\
96 &= 48 \cdot 2 + 0 \\
48 &= 24 \cdot 2 + 0 \\
24 &= 12 \cdot 2 + 0 \\
12 &= 6 \cdot 2 + 0 \\
6 &= 3 \cdot 2 + 0 \\
3 &= 1 \cdot 2 + 1 \\
1 &= 0 \cdot 2 + 1
\end{aligned} $$
We've now hit a quotient of 0, so we stop dividing. The binary representation is the sequence of remainders we found, with the leftmost bit being the last remainder we found and the rightmost bit being the first remainder we found. In other words, the binary representation of 193 is 11000001. 

There's a technical remark worth making here for those who are interested in computation. The above process is an "algorithm," but it's an algorithm that only human beings really have to perform...! A computer doesn't have to go through the above process to find the binary representation, because a computer will already know the binary representation of any integer: binary representations are how computers talk about integers!

<div class="element">
<span class="label">Exercise</span>
Find binary representations of the following integers.

a. 37
b. 123
c. 290
d. 300
</div>

<div class="element">
<span class="label">Exercise</span>
Why must the rightmost bit in the binary representation of an even number must be 0?
</div>

<div class="element">
<span class="label">Exercise</span>
If an integer is divisible by 8, what can you say about its binary representation?
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Prove that the output of the above algorithm is in fact the binary representation of $r$. 
</div>

We can now state the general fact about binary exponentiation we described in the context of examples above: 

<div class="element">
<span class="label">Binary Exponentiation Lemma</span>
Let $n$ be a positive integer, and let $b_k \cdots b_1 b_0$ be the binary representation of a non-negative integer $r$. To compute $a^r \bmod{n}$ for some integer $a$, first compute $a^{2^i} \bmod{n}$ for $i = 0, \dotsc, k$ by repeated squaring. Then, to get $a^r$, multiply together all of the $a^{2^i}$ where $b_i = 1$. In other words, 
$$ a^r \equiv \prod_{b_i = 1} a^{2^i} \pmod{n}. $$
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Prove the above lemma.
</div>

<div class="element">
<span class="label">Exercise</span>
Show that the number of multiplications mod $n$ that are required to compute $a^r \bmod{n}$ with binary exponentiation is at most $2\log_2(r)$. 
</div>

<div class="element">
<span class="label">SageCell</span>
Sage performs the algorithm we've described in this section when you ask it to compute remainders of high powers. For example, the following code computes $3^{4398391} \bmod{80}$ using this procedure of Euler's Theorem followed by binary exponentiation. 
<div class="sage">
<script type="text/x-sage">
power_mod(3, 4398391, 80)
</script>
</div>
</div>

<div class="element">
<span class="label">SageCell</span>
Sage can compute binary representations for you as follows. Note that the output will be prefixed with `0b` to indicate that what follows is a binary representation.
<div class="sage">
<script type="text/x-sage">
bin(193)
</script>
</div>
</div>

<div class="element">
<span class="label">Exercise</span>
A binary representation is also called a base 2 representation. What is a base 3 representation, and how do we compute it? How about base 4 representations? 
</div>

<div class="element">
<span class="label">Exercise</span>
Read about [hexadecimal representations](https://en.wikipedia.org/wiki/Hexadecimal) of integers, and then explain what's going on in your own words.
</div>

