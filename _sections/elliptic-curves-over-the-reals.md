We'll start with a discussion of elliptic curves over the real numbers in order to develop some geometric intuition about elliptic curves. 

<div class="element">
<span class="label">Definition</span>
A *Weierstrass equation* over the real numbers is an equation in $x$ and $y$ of the form
$$ y^2 = x^3 + ax + b $$
where $a, b$ are fixed real numbers. The *discriminant* of the equation is
$$ \Delta = -16(4a^3 + 27b^2) $$
and the equation is *singular* when $\Delta = 0$. Otherwise, the equation is said to be *nonsingular*.  
</div>

Here is a theorem that is useful to keep in mind, but we omit a proof because it will not be important for us in cryptography: 

<div class="element">
<span class="label">Theorem</span>
The Weierstrass equation $y^2 = x^3 + ax + b$ is nonsingular if and only if the cubic equation $x^3 + ax + b = 0$ has no repeated roots in the complex numbers.  
</div>

Given a Weierstrass equation, we can make a plot of the points $(x, y)$ in the plane that satisfy that equation. Here is a SageCell that makes these plots for you; make sure to play with it before moving forward!

<div class="element" id="sagecell-elliptic-plotter">
<span class="label">SageCell / Exercise</span>
This SageCell makes plots of curves defined by Weierstrass equations. It defaults to $y^2 = x^3 - 1$. The view box defaults to the range $-3 \leq x, y \leq 3$, but you can change the 3 using the ``View'' input box. The cell tells you whether the curve is singular or not, and then makes plots. Use the SageCell to look at plots of the following curves: 

a. $y^2 = x^3 + b$ for $b = 0, -1, -2, -3, \dotsc, -6$
b. $y^2 = x^3 + ax$ for $a = -6, -5, \dotsc, 0, 1, \dotsc, 6$
c. $y^2 = x^3 + ax + \sqrt{4/27}$ for $a = -0.5, -0.8, -0.9, -0.99, -1, -1.01, -1.1, -1.2, -1.5$

Note that you can enter expressions like `sqrt(4/27)` in the input box for `b`. 

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
def _(a=input_box(default="-1", label="a", width=80), 
      b=input_box(default="0", label="b", width=80),
      r=input_box(default="3", label="View", width=80),
      ):
    
    var("x y")
    delta = 4*a^3 + 27*b^2
    output_div("Δ", f"{float(delta):.2f}")
    output_div("Sing?", float(delta) == 0)
    p = implicit_plot(y^2 - x^3 - a*x - b, (x,-r,r), (y,-r,r))
    p.show(axes=True, frame=False)
</script>
</div> 
</div>

What you should have found by playing with the above SageCell is that the set of points satisfying a *nonsingular* Weierstrass equation always roughly has one of the following shapes: 

<div style="text-align: center; margin: auto;">
<img style="width: 40%; display: inline-block;" src="{{ site.baseurl }}/img/elliptic1.png"/>
<img style="width: 40%; display: inline-block;" src="{{ site.baseurl }}/img/elliptic2.png"/>
</div>

These curves have a lot of important geometry. To get started, observe that they are symmetric about the $x$-axis, ie, they have vertical symmetry. Stated formally, if $(x, y)$ is a point satisfying $y^2 = x^3 + ax + b$, then its reflection $(x, -y)$ across the $x$-axis is also a point satisfying the same equation! If $P = (x, y)$ is a point on a curve defined by a Weierstrass equation, we define $-P$ to be this reflection $(x, -y)$. (Careful: we only invert the $y$-coordinate of $P$ to get $-P$, not both coordinates!)

Here is the second property: if I pick any two points on the curve, the unique line that passes through those two points will have a unique "other" point of intersection with the curve. Well, *almost*...! But we will get to this *almost* soon. First, here is an example: 

Consider the Weierstrass equation 
$$ y^2 = x^3 + 17. $$
It is nonsingular because
$$ \Delta = -16(4 \cdot 0^3 + 27 \cdot 17^2) = -16 \cdot 27 \cdot 17^2 \neq 0. $$
Consider the points $P = (-2, 3)$ and $Q = (-1, 4)$. Both of these lie on the curve, as you should verify: 

<div class="element">
<span class="label">Exercise</span>
Check that $P = (-2, 3)$ and $Q = (-1, 4)$ are both on the curve defined by $y^2 = x^3 + 17$. 
</div>

