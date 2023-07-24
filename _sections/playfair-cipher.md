The *Playfair cipher* is another digraphic cipher, like the Hill cipher we just described above. It was invented by Charles Wheatstone in 1854, but is named after Lord Playfair, who promoted its use. The encryption and decryption is a little complicated to describe, but there is no sophisticated mathematics involved. 

The key for a Playfair cipher is a $5 \times 5$ grid of letters, each letter appearing exactly once. Of course, such a grid only fits 25 letters, but there are 26 letters in the English alphabet! The typical solution is to treat `I` and `J` as the same letter, which is what we will do. (You could instead imagine a variant where we use a $6 \times 6$ grid that includes all 26 letters and all 10 digits, like the one we used for a Polybius square but without the row and column labels.)

A convenient and "easy-to-remember" way of constructing such a grid is to start with a secret key word. Suppose, for example, that our key word is `ALPHABET`. We then start filling out our grid by writing out the letters of our key word across the rows, skipping over any letter that we've already written down. 

<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td>A</td>
<td>L</td>
<td>P</td>
<td>H</td>
<td>B</td>
</tr>

<tr>
<td>E</td>
<td>T</td>
<td> </td>
<td> </td>
<td> </td>
</tr>

<tr>
<td>&nbsp;</td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
</tr>

<tr>
<td>&nbsp;</td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
</tr>

<tr>
<td>&nbsp;</td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
</tr>
</table>

We then fill out the square by listing all of the remaining letters of the alphabet, skipping over anything we've already written down and remembering that `I` and `J` are the same. 

<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td>A</td>
<td>L</td>
<td>P</td>
<td>H</td>
<td>B</td>
</tr>

<tr>
<td>E</td>
<td>T</td>
<td>C</td>
<td>D</td>
<td>F</td>
</tr>

<tr>
<td>G</td>
<td>I</td>
<td>K</td>
<td>M</td>
<td>N</td>
</tr>

<tr>
<td>O</td>
<td>Q</td>
<td>R</td>
<td>S</td>
<td>U</td>
</tr>

<tr>
<td>V</td>
<td>W</td>
<td>X</td>
<td>Y</td>
<td>Z</td>
</tr>
</table>

We next encode our message by doing the following: 

1. As usual, we remove all non-alphabet characters and capitalize everything.
2. Replace all instances of `J` with `I`. 
3. Group the letters into pairs. 
4. If there are any pairs where both letters are the same, we insert the letter `X` in between the two letters of that pair and then regroup into pairs. 
5. If there is an unpaired letter at the end, insert the letter `X` after it. 

Suppose, for example, that we want to encrypt the message `hidden jewels in trees`. Here is what the encoding looks like after each step: 

1. `HIDDENJEWELSINTHETREES`
2. `HIDDENIEWELSINTHETREES`
3. `HI DD EN IE WE LS IN TH ET RE ES`
4. `HI DX DE NI EW EL SI NT HE TR EE S`  
   `HI DX DE NI EW EL SI NT HE TR EX ES`
5. `HI DX DE NI EW EL SI NT HE TR EX ES`

Note that we might have to apply rule 4 multiple times. We had to apply rule 4 twice above (first to eliminate the `DD`, and then to replace the `EE`). 

To encrypt, we replace each pair with another pair using the grid by following these rules: 

* (Row Rule) If both letters in the pair occur in the same row, replace each letter of the pair with the letter that appears immediately to its right (wrapping around to the left side of the row if needed). 
* (Column Rule) If both letters in the pair occur in the same column, replace each letter of the pair with the letter that appears immediately below it (wrapping around to the top of the column if needed). 
* (Rectangle Rule) Otherwise, the two letters define a rectangle inside the grid, and we replace each letter with the letter on the same row but the opposite side of that rectangle. 

Returning to our example: we start with the pair `HI`. These are marked in bold below; they do not appear in the same row or column, so the Rectangle Rule applies. The rectangle defined by these two letters is indicated in black. 

<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td style="color: #dddddd">A</td>
<td>L</td>
<td>P</td>
<td>**H**</td>
<td style="color: #dddddd">B</td>
</tr>

<tr>
<td style="color: #dddddd">E</td>
<td>T</td>
<td>C</td>
<td>D</td>
<td style="color: #dddddd">F</td>
</tr>

<tr>
<td style="color: #dddddd">G</td>
<td>**I**</td>
<td>K</td>
<td>M</td>
<td style="color: #dddddd">N</td>
</tr>

<tr>
<td style="color: #dddddd">O</td>
<td style="color: #dddddd">Q</td>
<td style="color: #dddddd">R</td>
<td style="color: #dddddd">S</td>
<td style="color: #dddddd">U</td>
</tr>

<tr>
<td style="color: #dddddd">V</td>
<td style="color: #dddddd">W</td>
<td style="color: #dddddd">X</td>
<td style="color: #dddddd">Y</td>
<td style="color: #dddddd">Z</td>
</tr>
</table>

