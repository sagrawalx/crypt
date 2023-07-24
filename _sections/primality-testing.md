We have one more theoretical ingredient to discuss for now: how can we quickly figure out if a number is prime? Remember that factoring is computationally expensive, so it's not a good idea to try to figure out that a number is prime by factoring it!

It turns out that there are a number of fast strategies for testing primality. Here we'll discuss one called the Miller-Rabin test. To explain how the this test works, we need to establish some theoretical preliminary results. 

<div class="element">
<span class="label">Lemma</span>
If $n$ is prime, the only solutions to $x^2 \equiv 1 \pmod{n}$ are $x \equiv \pm 1 \pmod{n}$. 
</div> 

*Proof*. Observe that $x^2 \equiv 1 \pmod{n}$ implies 
$$0 \equiv x^2 - 1 = (x-1)(x+1) \pmod{n}, $$
which means that $n$ divides $(x-1)(x+1)$. Since $n$ is prime, Euclid's lemma implies that $n$ divides either $x-1$ or $x+1$, which means we have either $x-1 \equiv 0 \pmod{n}$ or $x+1 \equiv 0 \pmod{n}$. In other words, we have either $x \equiv 1 \pmod{n}$ or $x \equiv -1 \pmod{n}$, respectively. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Miller-Rabin Lemma</span>
Suppose $n$ is a positive odd integer and write $n-1 = 2^s d$ for some positive integer $s$ and an odd number $d$. Suppose $a$ is an integer between 1 and $n-1$. If $n$ is prime, then one of the following congruence relations must hold true: 
$$ \begin{aligned}
a^d &\equiv 1 \pmod{n} \\
a^d &\equiv -1 \pmod{n} \\
a^{2d} &\equiv -1 \pmod{n} \\
a^{2^2d} &\equiv -1 \pmod{n} \\
&\,\vdots \\
a^{2^{s-1}d} &\equiv -1 \pmod{n}
\end{aligned} $$
</div>

*Proof*. Since $1 \leq a \leq n-1$ and $n$ is prime, we have $\gcd(a, n) = 1$. Thus, by Euler's Theorem, we know that $a^{n-1} = a^{2^s d} \equiv 1 \pmod{n}$. This means that $x_1 = a^{2^{s-1}d}$ is satisfies
$$ x^2_1 = (a^{2^{s-1}d})^2 = a^{2^s d} \equiv 1 \pmod{n}, $$
so by the previous lemma, we must have either $x_1 \equiv -1 \pmod{n}$, in which case we're done, or else $x_1 \equiv 1 \pmod{n}$. In the latter case, we can repeat the same argument with $x_1$ replaced by $x_2 = a^{2^{s-2}d}$, and so on, all the way down to $x_s = a^d$. <span style="float: right;">$\Box$</span>

Let's see what this lemma is saying in an example. Consider $n = 41$, which is prime. We then have $n-1 = 40 = 2^3 \cdot 5$. In other words, we can take $s = 3$ and $d = 5$ in the above lemma. Suppose we fix some integer $a = 17$. Since $s = 3$, we have 4 congruences in our list, and one of them should then be true, so let's check which one! We first compute $17^5 \equiv 27 \pmod{41}$ using binary exponentiation (as described in the previous section). This is not $\pm 1$, so the first two congruence in the list fail. We then have
$$ 17^{2 \cdot 5} = (17^5)^2 \equiv 27^2 \equiv 32 \bmod{41} $$
so the third congruence also fails. But then we have 
$$ 17^{2^2 \cdot 5} = (17^{2 \cdot 5})^2 \equiv 32^2 \equiv 40 \equiv -1 \pmod{41} $$
so the last congruence is in fact true!

<div class="element">
<span class="label">Exercise</span>

For each of the following integers $n$, find the integers $s$ and $d$ such that $n-1 = 2^s d$, where $d$ odd. 

a. 43
b. 49
c. 65
</div>

The Miller-Rabin Lemma leads us to the following definition: 

