The Elgamal cryptosystem is a public-key cryptosystem like RSA, named after the Egyptian cryptographer [Taher Elgamal](https://en.wikipedia.org/wiki/Taher_Elgamal). 

### How Elgamal Works

The process begins by Bob choosing a public key. He picks a prime number $p$ and a primitive root $g$ of $p$. He chooses a random integer $x$ with $0 \leq x < p-1$. This is his private key. He then computes $h = g^x \bmod{p}$ and his public key is the triple $(p, g, h)$. 

Suppose now that Alice wants to send Bob a message. She first encodes her message as an integer $m$ between 0 and $p-1$. For example, we can use the same "base 26" strategy that we employed for RSA above. She then chooses a random integer $y$ between 0 and $p-1$ called the "ephemeral key." Alice will have to choose a different ephemeral key for every message she sends, but Bob does not have to know the value of this key beforehand! Alice computes $s = h^y \bmod{p}$, $c_1 = g^y \bmod{p}$, and $c_2 = ms \pmod{p}$. Note that she can compute $s$ and $c_1$ quickly using binary exponentiation! The pair $(c_1, c_2)$ is the ciphertext that she sends to Bob. 

To decrypt the ciphertext $(c_1, c_2)$, Bob first computes $c_1^x \bmod{p}$. Bob can do this quickly using binary exponentiation. Observe that
$$ c_1^x \equiv (g^y)^x = g^{xy} = (g^x)^y \equiv h^y \equiv s \bmod{p}. $$
In other words, Bob has now found the same value of $s$ that Alice had, even though he does not know the value of the ephemeral key $y$. He then computes an inverse mod $p$ of $c_1^x$ using the extended euclidean algorithm (which is also very efficient). He then computes the product
$$ c_2 (c_1^x)^{-1} \equiv c_2 s^{-1} \equiv (ms)s^{-1} \equiv m \cdot 1 = m, \pmod{p} $$
and he has recovered Alice's message $m$.

For example, suppose Bob picks the prime $p = 4115549$, and $g = 2$ is his primitive root. He then picks a random integer $x = 2634326$. He can then compute $h = g^x \bmod{p}$ quickly using binary exponentiation (eg, using the `power_mod` function in Sage); he finds $h = 1149114$, so the triple $(4115549, 2, 1149114)$ is his public key. He must keep $x = 2634326$ secret. 

Now suppose that Alice wants to send Bob the message `Hi Bob`. As we did earlier in the RSA section, she can convert this to the integer $m = 3340481$. She chooses an ephemeral key, say $y = 2775147$. She keeps this value of $y$ secret. She computes $s = h^y \bmod{p}$ using binary exponentiation and finds $s = 962840$. This $s$ too, she keeps secret. She also computes $c_1 = g^y \bmod{p}$ using binary exponentiation and finds $c_1 = 621674$, and finally she computes $c_2 = ms \bmod{p}$ and finds $c_2 = 1911501$. The pair $(c_1, c_2) = (621674, 1911501)$ is then the ciphertext that she sends Bob. 

Bob receives the pair $(c_1, c_2) = (621674, 1911501)$. He computes $c_1^x \bmod{p}$ using binary exponentiation and finds that it is $962840$, which is the same value that Alice had found for $s$. He computes an inverse mod $p$ (eg, using the Sage function `inverse_mod`) and finds $s^{-1} \equiv 2329074 \bmod{p = 4115549}$. He then computes
$$ c_2 s^{-1} \bmod{p} = 3340481, $$
and he finally converts this back to the text `HIBOB`. 

<div class="element">
<span class="label">Exercise</span>
Suppose Bob picks the prime $p = 29$ and the primitive root $g = 2$.

a. Suppose Bob picks $x = 3$. What is his public key? 
b. Suppose Alice wants to send Bob the plaintext integer $m = 7$. What is the corresponding ciphertext pair if the ephemeral key she chooses is $y = 11$? 
c. Suppose Bob receives the ciphertext pair $(3, 9)$ from Alice. What is the plaintext integer $m$?   
</div>

<div class="element">
<span class="label">Exercise</span>
If Bob wants to be able to receive messages with $r = 10$ characters, how large must he choose $p$ to be? What if $r = 100$? $r = 1000$? 
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Write a pair of SageCells analogous to the [RSA SageCells](#sagecell-rsa): one should generate an Elgamal key, and the second should perform Elgamal encryption and decryption. 
</div>

### Why Elgamal is Probably Secure (for now...)

There are at least two strategies that Eve might employ to recover the plaintext $m$ from the ciphertext $(c_1, c_2)$. She might try to find Bob's decryption key $x$ so that she can follow Bob's decryption strategy, but to do this, she needs to find the discrete log base $g$ of $h$ mod $p$. Alternatively, she might try to find Alice's ephemeral key $y$ (because she can then compute $h^y$, and then $(h^y)^{-1} \equiv s^{-1} \pmod{p}$, and then $c_2(h^y)^{-1} \equiv (ms)s^{-1} \equiv m \pmod{p}$), but then she needs to find the discrete log base $g$ of $c_1$ mod $p$. 

In other words, it would seem that in either case, Eve needs to find a discrete log base $g$ mod $p$. The security of the Elgamal cryptosystem thus relies on the presumed difficulty of the following problem: 

<div class="element">
<span class="label">Discrete Logarithm Problem</span>
Suppose you are given a prime $p$, a primitive root $g$ mod $p$, and an integer $a$ not divisible by $p$. Find the discrete log base $g$ of $a$ mod $p$. In other words, find the unique integer $k$ such that $0 \leq k \leq p-1$ such that $g^k \equiv a \pmod{p}$.  
</div>

As $p$ gets large, this problem seems to get intractible for classical computers. The naive method to solving this problem would be to try all the possible values of $k$ from 1 up to $p-1$, but this is at least linear in $p$, which means it is at least exponential in the number of digits of $p$. There are slightly faster algorithms than this, and these faster methods are implemented by Sage for its discrete log functionality, but they are not faster by much (more precisely, there is no known algorithm that accomplishes this task that is polynomial in the number of digits of $p$). 

<div class="element">
<span class="label">Sage Exercise</span>
Read about Shank's Baby-Step Giant-Step Algorithm for computing discrete logarithms. Then implement the algorithm yourself.  
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Read about Pollard's Kangaroo Algorithm (also called "Pollard's Lambda Algorithm") for computing discrete logarithms. Then implement the algorithm yourself. 
</div>


