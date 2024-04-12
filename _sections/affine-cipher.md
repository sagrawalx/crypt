We've seen that, if we identify the letters `A` through `Z` in order with the numbers 0 through 25 in order, the encryption function for the Caesar cipher is $E(x) = (x + b) \bmod{26}$, where $b = 0, 1, \dotsc, 25$ is the shift. We now generalize this. An affine cipher is one whose encryption function is of the form 

$$ E(x) = (ax + b) \bmod{26} $$

for some integers $a$ and $b$, which form the key. As we will see shortly, not any pair of integers $a$ and $b$ will result in an affine encryption function above, but let us first consider an example. Suppose $a = 3$ and $b = 5$, so that our encryption function is $E(x) = (3x + 5) \bmod{26}$. To encrypt the letter `Y`, we compute 

$$ E(24) = (3 \cdot 24 + 5) \bmod{26} = (72 + 5) \bmod{26} = 77 \bmod{26} = 25. $$

Thus the encryption of `Y` is the letter `Z`, which corresponds to 25. 

<div class="element">
<span class="label">Exercise</span>
Keeping the same encryption function as above with $a = 3$ and $b = 5$, what is the encryption of `A`? What is the encryption of `D`?
</div>

How does decryption work? Let's start by stating the general result. 

<div class="element">
<span class="label">Affine Cipher Lemma</span>
Suppose $E : \{0, \cdots, 25\} \to \{0, \cdots, 25\}$ is a function of the form 
$$ E(x) = ax + b \bmod{26} $$
for some integers $a$ and $b$. Then there exists a function $D : \{0, \cdots, 25\} \to \{0, \cdots, 25\}$ such that $D(E(x)) = x$ if and only if $a$ is invertible mod 26. Moreover, if $c \equiv a^{-1} \pmod{26}$, then  
$$ D(y) = c(y-b) \bmod{26}. $$
</div>

Let's go back to our example with $a = 3$ and $b = 5$. Using `inverse_mod(3, 26)` in Sage, I find that $9 \equiv 3^{-1} \pmod{26}$, so the Affine Cipher Lemma tells us that the decryption function must be given by 

$$ D(y) = 9(y - 5) \bmod{26}. $$

For example, suppose I want to decrypt the letter `Z`, which corresponds to $y = 25$. We have $D(25) = 9(25 - 5) \bmod{26} = 9 \cdot 20 \bmod{26} = 180 \bmod{26} = 24$, which corresponds to `Y`, exactly as we would have expected!

<div class="element">
<span class="label">Exercise</span>
Alice and Bob are using the same affine encryption function as above, with $a = 3$ and $b = 5$. Bob has just received the message `LNKRLFKH`. Decrypt the message. 
</div>

<div class="element">
<span class="label">Exercise</span>
Suppose the encryption function for an affine cipher is $E(x) = 5x + 17 \bmod{26}$. What is the corresponding decryption function $D$? 
</div>

<div class="element">
<span class="label">Proof Exercise</span>
Prove the Affine Cipher Lemma.  
</div>

Notice that, of the numbers between 0 and 25, there are exactly 12 that are invertible mod 26 (the odds except 13, ie, 1, 3, 5, 7, 9, 11, 15, 17, 19, 21). This means that the number of pairs $(a, b)$ such that $E(x) = ax + b \bmod{26}$ is a legitimate encryption function for an affine cipher is $12 \cdot 26 = 312$. This is quite a bit larger than 26, and is probably already large enough that it would be a little tedious to try and check all possible keys to see if any result in something meaningful. That would probably only be worth doing if you knew the message had some very valuable information. 

In fact, we can increase the number of keys with a simple substitution cipher even more dramatically, as we'll see next. Meanwhile, here's a SageCell to play with!

<div class="element" id="sagecell-affine-cipher">
<span class="label">SageCell</span>
Alice has just sent you the message `VYOHVUTWVHPZUWTZUUBEYDUB`, encrypted using the affine cipher with $a = 5$ and $b = 7$. Here is some code that implements the affine cipher. What is the message?
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

# Encrypt a string using the given a and b    
def encrypt(text: str, a: int, b:int):
    nums = numerify(text)
    transformed = [(a*x + b) % 26 for x in nums]
    return denumerify(transformed)

# Decrypt a string using the given a and b  
def decrypt(text: str, a: int, b: int):
    if not text.isalpha():
        raise Exception("Argument to decrypt must consist of alphabet characters.")
    nums = numerify(text.upper())
    c = inverse_mod(a, 26)
    transformed = [c*(x-b) % 26 for x in nums]
    return denumerify(transformed)
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(a=slider(0, 25, 1, 1, label="a"),
      b=slider(0, 25, 1, 0, label="b"),
      text=input_box(default="VYOHVUTWVHPZUWTZUUBEYDUB", label="Input", height=5, width=70), 
      actions=selector(["decrypt", "encrypt"], buttons=True, label="Action")):
    
    if gcd(a, 26) != 1:
        raise Exception("a must be invertible mod 26")
    
    output = eval(actions)(text, a, b)
    output_div("Output", f'<textarea readonly rows="5" cols="70">{ output }</textarea>')
</script>
</div> 
</div> 

<div class="element">
<span class="label">Exercise</span>
The *Atbash cipher* is a simple substitution cipher in which encryption and decryption both simply reverse the order of the alphabet. In other words, `A` and `Z` are interchanged, `B` and `Y` are interchanged, and so forth. For example, the plaintext `APPLE` corresponds to the ciphertext `ZKKOV`. Show that the Atbash cipher is a special case of the affine cipher. What are the corresponding values of $a$ and $b$? 
</div>

<div class="element">
<span class="label">Exercise</span>

a. Make sense of and justify the following statement: "Two affine ciphers in succession result in just another affine cipher." 
b. Is it possible for "two affine ciphers in succession" to result in a *Caesar* cipher? Explain. 
</div>

