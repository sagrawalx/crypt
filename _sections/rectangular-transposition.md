*Rectangular transposition* (also called *regular columnar transposition*) is a transposition cipher: the ciphertext is obtained by permuting the letters of the plaintext in a particular pattern. The pattern is determined by a secret keyword. The strategy is best explained through example. 

Suppose that Alice and Bob share the keyword `GUARD`, and that Alice wants to send the following message to Bob: 

```
Hide! The baboons are coming for you.
```

As a first preliminary step, she removes spaces and punctuation and capitalizes all letters:

```
HIDETHEBABOONSARECOMINGFORYOU
```

This is the "encoded" message; notice that this is not yet remotely secure! It must still be encrypted. Since the keyword `GUARD` has 5 letters, she breaks up the message into 5-grams and stacks them into rows: 

```
HIDET
HEBAB
OONSA
RECOM
INGFO
RYOU
```

She then inserts nonsense letters at the end of the message to fill out any missing spaces: 

```
HIDET
HEBAB
OONSA
RECOM
INGFO
RYOUQ
```

Now she rearranges the letters in each row. Observe that the alphabetical rankings of the letters of the keyword `GUARD` are 3, 5, 1, 4, 2. So she rearranges the letters in each row by putting the first letter of each row in the 3rd position, the second letter of each row in the 5th position, the third letter of each row in the 1st position, the fourth letter of each row in the 4th position, and the fifth letter of each row in the 2nd position. 

```
DTHEI
BBHAE
NAOSO
CMROE
GOIFN
OQRUY
```

She then undoes the stacking to get the ciphertext: 

```
DTHEIBBHAENAOSOCMROEGOIFNOQRUY
```

This ciphertext is then sent to Bob, who can undo this same process using the keyword to recover the plaintext. Let's work this out. Bob first takes the letters of the ciphertext and stacks them into rows of 5, since `GUARD` has 5 letters: 

```
DTHEI
BBHAE
NAOSO
CMROE
GOIFN
OQRUY
```

Bob also knows that the alphabetical ranking of the letters of `GUARD` is 3, 5, 1, 4, 2. This means that, to decrypt the message, he has to move the 3rd letter of each row to the first position, the 5th letter to the second position, the 1st letter to the third position, the 4th letter to the fourth position, and the 2nd letter to the fifth position. 

```
HIDET
HEBAB
OONSA
RECOM
INGFO
RYOUQ
```

He then undoes the stacking:

```
HIDETHEBABOONSARECOMINGFORYOUQ
```

He then has to make an educated guess that the letter `Q` is nonsense and that the correctly punctuated message must be

```
Hide! The baboons are coming for you.
```

Now you try it!

<div class="element">
<span class="label">Exercise</span>
Suppose Alice and Bob share the keyword `CRASH`. Mimic the process above to do the following: 

* Encrypt the message `There is always hope`. 
* Decrypt the message `ETIHGFREAFRSLAESOXOE`.
</div>

Once you have a hang of doing this by hand with short examples, here's a SageCell you can play with to encrypt and decrypt longer messages using rectangular transposition. 

<div class="element" id="sagecell-rectangular-transposition">
<span class="label">SageCell</span>
There's a lot that may be new to you in the code here. Start by just hitting "Run" and playing with what you see; later, you might decide to play with the code to figure out how it works!
<div class="sage">
<script type="text/x-sage">
from re import sub

# Remove all non alphabetic characters and capitalize
def encode(text: str):
    stripped = sub(r"[^a-zA-Z]", "", text)
    return stripped.upper()
        
# Extract the permutation corresponding to a keyword
# Permutations of n elements are represented as lists 
# of the numbers 0 through n-1
def get_permutation(key: str):
    if not key.isalpha():
        raise Exception("Key must only contain alphabet characters.")  
    if len(set(key)) != len(key):
        raise Exception("Key must avoid repeat characters.")
    if not key.isupper():
        return get_permutation(key.upper())

    key = list(key)
    return [sorted(key).index(x) for x in key]
        
# Invert a given permutation
def invert(sigma: list):
    return [sigma.index(x) for x in range(len(sigma))]

# Return the string as a list, potentially with random extra uppercase characters 
# padded onto the end so that the length of the list is divisible by n
def pad(text: str, n: int):
    text = list(text)
    r = -len(text) % n
    letters = list(build_alphabet(name="upper"))
    padding = [letters[randint(0, len(letters)-1)] for i in range(r)]
    return text + padding

# Apply inverse of given permutation sigma to the string
def apply_inv_permutation(text: str, sigma: list):
    text = pad(text, len(sigma))
    permuted = []
    for x in range(0, len(text), len(sigma)):
        permuted += [text[x + sigma[i]] for i in range(len(sigma))]
    return "". join(permuted)

# Encrypt a string using the given key    
def encrypt(text: str, key: str):
    text = encode(text)
    sigma = invert(get_permutation(key))
    return apply_inv_permutation(text, sigma)

# Decrypt a string using the given key  
def decrypt(text: str, key: str):
    if len(text) % len(key) != 0:
        raise Exception("Invalid ciphertext: length of ciphertext is not a multiple of the length of the key.")
    if not text.isalpha():
        raise Exception("Invalid ciphertext: input contains non-alphabet characters")
    text = encode(text)
    sigma = get_permutation(key)
    return apply_inv_permutation(text, sigma)
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(key=input_box(default="CRASH", label="Key", height=2, width=80), 
      text=input_box(default="Hide! The baboons are coming for you.", label="Input", height=5, width=80), 
      actions=selector(["encrypt", "decrypt"], buttons=True, label="Action")):
    output = eval(actions)(text, key)
    output_div("Output", f'<textarea readonly rows="5" cols="80">{ output }</textarea>')
</script>
</div>
</div>

<div class="element">
<span class="label">Sage Exercise</span>
What does the SageCell above do when you try setting your key word to be something that has a repeated letter? Here's something different that that would be reasonable for the code to do when it encounters this situation: it could decide to drop any repeats from the key word before proceeding with the encryption. For example, if the user enters `BANANA`, it would instead use `BAN` to encrypt the message. Edit the code above so that this happens!
</div>