The letter in the same row as `H` but the opposite side is `L`, and the letter in the same row as `I` but the opposite side is `M`. Thus `HI` gets replaced by `LM`. 

Next we encrypt `DX`. These also define a rectangle, as depicted below, and get replaced by `CY`. 

<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td style="color: #dddddd">A</td>
<td style="color: #dddddd">L</td>
<td style="color: #dddddd">P</td>
<td style="color: #dddddd">H</td>
<td style="color: #dddddd">B</td>
</tr>

<tr>
<td style="color: #dddddd">E</td>
<td style="color: #dddddd">T</td>
<td>C</td>
<td>**D**</td>
<td style="color: #dddddd">F</td>
</tr>

<tr>
<td style="color: #dddddd">G</td>
<td style="color: #dddddd">I</td>
<td>K</td>
<td>M</td>
<td style="color: #dddddd">N</td>
</tr>

<tr>
<td style="color: #dddddd">O</td>
<td style="color: #dddddd">Q</td>
<td>R</td>
<td>S</td>
<td style="color: #dddddd">U</td>
</tr>

<tr>
<td style="color: #dddddd">V</td>
<td style="color: #dddddd">W</td>
<td>**X**</td>
<td>Y</td>
<td style="color: #dddddd">Z</td>
</tr>
</table>

Next we have `DE`. These two letters are on the same row, indicated in black below. We therefore apply the Row Rule and replace each one with the letter immediately to its right, obtaining `FT`. 

<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td style="color: #dddddd">A</td>
<td style="color: #dddddd">L</td>
<td style="color: #dddddd">P</td>
<td style="color: #dddddd">H</td>
<td style="color: #dddddd">B</td>
</tr>

<tr>
<td>**E**</td>
<td>T</td>
<td>C</td>
<td>**D**</td>
<td>F</td>
</tr>

<tr>
<td style="color: #dddddd">G</td>
<td style="color: #dddddd">I</td>
<td style="color: #dddddd">K</td>
<td style="color: #dddddd">M</td>
<td style="color: #dddddd">N</td>
</tr>

<tr>
<td style="color: #dddddd">O</td>
<td style="color: #dddddd">Q</td>
<td style="color: #dddddd">R</td>
<td style="color: #dddddd">S</td>
<td style="color: #dddddd">U</td>
</tr>

<tr>
<td style="color: #dddddd">V</td>
<td style="color: #dddddd">W</td>
<td style="color: #dddddd">X</td>
<td style="color: #dddddd">Y</td>
<td style="color: #dddddd">Z</td>
</tr>
</table>

And then we encrypt `NI`. These also lie on the same row, indicated in black below, so we apply the row rule and replace each letter with the letter immediately to its right. For the letter `N`, we have to wrap around to the beginning to `G`. Thus `NI` gets replaced by `GK`. 

<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td style="color: #dddddd">A</td>
<td style="color: #dddddd">L</td>
<td style="color: #dddddd">P</td>
<td style="color: #dddddd">H</td>
<td style="color: #dddddd">B</td>
</tr>

<tr>
<td style="color: #dddddd">E</td>
<td style="color: #dddddd">T</td>
<td style="color: #dddddd">C</td>
<td style="color: #dddddd">D</td>
<td style="color: #dddddd">F</td>
</tr>

<tr>
<td>G</td>
<td>**I**</td>
<td>K</td>
<td>M</td>
<td>**N**</td>
</tr>

<tr>
<td style="color: #dddddd">O</td>
<td style="color: #dddddd">Q</td>
<td style="color: #dddddd">R</td>
<td style="color: #dddddd">S</td>
<td style="color: #dddddd">U</td>
</tr>

<tr>
<td style="color: #dddddd">V</td>
<td style="color: #dddddd">W</td>
<td style="color: #dddddd">X</td>
<td style="color: #dddddd">Y</td>
<td style="color: #dddddd">Z</td>
</tr>
</table>

<div class="element">
<span class="label">Exercise</span>
Using the same grid
<table width="30%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<td>A</td>
<td>L</td>
<td>P</td>
<td>H</td>
<td>B</td>
</tr>

<tr>
<td>E</td>
<td>T</td>
<td>C</td>
<td>D</td>
<td>F</td>
</tr>

<tr>
<td>G</td>
<td>I</td>
<td>K</td>
<td>M</td>
<td>N</td>
</tr>

<tr>
<td>O</td>
<td>Q</td>
<td>R</td>
<td>S</td>
<td>U</td>
</tr>

<tr>
<td>V</td>
<td>W</td>
<td>X</td>
<td>Y</td>
<td>Z</td>
</tr>
</table>

finish encrypting the encoded text: 

```
HI DX DE NI EW EL SI NT HE TR EX ES
LM CY FT GK __ __ __ __ __ __ __ __
```
 
</div>

<div class="element">
<span class="label">Exercise</span>
Using the same grid, encode and encrypt the following messages:

