<div class="element">
<span class="label">Exercise</span>
Suppose it is 11:10am. 

a. Your friend Alice asks you if you want to get lunch together in 2 hours. What time is she proposing having lunch?
b. Your friend Bob tells you that he just woke up 15 minutes ago. What time did he wake up?
</div>

If you can do this exercise, you already know modular arithmetic at an intuitive level! What we're going to do here is introduce some notation to be able to talk about this "clock arithmetic" that you all already know in a rigorous way. 

### Quotients and Remainders {#division}

When an integer is divided by a positive integer, we get a quotient and a remainder. For example, when 17 is divided by 5, we get a quotient of 3 and a remainder of 2. The formal statement is the following: 

<div class="element">
<span class="label">Euclid's Division Lemma</span>
For any integer $a$ and positive integer $n$, there exists a unique pair of integers $q$ and $r$ such that $0 \leq r < n$ and $a = qn + r$. The integers $q$ and $r$ are called the *quotient* and *remainder*, respectively. We also write $a \bmod{n}$ to refer to the remainder. 
</div>

The basic idea of the proof for the existence statement is that you keep subtracting (or adding) $n$ from $a$ until you wind up in the range $[0, n)$. The number of times you had to subtract (or add) $n$ is the quotient, and the number in the range $[0, n)$ that you wind up with at the end is your remainder. 

This is likely to be very familiar to you when $a \geq 0$. Let's consider again the example of dividing $a = 17$ by $n = 5$. Subtracting $5$ once yields $12$, twice yields $7$, and thrice yields $2$. Since we've reached a number in the range $[0, 5)$, we stop subtracting. The number $2$ that we ended up with is the remainder, and since we had to subtract $5$ thrice, the quotient is $3$. 

This may be less familiar to you when $a < 0$, but it works the same way; we just have to add instead! Suppose, for example, that we want to divide $a = -7$ by $n = 5$. Adding $5$ once gives us $-2$ and twice gives us $3$. Thus the quotient is $-2$ (negative because we had *add* 5, instead of subtracting $5$ like we did above), and the remainder is $3$. 

<div class="element">
<span class="label">Exercise</span> For each of the following, calculate the quotient and remainder when $a$ is divided by $n$. Do these calculations by hand. 

a. $a = 13, n = 3$
b. $a = 134, n = 10$
c. $a = -37, n = 10$
d. $a = -15, n = 60$
e. $a = 13, n = 12$ 
</div>

<div class="element">
<span class="label">SageCell</span>
You can calculate quotients in Sage using the `//` operator: 
<div class="sage">
<script type="text/x-sage">
17 // 5
</script>
</div>
And you can calculate remainders using the `%` operator:
<div class="sage">
<script type="text/x-sage">
17 % 5
</script>
</div>
</div>

Suppose $a$ and $n$ are integers and $n > 0$. All of the following sentences mean *exactly* the same thing: 

* $a \bmod{n} = 0$.
* There is no remainder when $a$ is divided by $n$. 
* $a$ is a multiple of $n$. 
* $a$ is divisible by $n$. 
* $n$ is a divisor of $a$. 
* $n$ is a factor of $a$. 
* $n$ divides $a$ (in notation: $n \mid a$, where the vertical bar is read as "divides"). 
* $a/n$ is an integer. 

You're probably already familiar with some of these, but it will be beneficial for you to become comfortable with all of them. It'll feel strange at first that we have so many ways of saying exactly the same thing, but it's surprisingly useful. 

### Congruences {#congruences}

<div class="element">
<span class="label">Definition</span>
Fix a positive integer $n$. If $a$ and $b$ are integers, we say that "$a$ is congruent to $b$ mod $n$," or that "$a$ and $b$ are congruent mod $n$," if $a$ and $b$ have the same remainder when each is divided by $n$. This can be denoted in symbols as follows: 

$$ a \equiv b \pmod{n} $$
</div>

For example, we have $19 \equiv 7 \pmod{4}$ since $19$ and $7$ both have remainder $3$ when divded by $4$. Observe also that $19 - 7 = 12$ is a multiple of 4. This observation is generalized by the following: 

