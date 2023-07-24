There is also a variant of the Elgamal cryptosystem using elliptic curves that can be used to exchange messages, but there is a nontrivial encoding step. To make Elgamal work with elliptic curves, we first need a way to encode a plaintext message as a point on an elliptic curve $E$ mod $p$. 

There is a probabilistic algorithm for doing this. The rough idea is that we can try to encode plaintext as the $x$-coordinate of a point, but note that not every integer will occur as the $x$-coordinate of a point on an elliptic curve mod $p$! Specifically, if $E$ is given by $y^2 = x^3 + ax + b$ and if $P = (x, y)$ is a point on the curve, then the $x$-coordinate must have the property that $x^3 + ax + b$ is a quadratic residue mod $p$. 

### Entire Process

Let us start by describing the Elgamal process in its entirety and in the abstract.  

Suppose that Bob would like to be able to receive messages of length $N$. Bob will fix a positive integer $s$. We will see that, the larger that Bob chooses this integer, the higher the probability that encoding will succeed. Bob will then choose a prime $p > s26^N$, and an elliptic curve $E$ mod $p$ (defined by integers $a, b$ such that the integral Weierstrass equation $y^2 = x^3 + ax + b$ is nonsingular mod $p$), and a point $P \in E$. He computes $\mathrm{ord}_E(P)$. Then Bob chooses a secret integer $0 \leq k < \mathrm{ord}_E(P)$ to serve as his private key. He computes $Q = kP$, and this value is part of his public key. In other words, Bob will share all of the data $(s, E, P, \mathrm{ord}_E(P), Q)$ publicly, and keep only the integer $k$ secret.

Suppose now that Alice wants to send Bob a message. She first converts her message into an integer $m$ using the same base 26 idea we used for RSA. She will then iterate through values of $r = 0, 1, 2, \cdots, s-1$ until she finds the first value of $x = sm + r$ such that
$$ \left( \frac{x^3 + ax + b}{p} \right) \neq -1. $$
Note that that the maximum possible value of $m$ is $26^N - 1$, so 
$$ x = sm + r < s(26^N-1) + s = s26^N < p, $$
since Bob chose $p$ to be larger than $s26^N$. There is a roughly 1/2 chance that an integer in $[0, p)$ is not a quadratic residue mod $p$, and here we are iterating through $s$ integers in the range $[0, p)$, so there is a $1/2^s$ chance that $x^3 + ax + b$ is not a quadratic residue for any of the $s$ possible values of $x = sm + r$. If none of the $s$ integers are quadratic residues, encoding fails --- but if Bob chose $s$ to be even moderately large, encoding failure is *very unlikely*! If encoding does fail, Alice can just modify her message slightly (rephrase slightly? add a nonsense letter?) and try encoding again. Once Alice is able to find a value of $x$ such that $x^3 + ax + b$ is a quadratic residue mod $p$, then there is a integer $y$ such that $y^2 \equiv x^3 + ax + b \pmod{p}$, so the point $M = (x, y)$ is on $E$. This will be the encoding of her plaintext message. 

This is not yet the ciphertext, but at this point she can encrypt the encoded message using a process very similar to the Elgamal cryptosystem we discussed earlier. Alice chooses an "ephemeral key" $h$ such that $0 \leq h < \mathrm{ord}_E(P)$. She computes $S = hQ$, $R_1 = hP$, and $R_2 = M + S$. The pair $(R_1, R_2)$ is the ciphertext that she sends to Bob. 

To decrypt the ciphertext $(R_1, R_2)$, Bob uses his private key $k$ to compute $S = kR_1$. Observe that
$$ kR_1 = k(hP) = khP = h(kP) = hQ, $$
so Bob has found the same value of $S$ that Alice had, even though he does not know the value of the ephemeral key $h$. He can then compute $-S$ by negating the $y$-coordinate, and he then calculates 
$$ R_2 - S = R_2 + (-S) = (M + S) + (-S) = M + (S + (-S)) = M + O = M. $$
He has thus recovered Alice's encoded plaintext! 

