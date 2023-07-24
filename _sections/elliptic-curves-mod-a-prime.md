We now fix a prime $p$. For technical reasons that not worth lingering on for our cryptographic purposes, we should assume $p \neq 2, 3$, ie, that $p \geq 5$. (In applications to cryptography, $p$ will anyway be very very large!)

We start with adapting our definitions above as follows. 

<div class="element">
<span class="label">Definition</span>
A Weierstrass equation $y^2 = x^3 + ax + b$ is *integral* if $a$ and $b$ are integers. We then say that the equation is *singular mod $p$* if $\Delta \equiv 0 \pmod{p}$. Otherwise, it is *nonsingular mod $p$*. 
</div>

Consider, for example, the Weierstrass equation $y^2 = x^3 + 17$. It is integral since $a = 0$ and $b = 17$ are integers. We calculated earlier that
$$ \Delta = -16 \cdot 27 \cdot 17^2. $$
The only primes this is divisible by are 2, 3, and 17. Thus the equation $y^2 = x^3 + 17$ is singular mod $p = 17$, but nonsingular otherwise (recall that we are anyway ruling out $p = 2, 3$). 

<div class="element">
<span class="label">Exercise</span>
For each of the following integral Weierstrass equations, find all primes $p \geq 5$ such that the equation is singular mod $p$. 

a. $y^2 = x^3 - 2x$
b. $y^2 = x^3 + x + 1$
c. $y^2 = x^3 - 1$
</div>

When we have an integral Weierstrass equation $y^2 = x^3 + ax + b$, we can consider its "solutions mod $p$." By this we mean integers $(x, y)$ such that $y^2 \equiv x^3 + ax + b \pmod{p}$. For example, suppose $p = 7$ and consider the Weierstrass equation $y^2 = x^3 + 17$. Then $(1, 2)$ is a solution to the equation mod $p$ because
$$ y^2 = 2^2 = 4 \equiv 18 = 1^3 + 17 = x^3 + 17 \pmod{7}. $$
Let's define that, if $x, y, x', y'$ are all integers, then $(x, y) \equiv (x', y') \pmod{p}$ means that $x \equiv x' \pmod{p}$ and $y \equiv y' \pmod{p}$. Notice that, if $(x, y)$ is a solution mod $p$ of $y^2 = x^3 + ax + b$, then so is any pair of integers that is congruent mod $p$. In the example above, observe that $(1, 9)$ or $(8, 2)$ or $(-6, 9)$ are all congruent mod 7 to $(1,2)$, and they are all also solutions mod 7 to $y^2 = x^3 + 17$. We'll typically only consider solutions $(x, y)$ where $0 \leq x, y < p$, in order to avoid having to list off all of the congruent solutions. 

It doesn't really make sense to visualize (congruence classes of) solutions to Weierstrass equations mod $p$ in the same way that we visualized solutions over the real numbers. Nonetheless, we can take the *formulas* that we derived using the geometry we discussed above (eg, the Elliptic Curve Addition Formula Theorem), and use the same formulas to make sense of elliptic curves mod $p$. These formulas look very strange in isolation, but it is important to remember where they come from: they come from the same geometry involving secant and tangent lines that we discussed above! If you understand that geometry, you can do all of the same calculations "mod $p$" and derive the formulas that appear below. 

<div class="element">
<span class="label">Definition-Theorem</span>
Suppose $y^2 = x^3 + ax + b$ is an integral Weierstrass equation which is nonsingular mod $p$. The corresponding *elliptic curve mod $p$* is the set $E$ of solutions mod $p$ to the equation, together with an extra *point at infinity* called $O$. 

Suppose $P \in E$. We define $-P$ as follows:

* If $P = O$, then $-P = O$ also. 
* If $P = (x, y)$, then $-P = (x, -y \bmod{p})$. 

Then $-P$ is in fact an element of $E$. 

Moreover, suppose $P, Q \in E$. We define $P + Q$ as follows: 

