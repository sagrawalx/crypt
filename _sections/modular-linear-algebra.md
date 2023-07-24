The Polybius square is the last of our simple substitution ciphers for now. Before turning our attention to our first polygraphic cipher, let's look at how linear algebra interacts with modular arithmetic. Since linear algebra is not a prerequisite for this class, we'll focus on $2 \times 2$ matrices for simplicity.[^linalg]

[^linalg]: If you've studied linear algebra, you'll likely be able to convince yourself that everything here generalizes to $n \times n$ matrices. 

### $2 \times 2$ matrices

<div class="element">
<span class="label">Definition</span>

* A *$2 \times 2$ integer matrix* (or just *matrix* for short) is a $2 \times 2$ box of numbers $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$ where $a, b, c, d$ are all integers.  

* The *determinant* of $A$ is the integer $\det(A) = ad - bc$. 

* The *identity matrix* is the matrix $I = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$. 

* Suppose $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$ and $B = \begin{bmatrix} a' & b' \\ c' & d' \end{bmatrix}$ are two matrices. Their product $AB$ is defined to be: $$ AB = \begin{bmatrix} aa' + bc' & ab' + bd' \\ ca' + dc' & cb' + dd' \end{bmatrix} $$
</div>

For example, $A = \begin{bmatrix} 3 & 2 \\ 1 & 7 \end{bmatrix}$ is a matrix and $\det(A) = 3 \cdot 7 - 2 = 19$. Another matrix is $B = \begin{bmatrix} 1 & 4 \\ 2 & 3 \end{bmatrix}$, and you should check by comparing against the definition above that 
$$ AB = \begin{bmatrix} 7 & 18 \\ 15 & 25 \end{bmatrix} $$
You should also check that
$$ BA = \begin{bmatrix} 7 & 30 \\ 9 & 25 \end{bmatrix}. $$
Note that $AB \neq BA$. In other words, matrix multiplication is not commutative!

<div class="element">
<span class="label">Exercise</span>
Let $A$ be a matrix (ie, a $2 \times 2$ integer matrix). Show that $AI = IA = A$. 
</div>

<div class="element">
<span class="label">Theorem</span> (Multiplicativity of the Determinant)
If $A$ and $B$ are matrices, then $\det(I) = 1$ and 
$$ \det(AB) = \det(A)\det(B). $$
</div>

<div class="element">
<span class="label">Definition</span>

* A *vector* is $v$ a vertical column $v = \begin{bmatrix} x \\ y \end{bmatrix}$ where $x$ and $y$ are both integers. 

* If $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$ is a matrix, the product $Av$ is defined to be: 
$$ Av = \begin{bmatrix} ax + by \\ cx + dy \end{bmatrix} $$
</div>

<div class="element">
<span class="label">SageCell</span>
Sage can do all of the above types of computations. Construct matrices using the `matrix` function, and compute determinants using the `det` function: 
<div class="sage">
<script type="text/x-sage">
# This is how you input matrices into Sage
A = matrix([[3, 2], [1, 7]])
# This is how you compute the determinant
det(A)
</script>
</div>
Multiply using the `*` operator: 
<div class="sage">
<script type="text/x-sage">
# This is how you input matrices
A = matrix([[3, 2], [1, 7]])
B = matrix([[1, 4], [2, 3]])
# This is how you multiply
A*B
</script>
</div>
Construct vectors using the `vector` function, and again multiply using the `*` operator: 
<div class="sage">
<script type="text/x-sage">
# This is how you input a matrix
A = matrix([[3, 2], [1, 7]])
# This is how you input a vector
v = vector([1, 2])
# This is how you multiply
A*v
</script>
</div>
</div>

### Congruences and Inversion for Matrices

<div class="element">
<span class="label">Definition</span>
Fix a positive integer $n$ and suppose $A$ and $B$ are both matrices: 
$$ A = \begin{bmatrix} a & b \\ c & d \end{bmatrix} \quad\text{and}\quad B = \begin{bmatrix} a' & b' \\ c' & d' \end{bmatrix} $$
We say that $A \equiv B \pmod{n}$ if all four of the entries of the two matrices are congruent mod $n$, ie, if all of the following are true:
$$ \begin{aligned}
a &\equiv a' &&\pmod{n} \\
b &\equiv b' &&\pmod{n} \\
c &\equiv c' &&\pmod{n} \\
d &\equiv d' &&\pmod{n}
\end{aligned} $$ 
</div>

<div class="element">
<span class="label">Definition</span>
A matrix $A$ is *invertible mod $n$* if there exists a matrix $X$ such that $AX \equiv I \bmod{n}$. In this case, $X$ is called an *inverse of $A$ mod $n$*. In symbols, we write $X \equiv A^{-1} \pmod{n}$. 
</div>

<div class="element">
<span class="label">Matrix Inversion Theorem</span>
Suppose $$A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$$ is a matrix. Then $A$ is invertible if and only if $\det(A)$ is invertible mod $n$. Moreover, if $e \equiv \det(A)^{-1} \pmod{n}$, then
$$ X = \begin{bmatrix} ed & -eb \\ -ec & ea \end{bmatrix} $$
is an inverse of $A$ mod $n$. 
</div>

Suppose, for example, that we're interested in the matrix 

$$ A = \begin{bmatrix} 3 & 2 \\ 1 & 7 \end{bmatrix}. $$

We know that $\det(A) = 19$ is invertible mod 26, so $A$ is also invertible mod 26. We have $19^{-1} \equiv 11 \pmod{26}$, so the formula for the inverse from the Matrix Inversion Theorem tells us that

$$ A^{-1} \equiv \begin{bmatrix} 11 \cdot 7 & -11 \cdot 2 \\ -11 \cdot 1 & 11 \cdot 3 \end{bmatrix} = \begin{bmatrix} 77 & -22 \\ -11 & 33 \end{bmatrix} \equiv \begin{bmatrix} 25 & 4 \\ 15 & 7 \end{bmatrix} \pmod{26}. $$

In other words, 

$$ X = \begin{bmatrix} 25 & 4 \\ 15 & 7 \end{bmatrix} $$

is an inverse of $A$ mod 26. Let's check this: 

$$ AX = \begin{bmatrix} 3 & 2 \\ 1 & 7 \end{bmatrix} \begin{bmatrix} 25 & 4 \\ 15 & 7 \end{bmatrix} = \begin{bmatrix} 105 & 26 \\ 130 & 53 \end{bmatrix} \equiv \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} = I \pmod{26}.  $$

<div class="element">
<span class="label">SageCell</span>
The syntax for inverting a matrix mod $n$ in Sage is a little long. Notice where the matrix goes and where the modulus goes: 
<div class="sage">
<script type="text/x-sage">
A = matrix([[3, 2], [1, 7]])
# This line computes an inverse of A mod 26
A.change_ring(IntegerModRing(26)).inverse().lift()
</script>
</div>
</div>