<div class="element">
<span class="label">Lemma</span> Fix a positive integer $n$. Two integers $a$ and $b$ are congruent mod $n$ if and only if $a-b$ is a multiple of $n$. 
</div>

*Proof of lemma*. Divide $a$ and $b$ by $n$ to write $a = q_1n + r_1$ and $b = q_2n + r_2$. If $a \equiv b \pmod{n}$, this by definition means that $r_1 = r_2$, so
$$ a-b = (q_1n + r_1) - (q_2n + r_2) = q_1n - q_2n = n(q_1 - q_2), $$
so $a-b$ is a multiple of $n$. Conversely, suppose $a-b$ is a multiple of $n$. Then
$$ (a-b) - (q_1 - q_2)n = ((q_1n + r_1) - (q_2 n + r_2)) - (q_1 - q_2)n = r_1 - r_2 $$
is a multiple of $n$ also. But $0 \leq r_1, r_2 < n$, so we must have $|r_1 - r_2| < n$ and the only way that $r_1 - r_2$ can be a multiple of $n$ is if $r_1 - r_2 = 0$, ie, if $r_1 = r_2$. This means that $a \equiv b \pmod{n}$. <span style="float: right;">$\Box$</span>

Here's a crucial property of congruences that we'll find ourselves using frequently. 