There is a unique "secant" line that passes through these two points. We can calculate its slope using the usual rise-over-run formula as
$$ m = \frac{4 - 3}{-1 - (-2)} = \frac{1}{1} = 1 $$
and then we can find the equation of the secant line itself using point-slope form:
$$ \begin{aligned}
y - 4 &= 1 \cdot (x - (-1)) \\
y &= x + 5
\end{aligned} $$
Let's now think about intersecting this secant line with the curve. This will consist of all $(x, y)$ which satisfy both $y = x + 5$ and also $y^2 = x^3 + 17$, so if we substitute the first equation in the second, we see that we must have
$$ \begin{aligned}
(x+5)^2 &= x^3 + 17 \\
x^2 + 10x + 25 &= x^3 + 17 \\
x^3 - x^2 - 10x - 8 &= 0
\end{aligned} $$
We now have to solve a cubic equation, which isn't easy in general, but here's the trick: we already know that $x = -2$ and $x = -1$ must be solutions to this equation! After all, we know that $P = (-2, 3)$ and $Q = (-1, 4)$ are on the curve and on the line, and the $x$-coodinates of these points are precisely $-2$ and $-1$. This means that $(x+2)(x+1) = x^2 + 3x + 2$ must divide $x^3 - x^2 - 10x - 8$, and we can use polynomial long division to find the quotient. 

<div class="element">
<span class="label">Exercise</span>
Use polynomial long division to divide $x^3 - x^2 - 10x - 8$ by $x^2 + 3x + 2$ and check that the quotient is $x-4$ and the remainder is 0. 
</div>

The upshot is that $x^3 - x^2 - 10x - 8 = (x+2)(x+1)(x-4)$, so the third solution to the cubic equation $x^3 - x^2 - 10x - 8 = 0$ is $x = 4$. If we plug $x = 4$ into the equation of the line, we find the point $R = (4, 9)$. We already know by construction that this point $R$ must satisfy the Weierstrass equation too, but you should check this: 

<div class="element">
<span class="label">Exercise</span>
Check that $R = (4, 9)$ is on the curve defined $y^2 = x^3 + 17$. 
</div>

Here is the picture of what we've just done: 

<div style="text-align: center; margin: auto;">
<img style="width: 80%; display: inline-block;" src="{{ site.baseurl }}/img/elliptic3.png"/>
</div>

As indicated in the picture, we will define $P+Q$ to be the negative of this third point of intersection. Be careful: this process is much more complicated than simply adding the $x$ and $y$ coordinates of the two points! You should do the following exercise before proceeding: 

<div class="element">
<span class="label">Exercise</span>

a. Let $S = (2, 5)$. Check that $S$ is on the same curve $y^2 = x^3 + 17$.
b. What is the third point of the intersection on between the curve $y^2 = x^3 + 17$ and the secand line connecting $Q = (-1, 4)$ and $S$? 
c. What is $Q + S$? 
d. With $P = (-2, 3)$ and $R = (4, 9)$ on $y^2 = x^3 + 17$ as above, what is $P + R$? 
</div>

Let's now return to the "almost" we mentioned above. In fact, this "almost" actually encompassed two caveats that we need to discuss. 

The first caveat has to do with the case when the two points we start with are the same point. Consider again the situation $P = (-2, 3)$. How can we make sense of $P + P$, or $2P$? The very first step involves finding the equation of the secant line that passes through $P$ and $P$, but there isn't a unique such line...!

But, if you think back to your calculus classes, you might remember that: if you think about the secant lines through two points on a curve, and take the limit as one of the two points approaches the other, what you wind up with is the tangent line! So we'll use this. In other words, to define $P + P$, we'll go through the same process that we did above, but we'll use the tangent line of the curve at $P$. 

Let's work this out. To find the slope of the tangent line, we use implicit differentiation on the equation $y^2 = x^3 + 17$. We get
$$ 2y \frac{dy}{dx} = 3x^2 $$
and then we plug in $P = (x, y) = (-2, 3)$ to get
$$ \begin{aligned} 
2 \cdot 3 \cdot \left.\frac{dy}{dx}\right|_P &= 3(-2)^2 \\
\left.\frac{dy}{dx}\right|_P &= 2
\end{aligned} $$
We then find the equation of the tangent line again using point-slope form like we did above:
$$ \begin{aligned}
y - 3 &= 2(x - (-2)) \\
y &= 2x + 7
\end{aligned} $$
We now want to intersect this point with the curve, so we again substitute $y = 2x+7$ into $y^2 = x^3 + 17$ like we did above: 
$$ \begin{aligned}
(2x+7)^2 &= x^3 + 17 \\
4x^2 + 28x + 49 &= x^3 + 17 \\
x^3 - 4x^2 - 28x - 32 &= 0
\end{aligned} $$
We again have a cubic equation to factor. Last time, we knew two factors of the cubic because we knew two points on the intersection. This time, we only know one point of the intersection, but we know that the line is tangent to the curve at $P$, so the $x$-coordinate of $P$, ie $x = -2$, must be a double root of this cubic! This means that the cubic must be divisible by $(x-(-2))^2 = x^2 + 4x + 4$, and we can again use polynomial long division to find the "third" point of intersection. 

