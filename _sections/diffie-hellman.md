The Diffie-Hellman key exchange is not quite a cryptosystem for exchanging messages; it is a protocol that allows Alice and Bob to share a secret, but neither has full control over the content of the shared secret. Nonetheless, the shared secret can then be used as the key for a symmetric key cipher like a one-time pad.

The procedure is named after [Whitfield Diffie](https://en.wikipedia.org/wiki/Whitfield_Diffie) and [Martin Hellman](https://en.wikipedia.org/wiki/Martin_Hellman), who published the protocol in 1976. The GCHQ mathematician [Malcolm Williamson](https://en.wikipedia.org/wiki/Malcolm_J._Williamson) had developed the same protocol back in 1974, but his work was only declassified in 1997. 

The procedure is as follows. Alice and Bob publicly agree to fix a prime $p$ and a primitive root $g$ mod $p$. Alice then chooses a secret integer $0 \leq a < p-1$ and sends Bob $x = g^a \bmod{p}$. She can compute this value quickly using binary exponentiation. Bob similarly chooses a secret integer $0 \leq b < p-1$ and sends Alice $y = g^b \bmod{p}$. Alice computes $s = y^a \bmod{p}$, and Bob computes $s = x^b \bmod{p}$. The two values of $s$ that Alice and Bob compute are the same, because
$$ y^a \equiv (g^b)^a = g^{ab} = (g^a)^b \equiv x^b \pmod{p}. $$
Thus Alice and Bob now share a secret, $s$. Notice that neither has full control over the shared secret, so this cannot be regarded as Alice or Bob sending a message to the other.

<div class="element">
<span class="label">Exercise</span>
Suppose Alice and Bob agree to use $p = 11$ and $g = 2$. Alice chooses the integer $a = 3$. She receives the integer $y = 5$ from Bob. What is her shared secret $s$ with Bob? 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain how the base-26 representation of Alice and Bob's shared secret $s$ can be used as a key for a one-time pad. 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why the presumed difficulty finding discrete logarithms implies the security of the Diffie-Hellman key exchange. 
</div>

<div class="element">
<span class="label">Exercise</span>
Do one (or both!) of the following: 

a. Play the role of Alice and Bob: Pick a friend in the class and perform a Diffie-Hellman key exchange with them. Do your communications in our class stream on Zulip so that everyone can see the values of $p, g, x, y$ that you share between the two of you, but make sure to keep $a$ and $b$ secret. 
b. Play the role of Eve: Watch for some pair of classmates performing Diffie-Hellman on Zulip, and then see if you can figure out their shared secret! 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain how the Diffie-Hellman process of producing a "shared secret" also appears as a *part* of the Elgamal cryptosystem. 
</div>

There are many other cryptographic protocols besides the Diffie-Hellman key exchange which involve the presumed difficulty of the discrete log problem and which are not strictly about exchanging messages. Here are some examples: 

* [Schnorr signature algorithm](https://en.wikipedia.org/wiki/Schnorr_signature)
* [Elgamal signature scheme](https://en.wikipedia.org/wiki/ElGamal_signature_scheme)
* [DSA](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm)
* [Zero-knowledge proofs of discrete logarithms](https://en.wikipedia.org/wiki/Zero-knowledge_proof#Discrete_log_of_a_given_value)

<div class="element">
<span class="label">Sage Exercise</span>
Read about one of the protocols listed above, and then implement it!
</div>