<div class="element">
<span class="label">Modular Arithmetic Theorem</span> Fix a positive integer $n$. Suppose $a, a', b, b'$ are integers such that $a \equiv a' \bmod{n}$ and $b \equiv b' \bmod{n}$, and $k$ is any positive integer. Then all of the following are also true: 
$$ \begin{aligned}
a + b &\equiv a' + b' &&\pmod{n} \\
a - b &\equiv a' - b' &&\pmod{n} \\
ab &\equiv a'b' &&\pmod{n} \\
a^k &\equiv (a')^k &&\pmod{n}
\end{aligned} $$
</div>

*Proof*. Let $r$ and $s$ be integers such that $a - a' = nr$ and $b - b' = ns$. Then $$(a + b) - (a' + b') = (a - a') + (b - b') = nr + ns = n(r+s)$$ is a multiple of $n$, so $a+b \equiv a' + b' \pmod{n}$. The proof for subtraction is almost identical and is omitted. For multiplication, observe that 
$$ ab - a'b' = ab - ab' + ab' - a'b' = a(b-b') + (a-a')b' = ans + nrb' = n(as + rb') $$
is also a multiple of $n$, so $ab \equiv a'b' \pmod{n}$. The final statement regarding exponentiation follows by inducting on the statement about multiplication. <span style="float: right;">$\Box$</span>

You may notice that division is missing in this theorem. This is because division in modular arithmetic is a little subtle; we'll return to this soon. 

The upshot of this theorem is that, if we're just interested in remainders after dividing by $n$, we can "take remainders mod $n$" at every step of our addition/subtraction/multiplication to make our calculations easier. For example, suppose we want to calculate $123 \cdot 24 \bmod{10}$. We could do this by calculating $123 \cdot 24 = 2952$ and then taking a remainder to get 2, but that multiplication is not so easy to do in your head. It is easier to observe that $123 \equiv 3 \pmod{10}$, and $24 \equiv 4 \pmod{10}$, so the theorem tells us that 
$$ 123 \cdot 24 \equiv 3 \cdot 4 = 12 \equiv 2 \pmod{10}. $$
In other words, we have reached the same answer *without* having to multiply large numbers together! 

<div class="element">
<span class="label">Exercise</span>

Use the Modular Arithmetic Theorem to quickly calculate the following. 

a. $417 \cdot 22 \bmod{10}$. 
b. $333333 + 666 \bmod{3}$. 
c. $7^{202320232023} \bmod{6}$.

</div>

<div class="element">
<span class="label">Exercise</span>
Fix positive integers $k$ and $n$. Suppose $a$ and $a'$ are integers such that $a \equiv a' \pmod{n}$. It is not true in general that $k^a \equiv k^{a'} \pmod{n}$. Show this by example: in other words, find $k$, $n$, $a$, and $a'$ such that $a \equiv a' \pmod{n}$, but $k^a \not\equiv k^{a'} \pmod{n}$. 
</div>

<div class="element">
<span class="label">Harder Exercise</span>
Suppose that the number $273x49y5$, where $x$ and $y$ are unknown digits, is divisible by 495. Find $x$ and $y$. 
</div>

### Caesar Cipher Revisited {#caesar-revisited}

Imagine that we want to apply the Caesar cipher with a shift of 5 to encrypt the letter `Y`. Before we see how the Caesar cipher relates to modular arithmetic, convince yourself that the encryption of `Y` is the letter `D`. 

Suppose we identify the letters `A` through `Z` with the numbers 0 through 25, in order. So, for example, `A` corresponds to 0, `B` corresponds to 1, and so forth. The letter `Y` that we are hoping to encrypt corresponds to 24. Now consider the function
$$ E(x) = (x+5) \bmod{26}. $$
Then $E(24) = (24+5) \bmod{26} = 29 \bmod{26} = 3$, which corresponds precisely to the letter `D`. In other words, if we identify letters with numbers, the function $E$ is precisely the encryption function of the Caesar cipher. As you might expect, the decryption function is
$$ D(y) = (y-5) \bmod{26}. $$
For example, to decrypt the letter `D`, we look at the corresponding number 3, and $D(3) = (3-5) \bmod{26} = -2 \bmod{26} = 24$, which corresponds to `Y`. 

The fact that adding 5 mod 26 and subtracting 5 mod 26 are are inverse operations might seem obvious, but it's worth noting that it is actually a consequence of the Modular Arithmetic Theorem. Set $y = E(x)$ and observe that $y \equiv x+5 \pmod{26}$ and that $D(y) \equiv y-5 \bmod{26}$. Thus 
$$ \begin{aligned}
D(E(x)) &= D(y) \\
&\equiv (y - 5) \pmod{26} \\
&\equiv ((x+5)-5) \pmod{26} \\
&= x.
\end{aligned} $$
We have used the Modular Arithmetic Theorem for the second-to-last step. There is, of course, nothing special about the shift 5 in this argument; any other shift works exactly the same way.  

The upshot is that the Caesar cipher "is" just modular arithmetic, and computers can do modular arithmetic very quickly. 

<div class="element" id="sagecell-caesar-cipher">
<span class="label">SageCell</span>
Your friend Alice has just sent you the following message, encrypted using the Caesar cipher with a shift of 7: `TLLAHANYHMMPAPOHSSHATPKUPNOA`. Here is some code that implements the Caeser cipher; hit "Run." What is the plaintext message? 
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

# Encrypt a string using the given shift    
def encrypt(text: str, shift: int):
    nums = numerify(text)
    transformed = [(x + shift) % 26 for x in nums]
    return denumerify(transformed)

# Decrypt a string using the given shift  
def decrypt(text: str, shift: int):
    if not text.isalpha():
        raise Exception("Argument to decrypt must consist of alphabet characters.")
    nums = numerify(text.upper())
    transformed = [(x - shift) % 26 for x in nums]
    return denumerify(transformed)
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(shift=slider(0, 25, 1, 7, label="Shift"),
      text=input_box(default="TLLAHANYHMMPAPOHSSHATPKUPNOA", label="Input", height=5, width=80),
      actions=selector(["decrypt", "encrypt"], buttons=True, label="Action")):
    output = eval(actions)(text, shift)
    output_div("Output", f'<textarea readonly rows="5" cols="80">{ output }</textarea>')
</script>
</div>
Once you've decrypted the message, play with the SageCell! You can use it to encrypt as well as decrypt messages using the Caesar cipher with any shift. What is the plaintext corresponding to `VSSWIZIPX` with a shift of 4? What is the ciphertext for the message `Muir College` with a shift of 2? 
</div>

At this point, we're also ready to *start* thinking about codebreaking for a Caesar cipher. We'll develop more machinery later on that'll help us think more deeply and systemtically about this. 

<div class="element">
<span class="label">Exercise</span>
You have just intercepted the following ciphertext that was sent out to a politically active student group on campus: 

```
KLGJELZWJWYWFLKEWWLAFYSFVVWESFVVWUSJTGFARSLAGF
```

You know that the message was encrypted using a Caesar cipher, but you don't know the shift. What is the plaintext? *Hint*. There are "only" 26 possible shifts to try...!
</div>