<div class="element">
<span class="label">Exercise</span>
Use polynomial long division to divide $x^3 - 4x^2 - 28x - 32$ by $x^2 + 4x + 4$ and check that the quotient is $x-8$ and the remainder is 0. 
</div>

This means that the third point of intersection has $x$-coordinate 8, so its $y$-coordinate is $2 \cdot 8 + 7 = 23$. We then reflect this point across the $x$-axis and find that $2P = (8, -23)$. Here is a picture of what we have just done: 

<div style="text-align: center; margin: auto;">
<img style="width: 80%; display: inline-block;" src="{{ site.baseurl }}/img/elliptic4.png"/>
</div>

Note that the curve looks different from the previous picture only because of the scale on the axes; it is the same curve. 

<div class="element">
<span class="label">Exercise</span>
On the same curve $y^2 = x^3 + 17$, calculate the following. 

a. $2Q$, where $Q = (-1, 4)$. 
b. $2S$, where $S = (2, 5)$. 
</div>

This brings us to the second caveat that was encompassed by our "almost." There is one situation when the line passing through two points (either the secant line through two distinct points or the tangent line at a single point) does not have a third point of intersection with the curve. This will happen precisely when the line ends up being vertical! This should be clear from the geometry, but you should verify it algebraically: 

<div class="element">
<span class="label">Exercise</span>
Verify algebraically that the secant line passing through $(-2, 3)$ and $(-2, -3)$ is vertical, and that this line has no other point of intersection with the curve $y^2 = x^3 + 17$. 
</div>

We deal with this *by fiat*. We declare that there is a point, denoted $O$ and called *the point at infinity*, which is a point on the curve defined by $y^2 = x^3 + 17$ and is also a point on every vertical line in the plane. It is its own negative. Thus, if we're trying to "add" two points and encounter the situation where a secant or tangent line is vertical, we declare the sum to be $O$. 

<div class="element">
<span class="label">Exercise</span>
Based on our definition of $O$ as being "point on the curve defined by $y^2 = x^3 + 17$ and also a point on every vertical line in the plane," explain why it makes sense to assert that $P + O = P$ for $P = (-2, 3)$ -- or, in fact, for any point $P$ on the curve. 
</div>

Having worked through this extended example, we can now make some formal statements. 

<div class="element">
<span class="label">Definition-Theorem</span>
Fix a nonsingular Weiertrass equation $y^2 = x^3 + ax + b$. The *elliptic curve* $E$ defined by this equation is the set of points $(x, y)$ satisfying this equation, together with a *point at infinity* denoted $O$ and assumed by fiat to: 

* lie on the curve; 
* lie on every vertical line in the plane; and
* be its own reflection across the $x$-axis. 

Given any $P \in E$, we define $-P$ to be the reflection of $P$ across the $x$-axis. Given any two points $P, Q \in E$, there is a unique secant or tangent line passing through $P$ and $Q$, and this line has a unique third point of intersection with the curve; we define $P+Q$ to be the negative of this third point of intersection. Then:

* (Associativity) $(P + Q) + R = P + (Q + R)$ for any $P, Q, R \in E$. 
* (Commutativity) $P + Q = Q + P$ for any $P, Q \in E$. 
* (Identity) $P + O = P$ for any $P \in E$. 
* (Inverses) $P + (-P) = O$ for any $P \in E$. 
</div>

If you've taken some abstract algebra, you'll probably recognize that the above is saying that $E$ is an "abelian group" --- but don't worry about this if you don't know what this means. 

<div class="element">
<span class="label">Exercise</span>
Explain geometrically why the commutativity, identity, and inverses properties above hold. 

*Note*. It is also possible to explain geometrically why the associativity property holds, but this is much much harder. 
</div>

<div class="element">
<span class="label">Exercise</span>
What do you think goes wrong if we start with a *singular* Weierstrass equation instead? 
</div>

It is also useful to generalize the calculations we did above and derive the following general formulas: 

