We now turn our attention to our first polygraphic cipher: the *Hill cipher*. We'll focus on the digraphic case, which replaces 2 letters of plaintext at a time.[^linalg2]

[^linalg2]: If you knew something about linear algebra beforehand and were able to generalize the previous section to $n \times n$ matrices in your head, you can probably also generalize this section.

To encrypt messages using a Hill cipher, we start by our usual encoding. We also make the usual identification of the letters `A` through `Z` with the numbers 0 through 25. Suppose for example that we want to encrypt the following: 

```
 Y  O  U  H  A  V  E  S  A  V  E  D  U  S  A  L  L
24 14 20  7  0 21  4 18  0 21  4  3 20 18  0 11 11
```

The key for a Hill cipher is a matrix that is invertible mod 26. For example, we saw above that the matrix
$$ A = \begin{bmatrix} 3 & 2 \\ 1 & 7 \end{bmatrix} $$
has determinant 19, which is invertible mod 26, so the matrix inversion theorem tells us that $A$ is an invertible matrix mod 26; we can use this as our key. 

We are now going to go through our list of numbers, and replace each pair of numbers with the result of multiplying that pair by the matrix $A$ mod 26. For example, for the first pair, 24 and 14, we make a vector by stacking these two numbers on top of each other: 

$$ v = \begin{bmatrix} 24 \\ 14 \end{bmatrix} $$

We then compute 

$$ Av \bmod{26} = \begin{bmatrix} 3 & 2 \\ 1 & 7 \end{bmatrix} \begin{bmatrix} 24 \\ 14 \end{bmatrix} \bmod{26} = \begin{bmatrix} 100 \\ 122 \end{bmatrix} \bmod{26} = \begin{bmatrix} 22 \\ 18 \end{bmatrix} $$

In other words, the first two numbers will be replaced by 22 and 18. In other words, the first two letters of the message will be replaced by `W` and `S`, respectively. 

We then do the same thing with the next pair of numbers, 20 and 7. We get 22 and 17, so the third and fourth letters are replaced by `W` and `R`. We keep going like this until we reach the very end. In this particular case, we have an odd number of letters, so we should tack on a random letter at the end. For example, maybe we tack on the letter `Z`, which corresponds to 25. The last pair of numbers is then 11 and 25. Multiplying by $A$ mod 26 gives us 5 and 4, so we replace the last two letters with `F` and `E`. 

The net result is the ciphertext `WSWRQRWAQRSZSQWZFE`. 

As you might expect, to decrypt, we multiply pairs of numbers by the *inverse* of $A$ mod 26. For example, suppose we start with the ciphertext we computed above. We look at the associated list of numbers: 

```
 W  S  W  R  Q  R  W  A  Q  R  S  Z  S  Q  W  Z  F  E
22 18 22 17 16 17 22  0 16 17 18 25 18 16 22 25  5  4
```

We saw above that 

$$ X = \begin{bmatrix} 25 & 4 \\ 15 & 7 \end{bmatrix} $$

is an inverse of $A$, so now we're going to repeat the same process we went through during encryption, but now using the matrix $X$ in place of the matrix $A$. We start with the vector

$$ v = \begin{bmatrix} 22 \\ 18 \end{bmatrix} $$

consisting of the first two numbers. We then compute

$$ Xv \bmod{26} = \begin{bmatrix} 25 & 4 \\ 15 & 7 \end{bmatrix}\begin{bmatrix} 22 \\ 18 \end{bmatrix} \bmod{26} = \begin{bmatrix}
622 \\ 456 \end{bmatrix} \bmod{26} = \begin{bmatrix} 24 \\ 14 \end{bmatrix}.$$

Since 24 and 14 correspond to `Y` and `O` respectively, the first two letters of the decrypted message will be `YO` (as we'd expect). We keep going like this through the entire ciphertext to recover the plaintext. Of course, we'll find the message `YOUHAVESAVEDUSALLZ` at the end, and Bob has to remember that the `Z` was probably a random letter tacked onto the end. 

<div class="element">
<span class="label">Exercise</span>
Use the matrix
$$ A = \begin{bmatrix} 3 & -1 \\ 2 & 5 \end{bmatrix} $$
as the key for a Hill cipher. 

* Encrypt the message `Go to Lake Lerna`. 
* Decrypt the message `RNCQYVFRRLZI`. 
</div>

<div class="element" id="sagecell-hill-cipher">
<span class="label">SageCell</span>
Here is a Hill cipher to play with. 
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

# Groups a list pairwise
def pairwise(iterable):
    a = iter(iterable)
    return zip(a, a)

# Collapse a list of pairs into a list
def depairwise(iterable):
    return list(sum(iterable, ()))

# Encrypt a string using the given matrix    
def encrypt(text: str, A):
    nums = numerify(text)
    if len(nums) % 2 == 1:
        nums.append(randint(0, 25))
    transformed = [tuple(A*vector(v) % 26) for v in pairwise(nums)]
    transformed = depairwise(transformed)
    return denumerify(transformed)

# Decrypt a string using the given matrix  
def decrypt(text: str, A):
    if len(text) % 2 == 1:
        raise Exception("Input to decrypt must have even length.")
    nums = numerify(text)
    # Compute an inverse B of the matrix A
    B = A.change_ring(IntegerModRing(26)).inverse().lift()
    # Transform pairwise
    transformed = [tuple(B*vector(v) % 26) for v in pairwise(nums)]
    transformed = depairwise(transformed)
    return denumerify(transformed)
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(A=input_box(default="[[3, 2], [1, 7]]", label="A", width=40), 
      text=input_box(default="YOUHAVESAVEDUSALL", label="Input", height=5, width=80), 
      actions=selector(["encrypt", "decrypt"], buttons=True, label="Action")):
    A = matrix(A)
    if gcd(det(A), 26) != 1:
        raise Exception("Matrix A must be invertible mod 26.")
    
    output = eval(actions)(text, A)
    output_div("Output", f'<textarea readonly rows="5" cols="80">{ output }</textarea>')
</script>
</div>
</div>