* If $P = O$, then $P + Q = Q$. 
* If $Q = O$, then $P + Q = P$. 
* Suppose $P = (x_1, y_1)$ and $Q = (x_2, y_2)$ are both solutions mod $p$ of $y^2 = x^3 + ax + b$. 

    - If $P \neq Q$ and $x_2 - x_1$ is not invertible mod $p$, set $P + Q = O$. 
    - If $P = Q$ and $y_1$ is not invertible mod $p$, set $P + Q = 2P = O$. 
    - Otherwise, define
        $$ \lambda = \begin{cases} 
        (y_2 - y_1)(x_2 - x_1)^{-1} \bmod{p} & \text{if } P \neq Q \\ 
        (3x_1^2 + a)(2y_1)^{-1} \bmod{p} & \text{if } P = Q
        \end{cases} $$
        Then set 
        $$ \begin{aligned}
        \nu &= (y_1 - \lambda x_1) \bmod{p} \\
        x_3 &= (\lambda^2 - x_1 - x_2) \bmod{p} \\
        y_3 &= (\lambda x_3 + \nu) \bmod{p}
        \end{aligned} $$
        Then $R = (x_3, y_3)$ is a solution mod $p$ and we define $P + Q = -R = (x_3, -y_3 \bmod{p})$. 
        
Then:

* (Associativity) $(P + Q) + R = P + (Q + R)$ for any $P, Q, R \in E$. 
* (Commutativity) $P + Q = Q + P$ for any $P, Q \in E$. 
* (Identity) $P + O = P$ for any $P \in E$. 
* (Inverses) $P + (-P) = O$ for any $P \in E$. 
</div>

Let's do an example. Consider again $p = 7$ and the Weierstrass equation $y^2 = x^3 + 17 \equiv x^3 + 3 \pmod{7}$. We've already seen that $P = (1, 2)$ is a solution to this equation. You should verify for yourself that $Q = (3, 4)$ is *also* a solution to this equation. Let's think about computing $P + Q$ in "two" ways (but, as you'll see, they're both really the same thing). 

The first way is just to go through the above formulas carefully. Notice that $P \neq Q$ and $x_2 - x_1 = 3 - 1 = 2$ is invertible mod 7 because $\gcd(2, 7) = 1$. Its inverse is 4 (which we can compute by eyeballing since the numbers are small enough in this case, but in general we would use the extended euclidean algorithm). Thus
$$ \begin{aligned}
\lambda &= (y_2 - y_1)(x_2 - x_1)^{-1} \bmod{p} \\
&= (4 - 2)(3 - 1)^{-1} \bmod{7} \\
&= 2 \cdot 4 \bmod{7} = 1.
\end{aligned} $$
Then
$$ \begin{aligned}
\nu &= y_1 - \lambda x_1 \bmod{p} \\
&= 2 - 1 \cdot 1 \bmod{7} = 1 \\
x_3 &= \lambda^2 - x_1 - x_2 \bmod{p} \\
&= 1^2 - 1 - 3 \bmod{7} = 4 \\
y_3 &= \lambda x_3 + \nu \bmod{p} \\
&= 1 \cdot 4 + 1 \bmod{7} = 5
\end{aligned} $$
Thus $R = (4, 5)$ and $P + Q = -R = (4, -5 \bmod{7}) = (4, 2)$. 

The second way (which, again, is really the same way!) is to remember the geometry that underlies these formulas and use that geometry to guide your calculations; you just have to remember to do your calculations "mod 7" instead. We're trying to add two distinct points, so the first step is to find the line that passes through those points. We compute the slope of that line as rise-over-run, ie, as $(y_2 - y_1)/(x_2 - x_1)$ --- but we have to do this calculation "mod 7," so instead of dividing, we'll multiply by a modular inverse. In other words, we really compute
$$ (4 - 2)(3 - 1)^{-1} = 2 \cdot 2^{-1} \equiv 2 \cdot 4 = 8 \equiv 1 \pmod{7}. $$
Notice that this is the same as the value of $\lambda$ that we computed above. The next step is to use point-slope form to find the equation of the secant line: we now know it has slope 1, and it passes through $(1, 2)$, so its equation should be $y - 2 = 1(x - 1)$, or $y = x + 1$. Notice that this $y$-intercept matches what we computed for $\nu$ above. We next want to intersect this secant line with the curve. We substitute $y = x + 1$ into $y^2 = x^3 + 3$ and get 
$$ \begin{aligned}
(x+1)^2 &= x^3 + 3 \\
x^2 + 2x + 1 &= x^3 + 3 \\
x^3 - x^2 - 2x + 2 &= 0
\end{aligned} $$
We now need to find the roots of this cubic mod 7. We know that 1 and 3 are roots, so $(x-1)(x-3) = x^2 - 4x + 3$ must divide this cubic. 

