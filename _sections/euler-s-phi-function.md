Here's an idea that we've already seen before: 

<div class="element">
<span class="label">Definition</span>
For a positive integer $n$, let $\phi(n)$ denote the number of integers $r$ with $0 \leq r < n$ and $\gcd(n, r) = 1$. The function $n \mapsto \phi(n)$ is called *Euler's phi function* (or *Euler's totient function*). 
</div>

For example, when we were counting that there are 312 affine encryption functions available in English, part of the process involved counting the number of numbers relatively prime to 26, and we found that it was 12. We can now say this more succinctly by saying $\phi(26) = 12$. 

<div class="element">
<span class="label">Exercise</span>
Compute $\phi(12), \phi(13), \phi(14)$, and $\phi(15)$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why, in a language that uses an alphabet with $n$ letters, the number of distinct affine encryption functions is $n\phi(n)$. 
</div>

<div class="element">
<span class="label">Important Exercise</span> 
Suppose $p$ is prime. 

a. Explain why $\phi(p) = p-1$.
b. Explain why $\phi(p^e) = p^e - p^{e-1} = p^{e-1}(p-1)$ for any $e \geq 1$. 
</div>

In fact, there is a nice formula for $\phi(n)$ in terms of the prime factorization of $n$, which we now describe. First, let us record the following property.

<div class="element">
<span class="label">Multiplicativity of Euler's Phi Function</span>
If $\gcd(a, b) = 1$, then $\phi(ab) = \phi(a)\phi(b)$. 
</div>

*Proof sketch*. Observe that $\gcd(r, ab) = 1$ if and only if $\gcd(r, a) = 1$ and $\gcd(r, b) = 1$. Let's arrange the numbers 0 through $ab-1$ in a grid as follows. 
$$ \begin{matrix} 0 & 1 & 2 & \cdots & b-1 \\ 
b & b+1 & b+2 & \cdots & 2b-1 \\
2b & 2b+1 & 2b+2 & \cdots & 3b-1 \\
(a-1)b & (a-1)b + 1 & (a-1)b + 2 & \cdots & ab-1 \end{matrix} $$
All of the numbers in a given column are congruent mod $b$, so if if one of these numbers is relatively prime with $b$, then every number in that column must also be relatively prime with $b$. Thus there are $\phi(b)$ columns that are relatively prime with $b$, and it is sufficient to show that each of those columns has $\phi(a)$ entries which are relatively prime with $a$. 

The entries in the $r$th column are of the form $sb + r$ for $s$ varying between 0 and $a-1$. Since $\gcd(a, b) = 1$, every congruence class mod $a$ is represented exactly once in each column. We know that $\phi(a)$ of the congruence classes mod $a$ are relatively prime to $a$, so we are done. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Proof Exercise</span>
Fill in the details in the above proof sketch. In particular, explain how the Affine Cipher Lemma plays into the ideas discussed in the second paragraph of the sketch above. 
</div>

This brings us to the promised formula. 

<div class="element">
<span class="label">Formula for Euler's Phi Function</span>
Suppose $n = p_1^{e_1} \cdots p_r^{e_r}$ is the prime factorization of $n$. Then
$$ \phi(n) = p_1^{e_1-1} (p_1-1) \cdots p_r^{e_r-1}(p_r - 1). $$
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Prove the formula for Euler's phi function using the multiplicativity property and the calculation of $\phi(p^e)$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Use the formula for Euler's phi function to calculate $\phi(n)$ for each of the following values of $n$. 

a. $n = 20 = 2^2 \cdot 5$
b. $n = 25 = 5^2$
c. $n = 30 = 2 \cdot 3 \cdot 5$
d. $n = 35 = 5 \cdot 7$
e. $n = 40 = 2^3 \cdot 5$
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why $\phi(n)$ is always even when $n \geq 3$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why we can also write 
$$\phi(n) = n \prod_{p \mid n} \left( 1 - \frac{1}{p} \right). $$ 
</div>

<div class="element" id="sagecell-hill">
<span class="label">SageCell</span>
Sage can work with Euler's phi function as well. Try it!
<div class="sage">
<script type="text/x-sage">
euler_phi(75)
</script>
</div>
</div>