a. `Meet at aviary`
b. `Launch balloon at noon`
</div>

To decrypt messages, we have to undo this process. We follow almost the same three rules, except the Column Rule for decryption involves replacing each letter that appears immediately *above* it, and the Row Rule involves replacing each letter with the letter that appears immediately to the *left* of it. The recipient also has to decode the message by inferring which `X`s should be removed and how to correctly punctuate the message.  

<div class="element">
<span class="label">Exercise</span>
Using the same grid, decrypt and decode the message `OV CQ QI UG RY`.
</div>

<div class="element" id="sagecell-playfair-cipher">
<span class="label">SageCell</span>
Here's a SageCell, contributed by Rahul Avasarala, that implements a Playfair cipher.
<div class="sage">
<script type="text/x-sage">
# Code contributed by Rahul Avasarala
from re import sub

# All capital letters except J
letters = "ABCDEFGHIKLMNOPQRSTUVWXYZ"

# Remove all non alphabetic characters and capitalize
def encode(text: str):
    stripped = sub(r"[^a-zA-Z]", "", text)
    return stripped.upper()

# Remove all non alphabetic characters, capitalize, and replace J with I
def encode_and_remove_J(text: str):
    return sub("J", "I", encode(text))

# Encode the text to prepare for Playfair encryption
# Use the repeat_break letter to break repeats (default: X)
def playfair_encode(text: str, repeat_break = "X"):
    if repeat_break not in list(letters):
        raise Exception("repeat_break must be a capital letter except J")
    
    text = encode_and_remove_J(text)
    i = 0
    out = ""
    while i < len(text):
        if i == len(text) - 1:
            out += text[i] + repeat_break
            i += 1
        elif text[i] == text[i + 1]:
            out += text[i] + repeat_break
            i += 1
        else:
            out += text[i:i+2]
            i += 2       
    return out

# Convert interger locations to coordinates on a 5x5 board
def convert_int_to_coord(num):
    if num < 0 or num >= 25:
        raise Exception("num must be < 25")
    return (num // 5, num % 5)

# Generate a Playfair key matrix given a key word
def generate_key(word):
    word = encode_and_remove_J(word) + letters
    seen = set()
    key = [x for x in word if x not in seen and not seen.add(x)]
    
    # Make a dict of letter to coordinate correspondences
    letter_to_coord = {x: convert_int_to_coord(i) for i, x in enumerate(key)}
    # Make a dict of coordinate to letter correspondences
    coord_to_letter = {convert_int_to_coord(i): x for i, x in enumerate(key)}
    
    return letter_to_coord, coord_to_letter
    
# Transform a pair
def transform_pair(pair, letter_to_coord, coord_to_letter, encrypt = True):
    a, b = pair
    ca, cb = letter_to_coord[a], letter_to_coord[b]

    t = 1 if encrypt else -1
    if ca[0] == cb[0]:
        cx = (ca[0], (ca[1] + t) % 5)
        cy = (cb[0], (cb[1] + t) % 5)
    elif ca[1] == cb[1]:
        cx = ((ca[0] + t) % 5, ca[1])
        cy = ((cb[0] + t) % 5, cb[1])
    else:
        cx = (ca[0], cb[1])
        cy = (cb[0], ca[1])

    return coord_to_letter[cx] + coord_to_letter[cy]
    
# Groups a list pairwise
def pairwise(iterable):
    a = iter(iterable)
    return zip(a, a)

# Encrypt the text
def encrypt(text, letter_to_coord, coord_to_letter):
    text = playfair_encode(text)
    transformed_pairs = [transform_pair(x, letter_to_coord, coord_to_letter) for x in pairwise(text)]
    return "".join(transformed_pairs)
    
# Decrypt the text
def decrypt(text, letter_to_coord, coord_to_letter):
    if len(text) % 2 == 1 or not text.isalpha() or not text.isupper() or "J" in text:
        raise Exception("invalid ciphertext")
    transformed_pairs = [transform_pair(x, letter_to_coord, coord_to_letter, encrypt = False) for x in pairwise(text)]
    return "".join(transformed_pairs)

# Create an output div
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(keyword=input_box(default="ALPHABET", label="Keyword", height=5, width=80),
      text=input_box(default="Hidden Jewels in the Trees!!!!!", label="Input", height=5, width=80),
      actions=selector(["encrypt", "decrypt"], buttons=True, label="Action")):
    
    letter_to_coord, coord_to_letter = generate_key(keyword)
    s = "<table style='text-align: center;' width='25%'>"
    for i in range(5):
        s += "<tr>"
        for j in range(5):
            s += f"<td>{coord_to_letter[i,j]}</td>"
        s += "</tr>"
    output_div("Grid", s)
    
    output = eval(actions)(text, letter_to_coord, coord_to_letter)
    output_div("Output", f'<textarea readonly rows="5" cols="80">{output}</textarea>')
</script>
</div>
</div>