<div class="element">
<span class="label">Exercise</span>
Do polynomial long division "mod 7" to divide $x^3 - x^2 - 2x + 2$ by $x^2 - 4x + 3$. You should find a quotient of $x-4$ with remainder 0. 
</div>

The fact that the quotient is $x-4$ tells us that the third point of intersection of the secant line with the curve has $x$-coordinate 4, so its $y$-coordinate must be $y = x + 1 = 4 + 1 = 5$. This gives us the same point $R = (4, 5)$ that we found earlier, and then we "reflect" across the $x$-axis to get 
$$ P + Q = (1, 2) + (3, 4) = (4, -5 \bmod{7}) = (4, 2), $$
just as we did before. 

<div class="element">
<span class="label">Exercise</span>
Consider $p = 7$ and $y^2 = x^3 + 17$ again. Compute the following: 

a. $(1, 2) + (1, 2)$.
b. $(1, 2) + (1, 5)$.
c. $(2, 2) + (3, 3)$. 
d. $(2, 2) + (2, 2)$. 
e. $(2, 2) + O$.
f. $O + O$.
</div>

Again, we can quickly compute large multiples of a point using the same "binary multiplication" idea.

<div class="element">
<span class="label">Exercise</span>
Consider $p = 7$ and $y^2 = x^3 + 17$ again. Let $P = (1, 2)$ and use "binary multiplication" to compute $11P$. 
</div> 

We can also make the following definition of order.

<div class="element">
<span class="label">Definition</span>
Suppose $P$ is a point on an elliptic curve $E$ mod $p$. The *order* of $P$, denoted $\mathrm{ord}_E(P)$, is the smallest positive integer $n$ such that $nP = O$. 
</div>

Unlike in the real case, we don't need to worry about infinite order anymore: 

<div class="element">
<span class="label">Group Theory Exercise</span>
Explain why, if $E$ is an elliptic curve mod $p$, then it must be that $\mathrm{ord}_E(P)$ is finite for any $E \in P$. 
</div>

We also have the analogs of the First and Second Lemmas about Orders in this case: 

<div class="element">
<span class="label">First Lemma About Orders for Elliptic Curves</span>
Let $E$ be an elliptic curve mod $p$ and suppose $P \in E$. If $mP = O$, then $\mathrm{ord}_E(P)$ divides $m$. 
</div>

<div class="element">
<span class="label">Second Lemma About Orders for Elliptic Curves</span>
Let $E$ be an elliptic curve mod $p$ and suppose $P \in E$. Let $k = \mathrm{ord}_E(P)$. Then $iP \equiv jP \pmod{p}$ if and only if $i \equiv j \pmod{k}$. In particular, the points $O, P, 2P, 3P, \dotsc, (k-1)P$ are all incongruent mod $p$. 
</div>

If you haven't seen any group theory, you can just take these lemmas for granted. But if you have studied some group theory, I highly encourage you to work through the following exercise: 

<div class="element">
<span class="label">Group Theory Exercise</span>
Prove the First and Second Lemmas about Orders for elliptic curves.
</div>

Here are some concrete examples: 

<div class="element">
<span class="label">Exercise</span>
Consider $p = 5$ and $y^2 = x^3 + 17$. Verify that the following points are on the elliptic curve, and then calculate their orders.

a. $(2, 0)$
b. $(3, 2)$
c. $(3, 3)$
d. $(4, 1)$
</div>