Finally, Bob just needs to decode $M$. If $M = (x, y)$, he can extract the first coordinate $x$. The quotient when he divides this by $s$ is the integer $m$ that represents the message in base 26, so he then finishes off by converting back to text using the same process we used for RSA above. 

Whew!

### Encoding and Decoding

Here are some exercises to help you understand the above process by breaking it down and working with some pieces. First up are the encoding and decoding parts of the process (careful: not the encryption and decryption yet).  

<div class="element">
<span class="label">Exercise</span>
Suppose Bob's public key has $s = 2, p = 97, a = 31$, and $b = 20$. The elliptic curve $E$ is then the one given by $y^2 = x^3 + 31x + 20$ mod $p = 97$. 

a. What is the encoding of the plaintext message `B`? Follow the process above to find the corresponding point $M \in E$. 
b. Show that encoding fails for the letter `K`. 
c. Follow the process described above to find the plaintext message that results from decoding the point $(25, 30) \in E$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose Bob's public key has $p = 9393107$ and $s = 3$. 

a. What is the maximum length for messages that Bob can receive? 
b. What is the probability of encoding failure? 
</div>

<div class="element">
<span class="label">Exercise</span>
Bob would like to be able to receive messages of length $N = 50$ with an encoding failure rate of at most $0.1%$. What is the smallest prime $p$ that he can use for his public key? 

*Hint*. You should probably use Sage to help you do this calculation, and if you do, you might find it helpful to use the function `next_prime`, which tells you the smallest prime number greater than the input. 
</div>

<div class="element" id="sagecell-elliptic-curve-encoding">
<span class="label">SageCell</span>
Here is a SageCell that implements the encoding step described above. Note that it will output the point $M \in E$ with three coordinates where the third coordinate will always be 1 (for technical reasons); this is the same as what you observed in the previous elliptic curve SageCells, except that here commas are used to separate the three numbers instead of colons. 
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
    
# Get integer representation
def integerify(text: str):
    n = 0
    for i, x in enumerate(reversed(numerify(text))):
        n += x * 26^i
    return n

# Encode text as a point on an elliptic curve
def elliptic_encode(text: str, E, s):
    m = integerify(text)
    p = E.base_ring().cardinality()
    a = E.ainvs()[3]
    b = E.ainvs()[4]
    for r in range(s):
        x = Mod(s*m + r, p)
        f = x^3 + a*x + b
        if kronecker_symbol(f, p) != -1:
            return E(x, Mod(f, p).sqrt())
    raise Exception("Encoding fails")
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(text=input_box(default="HIBOB", label="Input", height=2, width=80),
      s=input_box(default="10", label="s", width=80),
      p=input_box(default="31624898780568028223033578567554929233454460200496038308094584218593743", label="p", width=80),
      a=input_box(default="2231", label="a", width=80),
      b=input_box(default="924384923849", label="b", width=80)):
    
    text = encode(text)
    o = f"Probability of encoding failure: {float(0.5^s)}"
    o += "<br/>"
    o += f"Maximum message length: {floor(log(p/s, 26))}"
    o += "<br/>"
    o += f"Input message length: {len(text)}"
    output_div("Remarks", o)
    
    E = EllipticCurve(GF(p), [a, b])  
    o = elliptic_encode(text, E, s)
    output_div("Output", f'<textarea readonly rows="2" cols="80">{ tuple(o) }</textarea>')
</script>
</div>
Here is another SageCell that implements the decoding step.
<div class="sage">
<script type="text/x-sage">
# Turn a list of numbers 0--25 back into a string
def denumerify(nums: list):
    return "".join([chr((x % 26) + 65) for x in nums])

# Get text from integer representation
def deintegerify(n: int):
    n = int(n)
    rems = []
    while n > 0:
        rems.append(n % 26)
        n = n // 26
    return denumerify(reversed(rems))