<div class="element">
<span class="label">Definition</span>
Suppose $n$ is a positive odd integer and write $n-1 = 2^s d$ for some positive integer $s$ and an odd number $d$. Suppose $a$ is an integer between 1 and $n-1$. We say that $n$ is a *strong probable prime to base $a$* if one of the following congruences is true: 
$$ \begin{aligned}
a^d &\equiv 1 \pmod{n} \\
a^d &\equiv -1 \pmod{n} \\
a^{2d} &\equiv -1 \pmod{n} \\
a^{2^2d} &\equiv -1 \pmod{n} \\
&\,\vdots \\
a^{2^{s-1}d} &\equiv -1 \pmod{n}
\end{aligned} $$
If $n$ is *not* a strong probable prime to base $a$, then $a$ is called a *witness for the compositness* of $a$, or a *Miller-Rabin witness* for $a$. 
</div>

Using this definition, the Miller-Rabin Lemma can be restated as saying simply that "every prime number is a strong probable prime to any base." This is equivalent to the contrapositive, ie, the statement that "if $n$ is *not* a strong probable prime to some base $a$, then $n$ must be composite."

Consider, for example, $n = 25$. Of course, this number is small enough that we can see right away that it is not actually prime! But let's see what's going on with this notion of strong probable primes. Observe that $n-1 = 24 = 2^3 \cdot 3$, so we have $s = 3$ and $d = 3$. Suppose first that we choose a base of $a = 7$. Then
$$ \begin{aligned}
7^3 &\equiv 18 \pmod{25} \\
7^{2 \cdot 3} &= (7^3)^2 \equiv 18^2 \equiv 24 \equiv -1 \pmod{25}
\end{aligned} $$
so this says that 25 is a strong probable prime to base 7. If we instead choose $a = 2$, then we have 
$$ \begin{aligned}
2^3 &= 8 \\
2^{2 \cdot 3} &= (2^3)^2 = 8^2 \equiv 64 \equiv 14 \pmod{25} \\
2^{2^2 \cdot 3} &= (2^{2 \cdot 3})^2 \equiv 14^2 \equiv 21 \pmod{25}
\end{aligned} $$
Thus $n$ is not a strong probable prime to base 2, and 2 is a witness to the compositeness of 25. 

<div class="element">
<span class="label">Exercise</span>

a. Check that 49 is a strong probable prime to base 18. Check also that 2 is a witness to the compositeness of 49. 
b. Check that $221$ is a strong probable prime to the base 174. Check also that 2 is a witness for the compositeness of 221.
</div>

<div class="element">
<span class="label">SageCell</span>
Here is some Sage code for a function called `is_strong_probable_prime` that checks if a number $n$ is a strong probable prime to some base $a$. At the very end, we call `is_strong_probable_prime(25, 7)` to check if 25 is a strong probable prime to the base 7. It should return `True`. Hit "Run" to check! Then try changing $n$ and $a$ and experimenting. 
<div class="sage">
<script type="text/x-sage">
def is_strong_probable_prime(n, a):
    # Deal with even values of n
    if n == 2:
        return True
    elif n % 2 == 0:
        return False

    # Find s and d
    s = 0
    d = n-1
    while d % 2 == 0:
        s += 1
        d = d // 2
    
    # Compute a^d mod n
    x = power_mod(a, d, n)
    
    # If a^d \equiv 1 \pmod{n}, we are done
    if x == 1:
        return True
        
    # Otherwise, we check the -1 congruences, squaring repeatedly
    for i in range(s):
        if x == n-1:
            return True
        x = power_mod(x, 2, n)
    
    # If we've made it to the end, it means none of the congruences hold, so...
    return False
    
is_strong_probable_prime(25, 7)
</script>
</div>
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why any positive odd integer $n$ is always a strong probable prime to base 1, and also to base $n-1$. 
</div>

The idea now is as follows. If $n$ is composite, the *most* of the possible choices of base $2 \leq a \leq n-2$ will in fact be witnesses for the compositeness of $n$. This means that, if we try several such bases and none of them turn out to be a witness for compositness, we can be quite sure that $n$ is in fact prime!