<div class="element">
<span class="label">Elliptic Curve Addition Formula Theorem</span>
Suppose $P = (x_1, y_1)$ and $Q = (x_2, y_2)$ are both points on the curve $y^2 = x^3 + ax + b$. Define
$$ \lambda = \begin{cases} 
\dfrac{y_2 - y_1}{x_2 - x_1} & \text{if } P \neq Q \\ 
\dfrac{3x_1^2 + a}{2y_1} & \text{if } P = Q
\end{cases} $$
If $\lambda$ is defined (ie, if the denominator above is nonzero), set 
$$ \begin{aligned}
\nu &= y_1 - \lambda x_1 \\
x_3 &= \lambda^2 - x_1 - x_2 \\
y_3 &= \lambda x_3 + \nu
\end{aligned} $$
Then $P + Q = (x_3, -y_3)$. 
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Prove the Elliptic Curve Addition Formula Theorem. *Hint*. The line passing through $P$ and $Q$ is $y = \lambda x + \nu$. 
</div>

<div class="element">
<span class="label">Harder Proof Exercise</span>
Use the Elliptic Curve Addition Formula Theorem to prove algebraically that the associativity property holds, ie, that $(P + Q) + R = P + (Q + R)$ for all $P, Q, R \in E$. 
</div>

Let's conclude our discussion of elliptic curves over the reals with one more concrete example. This is another highly recommended exercise!

<div class="element">
<span class="label">Exercise</span>
Consider the Weierstrass equation $y^2 = x^3 - 2x$. 

a. Verify that this equation is nonsingular. Let $E$ be the corresponding elliptic curve.
b. Verify that $P = (0, 0)$ is on the curve. What is $2P$? 
c. Verify that $Q = (-1, 1)$ is on the curve. What is $2Q$? 
d. What is $P + Q$? 
e. What is $3Q$?
f. What is $4Q$?
g. What is $5Q$?  
</div>

We can compute multiples of a point by using the same idea as binary exponentiaton --- except that maybe it's better to call it "binary multiplication" in this situation. Consider $P = (-2, 3)$ on the elliptic curve defined by $y^2 = x^3 + 17$ again. Let's say that we want to compute $6P$, ie, the sum of $P$ with itself 6 times. The idea is to double repeatedly:
$$ \begin{aligned}
P &= (-2, 3) \\
2P &= (8, -23) \\
4P &= 2(2P) = \left(\frac{752}{529}, -\frac{54239}{12167}\right)
\end{aligned} $$
We then put these together: 
$$ 6P = 4P + 2P = \left(\frac{752}{529}, -\frac{54239}{12167}\right) + (8, -23) = \left( -\frac{4471631}{3027600}, -\frac{19554357097}{5268024000} \right). $$

We also make the following definition of order. 

<div class="element">
<span class="label">Definition</span>
Suppose $P$ is a point on an elliptic curve $E$. The *order* of $P$, denoted $\mathrm{ord}_E(P)$, is the smallest positive integer $n$ such that $nP = O$, if such an integer exists. Otherwise, the order of $P$ is $\infty$. 
</div>

<div class="element">
<span class="label">Exercise</span>
Let $E$ be the elliptic curve defined by the Weierstrass equation $y^2 = x^3 - 4x$. Find the order of the following points. 

a. $P = (-2, 0)$.  
b. $Q = (0, 0)$.
c. $P + Q$.
</div>

<div class="element">
<span class="label">Exercise</span>
Let $E$ be the elliptic curve defined by the Weierstrass equation $y^2 = x^3 + 5805x - 285714$. Verify that $P = (327, 6048)$ is a point on this curve, and then find its order by hand. 

*Note*. This might be a little tedious, but it's good practice! It's also not *that* horrendous: the answer is somewhere between 5 and 10 ☺ 
</div>

<div class="element">
<span class="label">SageCell</span>
Here is some Sage code to create an elliptic curve. Specifically, this code sets `E` to be the elliptic curve corresponding to the equation $y^2 = x^3 + 17 = x^3 + 0 \cdot x + 17$. We then define `P` the be the point $(-2, 3)$ on the curve `E` and `Q` to be the point $(-1, 4)$, and then have Sage compute the sum for us!
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(QQ, [0, 17])
P = E(-2, 3)
Q = E(-1, 4)
P + Q
</script>
</div>
A note about the format of the output: the point that we've been calling $O$ is denoted by Sage as `(0 : 1 : 0)`, and the point that we'd call $(x, y)$ is what Sage would denote by `(x : y : 1)`. There are really good reasons for doing this (these are what are called "projective coordinates" for the points), but we don't need to get into this. If you like, you can also specify points on the curve in projective coordinates by writing things like `P = E(-2, 3, 1)`. 

We can also get Sage to compute multiples for us: 
<div class="sage">
<script type="text/x-sage">
E = EllipticCurve(QQ, [0, 17])
P = E(-2, 3)
6*P
</script>
</div>
Note that the `QQ` in the above tells Sage to do exact arithmetic with fractions, instead of using floating point decimal approximations. If you don't care about doing exact arithmetic, you can replace the `QQ` with `RR` instead. 
</div>