# Decode a point on an elliptic curve into plaintext
def elliptic_decode(M, E, s):
    return deintegerify(M[0] // s)
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(M=input_box(default="(33404810, 14631728016432515480233802166029066516512428400786216396603964460676414, 1)", label="Input", width=80),
      s=input_box(default="10", label="s", width=80),
      p=input_box(default="31624898780568028223033578567554929233454460200496038308094584218593743", label="p", width=80),
      a=input_box(default="2231", label="a", width=80),
      b=input_box(default="924384923849", label="b", width=80)):
    
    E = EllipticCurve(GF(p), [a, b])  
    M = E(M)
    o = elliptic_decode(M, E, s)
    output_div("Output", f'<textarea readonly rows="2" cols="80">{ o }</textarea>')
</script>
</div>
</div>

<div class="element">
<span class="label">Exercise</span>
Describe in your own words what goes wrong if Alice tries to encode a message that is too long (based on what Bob chose for his values of $p$ and $s$). You might find it helpful to experiment with the above SageCells. 
</div>

### Encryption and Decryption

Here is an example of the encryption and decryption process in a small example and using Sage. I encourage you to follow along and type things into a SageCell yourself as you read through this example (or you can do the calculations by hand, though this might get a little tiring even in this small example). 

When generating his elliptic curve Elgamal public key, Bob chooses the elliptic curve $E$ defined by $y^2 = x^3 + 3x + 4$ mod $p = 7$ and the point $P = (5, 5) \in E$. 

```sage
E = EllipticCurve(GF(7), [3, 4])
P = E(5, 5)
```

By running `P.order()`, we find that $\mathrm{ord}_E(P) = 10$. Suppose Bob chooses the secret integer $k = 3$. By typing `3*P` into Sage, he finds that $Q = kP = 3 \cdot (5, 5) = (2, 5)$. All parts of this calculation *except* the value of $k = 3$ are made public; $k$ is his private decryption key. 

Now suppose Alice wants to send Bob a message. She sets up her Sage session by pulling the following information from Bob's public key. 

```sage
E = EllipticCurve(GF(7), [3, 4])
P = E(5, 5)
order = 10
Q = E(2, 5)
```

Suppose that, following the encryption process described above, her encoded message is the point $M = (0, 5)$. She chooses an ephemeral key between $0$ and $\mathrm{ord}_E(P) = 10$, say $h = 4$. She must keep this value of $h$ secret. She computes $R_1 = hP$ by typing $4 * P$ into Sage and finds $R_1 = (0, 2)$. She then computes $R_2 = M + S = M + hQ$ by typing `E(0, 5) + 4*Q` into Sage and finds $R_2 = (1, 6)$. The pair $(R_1, R_2) = ((0, 2), (1, 6))$ is the ciphertext that she sends to Bob.

Let's now go backwards and check that Bob is able to recover Alice's message. He computes $M = R_2 - kR_1$ by typing `E(1, 6) - 3*E(0, 2)` and finds $M = (0, 5)$, which was Alice's encoded message!

<div class="element">
<span class="label">Exercise</span>
When generating his elliptic curve Elgamal public key, Bob chooses the elliptic curve $E$ defined by $y^2 = x^3 - 3x + 3$ mod $p = 11$ and the point $P = (10, 7) \in E$. 

a. What is $\mathrm{ord}_E(P)$?
b. Suppose Bob chooses $k = 4$. What is the value of $Q = kP$? 
c. Alice encodes some message to produce the point $M = (1, 1) \in E$. She also generates the ephemeral key $h = 2$. What is the ciphertext $(R_1, R_2)$ that she sends to Bob? 
d. Check that if Bob decrypts the ciphertext $(R_1, R_2)$ that he receives from Alice, he does in fact recover $M = (1, 1)$. 
e. Bob receives the message $((6, 5), (10, 4))$ from Alice. What is the encoded plaintext point $M \in E$? 

</div>

<div class="element">
<span class="label">Exercise</span>
Suppose Bob's elliptic curve Elgamal public key is as follows:
$$ \begin{aligned}
s &= 10 \\
p &= 965840826414842165347088832781 \\
E &: y^2 = x^3 + 2x + 1 \\
P &= (537016992844572701251197286080, 809430503509725487398100177667) \\
\mathrm{ord}_E(P) &= 965840826414840286830465288570 \\
Q &= (636209278006637725237624425202, 310653932799215077905485882454)
\end{aligned} $$

a. What is the probability that encoding fails? 
b. Alice wants to send Bob the message `I have turned into a cat`. What is the ciphertext she sends Bob? 
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose Bob's elliptic curve Elgamal public key is as follows:
$$ \begin{aligned}
s &= 10 \\
p &= 884300930387974803998998733399 \\
E &: y^2 = x^3 + 2x + 1 \\
P &= (361652577940189312498172547614, 514746227051815551959384860286) \\
\mathrm{ord}_E(P) &= 884300930387974691696035383315 \\
Q &= (882188314538809138823874338820, 772608417660306453415686049932)
\end{aligned} $$
His private key is
$$ k = 587487808581088164390024475576. $$
He receives the following ciphertext $(R_1, R_2)$ from Alice. 
$$ \begin{aligned}
R_1 &= (779503153810704949581662586128, 391600280839951295424353453750) \\
R_2 &= (590367533102870068877884575138, 460366500620283882415563572675)
\end{aligned} $$
What is the plaintext (as text, rather than as an encoded point)? 
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose Bob's elliptic curve Elgamal public key is as follows:
$$ \begin{aligned}
s &= 10 \\
p &= 34159136004208027161199 \\
E &: y^2 = x^3 + 2x + 1 \\
P &= (5205000772914715415725, 20236812690413582503099 \\
\mathrm{ord}_E(P) &= 34159136004127088328131 \\
Q &= (25509677592482725856096, 823756708348136845691)
\end{aligned} $$
Unfortunately, his public key is not large enough to be secure! What is his private decryption key $k$?
</div>

<div class="element" id="sagecell-elliptic-curve-elgamal">
<span class="label">SageCell</span>
Here is a SageCell that implements Elgamal encryption. When you hit "Run," you'll see the following input boxes: 

* `Input` for the input plaintext
* `s` for the integer $s$ that controls the encoding failure rate
* `p` for the prime mod which we'll consider our elliptic curve $E$
* `a` and `b` for the coefficients of the Weierstrass equation $y^2 = x^3 + ax + b$ of $E$
* `P` for the coordinates of a point on the curve
* `order` for $\mathrm{ord}_E(P)$
* `Q` for the coordinates of some multiple of `P`

The SageCell will then compute the corresponding ciphertext pair $(R_1, R_2)$ that Alice sends to Bob and display it in the `Output` box. Again, each point $R_1, R_2$ will be represented by three coordinates; the point $O$ will be represented by `(0, 1, 0)`, and a point $(x, y)$ will be represented by `(x, y, 1)`. Again, this is the same format for points that you are familiar with from the earlier elliptic curve SageCells, except that we use commas instead of colons to separate the numbers. 

Note that the encryption generates a random ephemeral key, so you'll see different ciphertexts every time you run this SageCell even if all of the input fields are exactly the same!

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
    
# Get integer representation
def integerify(text: str):
    n = 0
    for i, x in enumerate(reversed(numerify(text))):
        n += x * 26^i
    return n

# Encode text as a point on an elliptic curve
def elliptic_encode(text: str, s, E):
    m = integerify(text)
    p = E.base_ring().cardinality()
    a = E.ainvs()[3]
    b = E.ainvs()[4]
    for r in range(s):
        x = Mod(s*m + r, p)
        f = x^3 + a*x + b
        if kronecker_symbol(f, p) != -1:
            return E(x, Mod(f, p).sqrt())
    raise Exception("Encoding fails")

# Encrypt the encoded message M
def encrypt(M, E, P, order, Q):
    ephemeral = randint(0, order-1)
    return (ephemeral*P, M + ephemeral*Q)
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(text=input_box(default="HIBOB", label="Input", height=2, width=80),
      s=input_box(default="10", label="s", width=80),
      p=input_box(default="31624898780568028223033578567554929233454460200496038308094584218593743", label="p", width=80),
      a=input_box(default="2231", label="a", width=80),
      b=input_box(default="924384923849", label="b", width=80),
      P=input_box(default="(28242533037372562028376915025599704895624883370665290980560308139223847, 27339484572109674275112696180600875534462601488783406730716833705598007, 1)", label="P", width=80),
      order=input_box(default="31624898780568028223033578567554928906213834570791083268618301693807894", label="order", width=80),
      Q=input_box(default="(14860594925009170003954530097202645513024232456550100199567054270027585, 31448458171617918642007942988642343116727272631897730366311554311900365, 1)", label="Q", width=80)):
    
    text = encode(text)
    o = f"Probability of encoding failure: {float(0.5^s)}"
    o += "<br/>"
    o += f"Maximum message length: {floor(log(p/s, 26))}"
    o += "<br/>"
    o += f"Input message length: {len(text)}"
    output_div("Remarks", o)
    
    E = EllipticCurve(GF(p), [a, b])  
    P = E(P)
    Q = E(Q)
      
    M = elliptic_encode(text, s, E)
    R = encrypt(M, E, P, order, Q)
    o = (tuple(R[0]), tuple(R[1]))
    
    output_div("Output", f'<textarea readonly rows="2" cols="80">{ o }</textarea>')
</script>
</div>

The next SageCell implements decryption. You'll see a lot of the same fields as above, except now:

* `Input` should be ciphertext; in other words, it is a pair of points on the elliptic curve $E$, in the same format as the output of the above encryption SageCell
* `k` is the decryption key, which must be specified

The `Output` is the corresponding plaintext. 

<div class="sage">
<script type="text/x-sage">
# Turn a list of numbers 0--25 back into a string
def denumerify(nums: list):
    return "".join([chr((x % 26) + 65) for x in nums])

# Get text from integer representation
def deintegerify(n: int):
    n = int(n)
    rems = []
    while n > 0:
        rems.append(n % 26)
        n = n // 26
    return denumerify(reversed(rems))

# Decode a point on an elliptic curve into plaintext
def elliptic_decode(M, E, s):
    return deintegerify(M[0] // s)

# Decrypt the ciphertext pair R
def decrypt(R, E, k):
    return R[1] - k*R[0]
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(cipher=input_box(default="((12439165645636780372408872819196985400098429041444905471841965054215852, 13168353947844887098155880466151122665060908575026632194378808065612145, 1), (10653009826871052250391884872759053215859987410006331499967649660641664, 4230942832501466957956641218702916607547526549773148196384944547108179, 1))", label="Input", width=80),
      s=input_box(default="10", label="s", width=80),
      p=input_box(default="31624898780568028223033578567554929233454460200496038308094584218593743", label="p", width=80),
      a=input_box(default="2231", label="a", width=80),
      b=input_box(default="924384923849", label="b", width=80),
      P=input_box(default="(28242533037372562028376915025599704895624883370665290980560308139223847, 27339484572109674275112696180600875534462601488783406730716833705598007, 1)", label="P", width=80),
      order=input_box(default="31624898780568028223033578567554928906213834570791083268618301693807894", label="order", width=80),
      Q=input_box(default="(14860594925009170003954530097202645513024232456550100199567054270027585, 31448458171617918642007942988642343116727272631897730366311554311900365, 1)", label="Q", width=80),
      k=input_box(default="3378570623517000468044706849659752427201103476704244805424841619588371", label="K", width=80)):
    
    E = EllipticCurve(GF(p), [a, b])  
    P = E(P)
    Q = E(Q)
    R = (E(cipher[0]), E(cipher[1]))
    
    M = decrypt(R, E, k)
    o = elliptic_decode(M, E, s)
    
    output_div("Output", f'<textarea readonly rows="2" cols="80">{ o }</textarea>')
</script>
</div> 
</div>

<div class="element">
<span class="label">Exercise</span>
Do one (or more!) of the following: 

a. Play the role of Bob: Choose an elliptic curve Elgamal public key for yourself and post it to our class stream on Zulip.
b. Play the role of Alice: Watch for a classmate who has posted their elliptic curve Elgamal public key to Zulip, and send them a secret message.
c. Play the role of Eve: Watch for an elliptic curve Elgamal ciphertext being exchanged by some pair of your classmates on Zulip, and see if you can figure out what the plaintext is. 
</div>