<div class="element">
<span class="label">Exercise</span>
Consider $p = 7$ and $y^2 = x^3 + 17$. Show that $P = (1, 2)$ has order 13. 
</div>

<div class="element">
<span class="label">SageCell</span>
Here is some Sage code to create an elliptic curve mod $p$. Specifically, this code sets `E` to be the elliptic curve mod $p = 7$ corresponding to the equation $y^2 = x^3 + 17 = x^3 + 0 \cdot x + 17$. The `GF(7)` is what tells Sage that you want to work mod 7. We then call `points()` to see a complete list of all the (congruence classes of) points on the elliptic curve. Hit "Run," and then read below to understand more about the output: 
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(GF(7), [0, 17])
E.points()
</script>
</div>
Again, you will see points in projective coordinates when you do this. For example, we saw above that $(1, 2)$ is a solution to $y^2 = x^3 + 17$ mod 7, and Sage lists `(1 : 2 : 1)`, which refers to the same point.

If you already know a point $(x, y)$ on the elliptic curve, you can tell Sage to treat it as a point on the elliptic curve by prefixing it with the name of the curve. The following code defines `P` to be the point $(1, 2)$. 
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(GF(7), [0, 17])
P = E(1, 2)
P
</script>
</div>
You can now easily get Sage to perform addition on an elliptic curve for you! The following computes $(1, 2) + (3, 4)$, the same calculation we did earlier. Hit "Run" to make sure that Sage agrees that the answer is the point $(4, 2)$.
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(GF(7), [0, 17])
E(1, 2) + E(3, 4)
</script>
</div>
We can also get Sage to calculate orders for us, as follows: 
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(GF(7), [0, 17])
E(1, 2).order()
</script>
</div>
</div>

<div class="element">
<span class="label">SageCell</span>
Earlier, I said that "it doesn't really make sense to visualize (congruence classes of) solutions to Weierstrass equations mod $p$ in the same way that we visualized solutions over the real numbers." You *could* make a plot of all of the points on the curve, but you get very weird pictures: it doesn't look anything like a "curve" and it doesn't make any sense to draw secant or tangent lines this. You can get Sage to make these types of pictures for you as follows, but I don't recommend thinking too much about these pictures. They're next to meaningless...
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(GF(7), [0, 17])
E.plot()
</script>
</div>
</div>

Having developed all of this theory, we can now state the basic problem that underlies elliptic curve cryptography. You should compare it with the discrete logarithm problem that we discussed above:

<div class="element">
<span class="label">Elliptic Curve Discrete Logarithm Problem</span>
Suppose you are given a prime $p$, an elliptic curve $E$ mod $p$, and a point $P \in E$. Suppose further that $Q \in E$ is known to be a multiple of $P$. Find the discrete logarithm base $P$ of $Q$, ie, the unique integer $k$ such that $0 \leq k < \mathrm{ord}_E(P)$ such that $Q = mP$. 
</div>

The naive method for doing this is to just try all the values of $k$ starting from $k = 0$ all the way up to $k = \mathrm{ord}_E(P) - 1$, but if $\mathrm{ord}_E(P)$ is very large, this will be very slow! There are *slightly* faster methods than this that work *in general*. There are also *substantially* faster methods that work *in special cases*. But we do not know of anything that is *substantially* faster *in general*, which means that by choosing $p, E, P$ judiciously, we can make the Elliptic Curve Discrete Logarithm Problem intractable for an adversary. This intractability can then be used for cryptographic purposes, in much the same way that the intractibility of the usual Discrete Logarithm Problem can be.

The National Institute of Standards and Technology (NIST) *just* published (on February 3rd, 2023) a document describing which elliptic curves are currently thought to be safe to use for cryptography; you can browse this document [here](https://doi.org/10.6028/NIST.SP.800-186). 

<div class="element">
<span class="label">SageCell</span>
Sage can find discrete logarithms for elliptic curves as well. Here is the syntax for finding the integer $m$ such that $(5, 3) = m(1, 2)$. 
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(GF(7), [0, 17])
E(1, 2).discrete_log(E(5, 3))
</script>
</div>
</div>

