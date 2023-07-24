Let's start with the elliptic curve variant of the Diffie-Hellman key exchange. Alice and Bob publicly agree to fix a prime $p$, an elliptic curve $E$ mod $p$ (specified by integers $a, b$ such that the Weierstrass equation $y^2 = x^3 + ax + b$ is nonsingular mod $p$), and a point $P \in E$. To ensure security, we need for $\mathrm{ord}_P(E)$ to be large. The data $(p, E, P)$ is all shared publicly. 

Alice then chooses a secret integer $0 \leq k_a < \mathrm{ord}_E(P)$ and sends Bob $Q_a = k_a P$. She can compute this value quickly using binary multiplication. Bob similarly chooses a secret integer $0 \leq k_b < \mathrm{ord}_E(P)$ and sends Alice $Q_b = k_b P$. Alice computes $R = k_a Q_b$, and Bob computes $R = k_b Q_a \bmod{p}$. The two values of $R$ that Alice and Bob compute are the same, because
$$ k_aQ_b \equiv k_a(k_bP) = k_ak_bP = k_b(k_aP) = k_bQ_a. $$
Thus Alice and Bob now share a secret point $R$ on the elliptic curve. 

<div class="element">
<span class="label">Exercise</span>
Suppose Alice and Bob publicly agree to use the elliptic curve $y^2 = x^3 + 17$ mod $p = 7$ and the point $P = (1, 2)$. 

a. Suppose Alice picks the number $k_a = 4$. What is the message $Q_a$ that she sends Bob? 
b. Suppose Alice receivces the point $Q_b = (5, 3)$ from Bob. What is her shared secret with Bob? 
</div>

<div class="element">
<span class="label">Exercise</span>
Explain why the presumed difficulty of finding elliptic curve discrete logarithms implies the security of the elliptic curve Diffie-Hellman key exchange. 
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose Alice and Bob publicly agree to use the elliptic curve $y^2 = x^3 + 17$ mod $p = 7$ and the point $P = (1, 2)$. This point has order 13, which is *too small* to be secure. Suppose Eve intercepts Alice and Bob's message: she sees that Alice sent Bob $Q_a = (3, 3)$ and that Bob sent Alice $Q_b = (6, 4)$. What is Alice and Bob's shared secret?  
</div>

<div class="element">
<span class="label">Exercise</span>
Do one (or both!) of the following: 

a. Play the role of Alice and Bob: Pick a friend in the class and perform an elliptic curve Diffie-Hellman key exchange with them. Do your communications in our class stream on Zulip so that everyone can see the values of $p, E, P, Q_a, Q_b$ that you share between the two of you, but make sure to keep $a$ and $b$ secret. 
b. Play the role of Eve: Watch for some pair of classmates performing elliptic curve Diffie-Hellman on Zulip, and then see if you can figure out their shared secret! 
</div>

<div class="element">
<span class="label">Sage Exercise</span>
Write some code that for performing an elliptic curve Diffie-Hellman key exchange. 
</div>

