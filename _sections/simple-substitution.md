We can describe a general *simple substitution cipher* (also called *simple monoalphabetic substitition cipher* or a *monoalphabetic substitution cipher*) by using a full conversion table as a key. For example, we might use a table like the following: 

<table style="display-style: block; margin: auto; text-align: center; font-family: monospace;">
<thead>
<tr class="header">
<th>A</th>
<th>B</th>
<th>C</th>
<th>D</th>
<th>E</th>
<th>F</th>
<th>G</th>
<th>H</th>
<th>I</th>
<th>J</th>
<th>K</th>
<th>L</th>
<th>M</th>
<th>N</th>
<th>O</th>
<th>P</th>
<th>Q</th>
<th>R</th>
<th>S</th>
<th>T</th>
<th>U</th>
<th>V</th>
<th>W</th>
<th>X</th>
<th>Y</th>
<th>Z</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>P</td>
<td>V</td>
<td>J</td>
<td>W</td>
<td>D</td>
<td>C</td>
<td>H</td>
<td>T</td>
<td>S</td>
<td>K</td>
<td>Z</td>
<td>F</td>
<td>N</td>
<td>Q</td>
<td>E</td>
<td>Y</td>
<td>O</td>
<td>R</td>
<td>I</td>
<td>G</td>
<td>A</td>
<td>U</td>
<td>M</td>
<td>L</td>
<td>X</td>
<td>B</td>
</tr>
</tbody>
</table>

This tells us to convert every instance of `A` into `P`, every instance of `B` into `V`, and so forth, and to go backwards during decryption. For example, suppose Alice wants to encrypt the message:

```
You must destroy all of the horcruxes!
```

She again starts by encoding the message by removing spaces and punctuation and capitalizing everything: 

```
YOUMUSTDESTROYALLOFTHEHORCRUXES
```

She then converts each letter using the table: 

```
XEANAIGWDIGREXPFFECGTDTERJRALDI
```

This is the ciphertext she sends to Bob. To decrypt the message, Bob uses the same table backwards. 

Notice that, if the entire table is our key, the number of possible keys is  

$$ 26! = 403291461126605635584000000, $$

ie, about 403 *septillion*. There are now *far* too many to try all of them! 

In spite of the massive increase in the number of keys, we'll see later that simple substitution can still be easily broken using some ideas from probability theory. Meanwhile, let's get some practice with this: 

<div class="element">
<span class="label">Exercise</span>
Using the same table given above, do the following by hand:  

* Encrypt the message `The moon is pitted with holes!`
* Decrypt the message `TEMPRDXEAWESQHGEWPX`. 
</div>

<div class="element" id="sagecell-simple-substitution">
<span class="label">SageCell</span>
Here's a SageCell that implements simple substitution. The text box labeled "Key" represents the *encryption* key: the top row corresponds to letters of plaintext and the bottom row to letters of ciphertext. Don't change anything in the first row. A plaintext letter in the top row corresponds to the ciphertext letter in the second row.
<div class="sage">
<script type="text/x-sage">
from re import sub

alphabet = list(build_alphabet(name="upper"))
default = "QEYORIGAUMLXBPVJWDCHTSKZFN"
key_start = "\n".join(["".join(alphabet), default])

# Remove all non alphabetic characters and capitalize
def encode(text: str):
    stripped = sub(r"[^a-zA-Z]", "", text)
    return stripped.upper()
    
# Validate the input key
def validate(input_key):
    if len(input_key) != len(key_start):
        raise Exception("Do not add or remove characters from the key")
    if input_key[:27] != key_start[:27]:
        raise Exception("Do not change the first row of the key")
    if len([x for x in input_key[27:] if x not in alphabet]):
        raise Exception(f"Every character in second row of key must be a capital letter")
    if len(set(input_key[27:])) != 26:
        raise Exception("Do not repeat letters in the second row")
    return {x: y for x, y in zip(alphabet, input_key[27:])}
        
# Encrypt a string using the given key    
def encrypt(text: str, key: dict):
    text = encode(text)
    return "".join([key[x] for x in list(text)])

# Decrypt a string using the given key  
def decrypt(text: str, key: dict):
    text = encode(text)
    reversed_key = { v: k for k, v in key.items() }
    return "".join([reversed_key[x] for x in list(text)])
    
# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(input_key=input_box(default=key_start, label="Key", height=2, width=70), 
      text=input_box(default="Hide! The baboons are coming for you.", label="Input", height=5, width=70), 
      actions=selector(["encrypt", "decrypt"], buttons=True, label="Action")):
    
    # Validate key
    key = validate(input_key)
    
    # Encrypt or decrypt
    output = eval(actions)(text, key)
    output_div("Output", f'<textarea readonly rows="5" cols="70">{ output }</textarea>')
</script>
</div>
</div>
