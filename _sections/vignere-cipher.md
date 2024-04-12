The *Vignère cipher* is our first example of polyalphabetic substitution: ie, a substitution cipher in which the substitution scheme changes over the course of the message. It is closely related to the Caesar cipher and was first described by Giovan Battista Bellaso in 1553; it carries the name of Blaise de Vignère due to a misattribution in the 1800s. The cipher resisted all attempts to break it for three centuries, until work of Friedrich Kasiski in 1863. 

The Vignère cipher makes use of modular arithmetic and of the correspondence between the letters `A` through `Z` and the numbers 0 through 25 that we've seen above. The key for a Vignère cipher is a finite sequence of shifts. A convenient and "easy-to-remember" way of constructing such a sequence is to have a secret key word, and then associate each letter of that word with the corresponding number to get the sequence of shifts. For example, if our secret word is `ASGARD`, the corresponding sequence of numbers is $(0, 18, 6, 0, 17, 3)$, because `A` corresponds to 0, `S` corresponds to 18, etc. 

To encrypt a message, we start by encoding it in the usual way: remove all non-alphabet characters and capitalize everything. Suppose, for example, that we want to encrypt the message `Keep Loki Away`. We encode to `KEEPLOKIAWAY` and then we associate to each letter the corresponding number 0 through 25. 

```
 K  E  E  P  L  O  K  I  A  W  A  Y
10  4  4 15 11 14 10  8  0 22  0 24
```

We will then perform addition mod 26 to each of these numbers. We'll use the first element of our key sequence for the first number, the second for the second, and so forth; when we finish the key, we repeat it from the beginning. We then convert those sums back to numbers using the usual correspondence. Using the key $(0, 18, 6, 0, 17, 3)$ corresponding to the key word `ASGARD` from above, we have: 

```
Encoded          :  K  E  E  P  L  O  K  I  A  W  A  Y
Numbers (1)      : 10  4  4 15 11 14 10  8  0 22  0 24
Key Word         :  A  S  G  A  R  D  A  S  G  A  R  D
Key Number (2)   :  0 18  6  0 17  3  0 18  6  0 17  3
(1) + (2) mod 26 : 10 22 10 15  2 17 10  0  6 22 17  1
Encrypted        :  K  W  K  P  C  R  K  A  G  W  R  B
```

Thus `KWKPCRKAGWRB` is our ciphertext. Notice that the first `E` of the plaintext was encrypted to `W`, while the second `E` was encrypted to `K`. Similarly, the first `A` was encrypted to `G`, while the second `A` was encrypted to `R`. (Coincidentally, the first `K` and the second `K` are both encrypted to the letter `K`.) This is what is meant when we say that the Vignère cipher is polyalphabetic, or that the substitution scheme changes over the course of the message. 

Decryption, of course, works almost exactly the same way --- except that we subtract mod 26 instead of adding!

<div class="element">
<span class="label">Exercise</span>
Continue to use the key word `ASGARD`. 

* Encrypt the message `Protect Odin from Fenrir`. 
* Decrypt the message `RSMNRUOCOSTRMATG`. 
</span>
</div>


<div class="element" id="sagecell-vignere-cipher">
<span class="label">SageCell</span>
Here is a SageCell implementing the Vignère cipher. 
<div class="sage">
<script type="text/x-sage">
from re import sub
from itertools import cycle

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

# Encrypt a string using the given key sequence    
def encrypt(text: str, key: list):
    nums = numerify(text)
    transformed = [(x + shift) % 26 for x, shift in zip(nums, cycle(key))]
    return denumerify(transformed)

# Decrypt a string using the given key sequence  
def decrypt(text: str, key: list):
    if not text.isalpha():
        raise Exception("Argument to decrypt must consist of alphabet characters.")
    nums = numerify(text.upper())
    transformed = [(x - shift) % 26 for x, shift in zip(nums, cycle(key))]
    return denumerify(transformed)

# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(keyword=input_box(default="ASGARD", label="Key", height=2, width=70), 
      text=input_box(default="TZURYDSKRAZQTZKSVUPWTT", label="Input", height=5, width=70), 
      actions=selector(["decrypt", "encrypt"], buttons=True, label="Action")):
    key = numerify(keyword)
    output = eval(actions)(text, key)
    output_div("Output", f'<textarea readonly rows="5" cols="70">{ output }</textarea>')
</script>
</div>
</div>