<div class="element">
<span class="label">Sage Exercise</span>
Write some code that plots, for each positive odd *composite* integer $n$ up to 10000, the percentage of bases $2 \leq a \leq n-2$ such that $n$ is a strong probable prime to base $a$. (You should find that no more than 25% of bases have this property.)
</div>

We've just described the Miller-Rabin primality test: 

<div class="element">
<span class="label">Miller-Rabin Primality Test</span>
Suppose $n$ is a positive integer. If $n$ is 2, output `True`. Otherwise, if $n$ is even, output `False`. Otherwise, repeat the following step some fixed pre-determined number of times: 

* Choose a random base $2 \leq a \leq n-2$. If $a$ is a witness for the compositeness of $n$, output `False`.

If we reach the end without having output `False`, then output `True`.  
</div>

If we do $k$ repetitions, the probability of a false positive is less than $4^{-k}$. This gets small very quickly! For example, if we do just 10 repetitions, the probability of a false positive is about one in a million!

<div class="element">
<span class="label">Exercise</span>
Perform the Miller-Rabin primality test by hand for each of the following numbers: 

a. 13
b. 15 
c. 9
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Write some code that implements the Miller-Rabin primality test. It should be a function that takes as input a positive integer $n \geq 2$ and a number of iterations $k$, and then perform at most $k$ iterations as described above, returning either `True` or `False` at the end. Feel free to use the `is_strong_probable_prime` function defined in the SageCell above. 
</div>

If you don't like the idea of a probabilistic algorithm, you may be interested in reading about the [AKS Primality Test](https://en.wikipedia.org/wiki/AKS_primality_test), which is a fully deterministic algorithm that runs in polynomial time (ie, polynomial in the number of digits in the input). Note, though, that the AKS primality test often runs much slower than the Miller-Rabin test...! If you like, you can also read about [deterministic variants of the Miller-Rabin test](https://en.wikipedia.org/wiki/Miller%E2%80%93Rabin_primality_test#Deterministic_variants).

The upshot is that we can quickly check if a number is prime without having to factor it. This also gives us an algorithm for generating large prime numbers: basically, just keep generating random numbers until we find one that's prime!

<div class="element">
<span class="label">Algorithm for Generating Large Primes</span>
Let $r$ be a positive integer. To generate an $r$-bit prime, run the following steps:  

* Pick a random odd integer $n$ between $2^{r-1}$ and $2^r - 1$. 
* Check if $n$ is prime. 
    - If it is, return $n$. 
    - Otherwise, return to the first step. 
</div>

<div class="element">
<span class="label">Exercise</span>
The first step of the algorithm above requires you to pick a random *odd* integer between $2^{r-1}$ and $2^r - 1$. How would you go about ensuring that the number you pick is odd and that every odd number between $2^{r-1}$ and $2^r - 1$ is equally likely? *Hint*. It might be helpful to think about binary representations. 
</div>

<div class="element">
<span class="label">SageCell</span>
Sage has functionality to generate a random prime using the function `random_prime`. The algorithm it uses is very close to what's described above, though the primality test is not exactly the Miller-Rabin test (it instead uses the [Baillie-PSW primality test](https://en.wikipedia.org/wiki/Baillie%E2%80%93PSW_primality_test), which is a probabilistic primality test that involves some of the same ideas). By default, it generates a random prime between 2 and the input number: 
<div class="sage">
<script type="text/x-sage">
# Generates a random "prime" between 2 and 2^100-1 
random_prime(2^100-1)
</script>
</div>
If you want to generate a random prime between something bigger than 2, you have to tell it as follows: 
<div class="sage">
<script type="text/x-sage">
# Generate a random 100-bit "prime" 
random_prime(2^100-1, lbound=2^(100-1))
</script>
</div>
Sage is using a probabilistic test, so it isn't *guaranteed* that the number will actually be prime: it's just very likely to be prime! If you want Sage to verify that the number is actually prime, you can do that too: 
<div class="sage">
<script type="text/x-sage">
# Generate a random prime between 2 and 2^100-1, and verify that it is actually prime
random_prime(2^100-1, proof=True)
</script>
</div>
</div>