We can now calculate Euler's phi function, but the reason this number is important is because of the following very important theorem. 

<div class="element">
<span class="label">Euler's Theorem</span>
Suppose $n$ is a positive integer and $a$ is another integer with $\gcd(a, n) = 1$. Then 
$$ a^{\phi(n)} \equiv 1 \pmod{n}. $$
</div>

*Proof sketch*. Let $r_1, \dotsc, r_{\phi(n)}$ be the integers between 0 and $n-1$ which are relatively prime to $n$. Since $\gcd(a, n) = 1$, the integers $ar_1, \dotsc, ar_{\phi(n)}$ represent the same congruence classes, but maybe in a different order. Since multiplication of integers is commutative, we have
$$ r_1 \cdots r_{\phi(n)} \equiv (ar_1) \cdots (ar_{\phi(n)}) = a^{\phi(n)} r_1 \cdots r_{\phi(n)} \pmod{n}. $$
We can now multiply mod $n$ by $r_{\phi(n)}^{-1}$, and $r_{\phi(n)-1}^{-1}$, and so forth, all the way down to $r_1^{-1}$ to conclude that
$$ 1 \equiv a^{\phi(n)} \pmod{n}, $$
which is what we wanted to show. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Proof Exercise</span>
Fill in the details in the above proof sketch. 
</div>

Let's consider an example. Notice that $20 = 2^2 \cdot 5$, so
$$ \phi(20) = 2^1(2-1) \cdot 5^0 (5-1) = 8. $$
This means that $a^{\phi(20)} = a^8 \equiv 1 \bmod{20}$ for any $a$ that is relatively prime to 20. For example, consider $a = 3$. We do in fact have $\gcd(3, 20) = 1$, and we can check explicitly using the Modular Arithmetic Theorem that: 
$$ \begin{aligned}
3^2 &= 9 \\
3^4 &= (3^2)^2 \equiv 9^2 = 81 \equiv 1 \pmod{20} \\
3^8 &= (3^4)^2 \equiv 1^2 = 1 \pmod{20}.
\end{aligned} $$
We can also use this Euler's theorem to compute very very large powers of some number. Suppose, for example, that we want to compute $7^{20232023} \bmod{20}$. Notice that $20232000$ is divisible by 8, so we have
$$ 20232023 = 20232000 + 23 \equiv 23 \equiv 7 \bmod{8}. $$
In other words, this means that $20232023 = 8q + 7$ for some integer $q \geq 0$. We then have
$$ 7^{20232023} = 7^{8q + 7} = (7^8)^q 7^7 \equiv 1^q 7^7 = 7^7 \pmod{20}. $$
So now we just have to compute $7^7 \bmod{20}$, which we can do as follows: 
$$ \begin{aligned}
7^2 &= 49 \equiv 9 \bmod{20} \\
7^4 &= (7^2)^2 \equiv 9^2 = 81 \equiv 1 \bmod{20} \\
7^7 &= 7^4 \cdot 7^2 \cdot 7 \equiv 1 \cdot 9 \cdot 7 = 63 \equiv 3 \bmod{20} 
\end{aligned} $$
We thus have $7^{20232023} \bmod{20} = 3$. 

Notice that we were able to do this remainder calculation without having to actually compute $7^{20232023}$. This is an important observation, because we'll see below that RSA will require us to do similar remainder calculations even when the numbers are so large that it is completely infeasible for computers to actually calculate out the result of the exponentiation. We'll discuss this point in more detail in the following section. 

Meanwhile, here are some exercises to practice with Euler's theorem!

<div class="element">
<span class="label">Exercise</span>
Use Euler's theorem to show that 51 divides $10^{32n+9} - 7$ for any integer $n \geq 0$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Find the units digit of $3^{100}$ using Euler's theorem. 
</div>


<div class="element">
<span class="label">Exercise</span>
Fix a prime number $p$. There are two versions of "Fermat's little theorem." 

a. If $a$ is an integer that is not divisible by $p$, then $a^{p-1} \equiv 1 \pmod{p}$. 
b. For any integer $a$, we have $a^p \equiv a \pmod{p}$. 

Explain how to derive both of these statements from Euler's theorem. You may find it helpful to first derive statement (a), and then use that to derive statement (b). 
</div>

