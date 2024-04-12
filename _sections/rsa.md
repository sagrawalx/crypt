RSA is a cryptosystem named after [Ron Rivest](https://en.wikipedia.org/wiki/Ron_Rivest), [Adi Shamir](https://en.wikipedia.org/wiki/Adi_Shamir), and [Leonard Adleman](https://en.wikipedia.org/wiki/Leonard_Adleman), who publicly described the algorithm in 1977. The GCHQ mathematician [Clifford Cocks](https://en.wikipedia.org/wiki/Clifford_Cocks) developed an equivalent system back in 1973, but his work was not declassified until 1997. 

Let's start by discussing how RSA works. We'll then talk about why it works, and finally about why it's secure (or at least, probably secure for now). 

### How to Convert Text Messages to Numbers

We first need a preliminary discussion about how to convert text messages into integers, since RSA only allows us to transfer integers. A variety of methods could be employed to make this happen, but one simple idea is for her to encode a message in the usual way (remove all non-alphabet characters and capitalize everything), and regard the resulting string as a number in base 26. 

Suppose, for example, that Alice's message is `HIBOB`. Under the usual letter-to-number correspondence (`A` is 0, `B` is 1, etc), these numbers correspond to the numbers 7, 8, 1, 14, 1 (in that order). We can then construct the integer
$$ 1 \cdot 26^0 + 14 \cdot 26^1 + 1 \cdot 26^2 + 8 \cdot 26^3 + 7 \cdot 26^4 = 3340481. $$
This is an integer representation of the message `HIBOB` in the sense that there is a straightforward algorithm to recover the plaintext: we use the same algorithm that we used to write a number in binary, but now we divide repeatedly by 26 instead. 
$$ \begin{aligned}
3340481 &= 128480 \cdot 26 + 1 \\
128480 &= 4941 \cdot 26 + 14 \\
4941 &= 190 \cdot 26 + 1 \\
190 &= 7 \cdot 26 + 8 \\
7 &= 0 \cdot 26 + 7
\end{aligned} $$
We then look at the sequence of remainders, starting from the bottom (ie, 7, 8, 1, 14, 1), and associate to each of these remainders the corresponding letters to get back to the plaintext: `HIBOB`. 

<div class="element">
<span class="label">Exercise</span>

a. Find the integer representation of the text `GAIA`. 
b. Find the text corresponding to the integer 245405438. 
</div>

<div class="element" id="sagecell-integer-representations">
<span class="label">SageCell</span>
Here is a SageCell that implements the above described integer representations of text, and also the reverse. 
<div class="sage">
<script type="text/x-sage">
from re import sub

# Remove all non alphabetic characters and capitalize
def encode(text: str):
    stripped = sub(r"[^a-zA-Z]", "", text)
    return stripped.upper()

# Encode a string as a list of numbers 0--25
def numerify(text: str):
    return [(ord(x) - 65) for x in encode(text)]

# Turn a list of numbers 0--25 back into a string
def denumerify(nums: list):
    return "".join([chr((x % 26) + 65) for x in nums])
    
# Get integer representation
def integerify(text: str):
    n = 0
    for i, x in enumerate(reversed(numerify(text))):
        n += x * 26^i
    return n
    
# Get text from integer representation
def deintegerify(n: int):
    n = int(n)
    rems = []
    while n > 0:
        rems.append(n % 26)
        n = n // 26
    return denumerify(reversed(rems))
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(text=input_box(default="HIBOB", label="Input", height=2, width=70),
      actions=selector(["integerify", "deintegerify"], buttons=True, label="Action")):
    output = eval(actions)(text)
    output_div("Output", f'<textarea readonly rows="2" cols="70">{ output }</textarea>')
</script>
</div>
</div>

<div class="element">
<span class="label">Exercise</span>
Let's say you wanted to preserve spaces in your message. How would you modify the above method of associating an integer to text to make this happen? 
</div>

<div class="element">
<span class="label">Exercise</span>
There's a problem with the base 26 encoding/decoding scheme described above! Specifically, the problem arises with words like `APPLE` or `AARDVARK`. 

a. What is the problem? 
b. How would you go about modifying this scheme to avoid this problem? (There are many possible solutions, including "do nothing"...! What's your favorite solution, and why?)
</div>

### How RSA Works

Bob starts off by picking two distinct large primes $p$ and $q$. He computes $n = pq$ and $\phi(n) = (p-1)(q-1)$. He picks a random integer $d$ between 0 and $\phi(n)$ such that $\gcd(d, \phi(n)) = 1$. He then computes $e \equiv d^{-1} \pmod{\phi(n)}$ (eg, using the extended Euclidean algorithm). He then publishes the pair $(n, e)$ for the world to see; this is his *public encryption key*. He keeps the remaining numbers private. 

Suppose Alice has a secret integer $m$ between 0 and $n-1$ that she wants to send to Bob. She encrypts $m$ by computing $c = m^e \bmod{n}$, and this is the ciphertext that she sends to Bob. 

When Bob receives $c$, he computes $c^d \bmod{n}$. As it turns out, this must be $m$, so he has received Alice's message!

<div class="element">
<span class="label">Exercise</span>
Suppose Bob picks the primes $p = 3$ and $q = 5$. These are not even remotely large enough primes to be secure, but it's useful to work through small examples to understand the generality described above. We have $n = pq = 15$ and $\phi(n) = (p-1)(q-1) = 8$. Suppose Bob further chooses $d = 3$ (which is in fact relatively prime to 8). 

a. What is Bob's public encryption key?
b. Suppose Alice wants to send Bob the message $m = 7$. What is the ciphertext $c$ that Alice sends Bob? 
c. Check that, if Alice sends the ciphertext $c$ corresponding to $m = 7$ to Bob, that Bob actually recovers the original plaintext. 
d. Suppose Alice sends the ciphertext $c = 2$ to Bob. What is the corresponding plaintext? 
</div>

<div class="element">
<span class="label">Exercise</span>
Let's now take Eve's perspective to see why choosing large primes is crucial. Suppose Bob's RSA public key is $(35, 7)$ and Alice has just sent Bob the ciphertext $c = 17$. What is Bob's decryption key? What is Alice's plaintext message?
</div>

<div class="element">
<span class="label">Exercise</span>

a. Suppose Bob's public key has $n = 3233$. What is the maximum integer $n$ such that Alice can send Bob *any* message of length $n$?

b. Suppose Bob wants to be able to receive *any* message with 100 characters. How big does $n$ have to be?
</div>

<div class="element" id="sagecell-rsa">
<span class="label">SageCell</span>
Here are two SageCells that together implement RSA. 

The first cell generates RSA keys. You will input a number of bits; it will then generate two primes with that many bits in their binary representation, multiply them together to get $n$, and then generate $d$ and $e$ as described above. The larger you choose the number of bits, the more secure the key is. Remember that $(n, e)$ is the public key, and that $d$ must be kept secret. 
<div class="sage">
<script type="text/x-sage">

# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))
    
@interact
def _(bits=input_box(default="10", label="Bits", width=62)):
    p = random_prime(2^bits-1, lbound=2^(bits-1))
    q = random_prime(2^bits-1, lbound=2^(bits-1))
    n = p*q
    phi_n = (p-1)*(q-1)
    d = randint(0, phi_n)
    while gcd(d, phi_n) != 1:
        d = randint(0, phi_n)
    e = inverse_mod(d, phi_n)
    
    output_div("n", str(n))
    output_div("e", str(e))
    output_div("d", str(d))

</script>
</div>

The next cell implements the encryption and decryption. It defaults to using a key that was generated using the above SageCell with 100-bit primes. Encryption does not require you to enter a value for `d`, but decryption does. 
 
<div class="sage">
<script type="text/x-sage">
from re import sub

# Remove all non alphabetic characters and capitalize
def encode(text: str):
    stripped = sub(r"[^a-zA-Z]", "", text)
    return stripped.upper()

# Encode a string as a list of numbers 0--25
def numerify(text: str):
    return [(ord(x) - 65) for x in encode(text)]

# Turn a list of numbers 0--25 back into a string
def denumerify(nums: list):
    return "".join([chr((x % 26) + 65) for x in nums])
    
# Get integer representation
def integerify(text: str):
    n = 0
    for i, x in enumerate(reversed(numerify(text))):
        n += x * 26^i
    return n
    
# Get text from integer representation
def deintegerify(n: int):
    n = int(n)
    rems = []
    while n > 0:
        rems.append(n % 26)
        n = n // 26
    return denumerify(reversed(rems))
    
# Encrypt plaintext
def encrypt(text: str, n: int, e: int):
    return power_mod(integerify(text), e, n)
    
# Decrypt ciphertext
def decrypt(c: int, n: int, d: int):
    return deintegerify(power_mod(c, d, n))
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(n=input_box(default="717820673049189281018479026338616874477777570300534678926289", label="n", width=62),
      e=input_box(default="214320273459987081422530851104153021999292494860068474396359", label="e", width=62),
      d=input_box(default="637365142515768052967427740225606348702321505441761588369459", label="d", width=62),
      text=input_box(default="Hi Bob!", label="Input", height=2, width=70),
      actions=selector(["encrypt", "decrypt"], buttons=True, label="Action")):
    if actions == "encrypt":
        output = encrypt(text, n, e)
    else:
        try:
            d = int(d)
            output = decrypt(int(text), n, d)
        except: 
            output = "Decryption key required for decryption!"
        output = decrypt(int(text), n, d)
    output_div("Output", f'<textarea readonly rows="2" cols="70">{ output }</textarea>')
</script>
</div>
</div>

### Why RSA Works

<div class="element">
<span class="label">RSA Theorem</span>
Suppose $p, q$ are distinct primes and $n = pq$, that $d$ is an integer with $1 \leq d \leq \phi(n)$ and $\gcd(d, \phi(n)) = 1$, and that $e \equiv d^{-1} \pmod{\phi(n)}$. If $0 \leq m \leq n-1$ and $c = m^e \bmod{n}$, then
$$ c^d \bmod{n} = m. $$
</div>

*Proof*. Observe that $de \equiv 1 \pmod{\phi(n)}$, so there exists an integer $k$ such that $de = 1 + k\phi(n)$ and we have
$$ c^d \equiv (m^e)^d = m^{ed} = m^{1 + k\phi(n)} \pmod{n}. $$
Since $0 \leq m < n$, it is therefore sufficient for us to show that $m^{1 + k\phi(n)} \equiv m \pmod{n}$. 

There are two cases. The first case is when $\gcd(m, n) = 1$. Then Euler's theorem says that $m^{\phi(n)} \equiv 1 \pmod{n}$, so
$$ \begin{aligned}
m^{1 + k\phi(n)} = m \cdot (m^{\phi(n)})^k \equiv m \cdot 1^k = m \pmod{n}, \end{aligned} $$
which is what we wanted to show. 

The second case is when $\gcd(m, n) \neq 1$. Since $n = pq$ and $0 \leq m < n$, it must be that $\gcd(m, n) = p$ or $\gcd(m, n) = q$. Without loss of generality, suppose $\gcd(m, n) = p$. This means that $m = xp$ for some integer $x$ and also that $m$ is *not* divisible by $q$. Euler's theorem tells us that $m^{q-1} \equiv 1 \pmod{q}$, so 
$$ m^{k\phi(n)} = m^{k(p-1)(q-1)} = (m^{q-1})^{k(p-1)} \equiv 1^{k(p-1)} = 1 \pmod{q} $$
so there exists an integer $h$ such that $m^{k\phi(n)} = 1 + hq$. Then 
$$ m^{1 + k\phi(n)} = m(1 + hq) = m + hmq = m + hxpq = m + hxn \equiv m \pmod{n}, $$
which again is what we wanted to show! <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Proof Exercise</span>
Show that the RSA theorem continues to hold when $n$ is a product of arbitrarily many distinct primes (ie, not just 2 distinct primes). 
</div>

<div class="element">
<span class="label">Exercise</span>
Give an example to show that the RSA theorem does *not* hold when $n$ is *not* a product of two distinct primes. 

*Hints*. In other words, you want to find an integer $n$, and an integer $d$ which is relatively prime to $\phi(n)$ with inverse $e$, and an integer $m$ with $0 \leq m \leq n-1$ such that $(m^e)^d \not\equiv m \pmod{n}$. The RSA theorem tells us that $n$ cannot be a product of two distinct primes, and in fact if you did the above proof exercise, you know that it cannot be a product of any number of distinct primes, which means the prime factorization of $n$ must contain at least one exponent that's bigger than 1. Pick your favorite such value of $n$. Then use Euler's theorem to rule out many possibilities for $m$...
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose $n$ is a product of two distinct primes, that $e$ is invertible mod $\phi(n)$, and that $c$ is an $e$th power mod $n$. Show that $c$ has a *unique* $e$th root mod $n$. 
</div>

### Why RSA is Probably Secure (for now...)

If Eve is eavesdropping on Alice and Bob's communication, she knows Bob's public key $(n, e)$ and she sees Alice's ciphertext $c$. She knows that $c$ is the $e$th power mod $n$ of Alice's original message $m$, so the security of RSA relies on the presumed difficulty of the following problem: 

<div class="element">
<span class="label">Discrete Root Problem</span>
Suppose you are given positive integers $n$, $e$, and $c$, and you know further that: 

* $n$ is a product of two distinct primes (but you do not know which ones),
* $e$ is invertible mod $\phi(n)$ (but you do not know $\phi(n)$), and
* $c$ is an $e$th power mod $n$.

Find the unique $e$th root of $c$ mod $n$, ie, the unique integer $m$ such that $0 \leq m \leq n-1$ such that $m^e \equiv c \bmod{n}$. 
</div>

Most likely, there is no *fast* way of doing this --- except for the way that Bob uses, which requires some secret knowledge! Bob needs to know the decryption exponent $d$, which is an inverse of $e$ mod $\phi(n)$, and knowing that would seem to require knowing $\phi(n)$.[^know-totient] The following lemma shows that knowledge of $\phi(n)$ is actually equivalent to a knowledge of a factorization of $n$, which is believed to be hard to find quickly! 

[^know-totient]: I do not know if it is known how to prove that finding $d$ is computationally equivalent to finding $\phi(n)$, or if this is an open problem. I would be grateful if someone could tell me!

<div class="element">
<span class="label">Lemma</span>
Suppose $p, q$ are distinct primes and $n = pq$. If Eve knows $n$ and can quickly calculate $\phi(n)$, then she can also quickly find $p$ and $q$. 
</div>

*Proof*. Observe that
$$ \phi(n) = (p-1)(q-1) = pq - p - q + 1 = n - p - q + 1, $$
which means that
$$ n+1-\phi(n) = p + q, $$
which means that
$$ (x-p)(x-q) = x^2 - (p+q)x + pq = x^2 - (n+1-\phi(n))x + n. $$
In other words, $p$ and $q$ are the two roots of the quadratic polynomial $x^2 - (n+1-\phi(n))x + n$, so if Eve knows how to quickly find $\phi(n)$, she can then just apply the quadratic formula to this polynomial to factor $n$ and find $p$ and $q$. <span style="float: right;">$\Box$</span>

<div class="element">
<span class="label">Exercise</span>
Suppose $p$ and $q$ are distinct primes and $n = pq$. Use the quadratic formula to write down an explicit formula for $p$ and $q$ in terms of $n$ and $\phi(n)$. 
</div>

In other words, the lemma tells us that, since it is believed that there is no fast factoring algorithm for classical (ie, non-quantum) computers, this tells us that Eve probably does not have a quick way of finding the decryption exponent $d$ in the same way that Bob does. 

<div class="element">
<span class="label">Exercise</span>
Bob's public key is $(n, e)$ where 
$$ \begin{aligned}
n &= 717426848223284193177165144369883618288687316631460708260937 \\
e &= 31883625115837871682689851305708348256252087949589953972423. 
\end{aligned} $$
Unfortunately for Bob, he has chosen his public key to be too small: this $n$ is small enough that it can be factored by modern computers in a few seconds! Eve has just intercepted the ciphertext
$$ c = 35831783079824311094981278118129411294275821240128031455119 $$
that Alice was trying to send to Bob. Find the plaintext (as text, not just as a number).  
</div>

<div class="element">
<span class="label">Exercise</span>
Do one (or more!) of the following: 

a. Play the role of Bob: Generate a RSA public key for yourself using the [RSA SageCells](#sagecell-rsa) and post it to our class stream on Zulip.
b. Play the role of Alice: Watch for a classmate who has posted their RSA public key to Zulip. Send them a secret message by encrypting it with their public key using the [RSA SageCells](#sagecell-rsa).
c. Play the role of Eve: Watch for RSA ciphertext being exchanged by some pair of your classmates on Zulip. See if you can figure out what the plaintext is!
</div>

<div class="element">
<span class="label">Exercise</span>
Bob chooses a large and secure RSA key. Suppose that Alice encodes each letter in her message as an integer 0--25 and then encrypts each of these integers individually using Bob's public key (instead of encoding her entire message using the base-26 encoding scheme described above). She then sends that entire sequence of ciphertexts over to Bob. 

Explain why this is very insecure. In other words, suppose Eve intercepts the sequence of ciphertexts (each corresponding to a single letter in her message) that Alice sends to Bob. Explain how Eve will be able to recover Alice's message even if the modulus of Bob's RSA public key is too large to be factored. 
</div>

