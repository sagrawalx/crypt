The *Caesar cipher* (also called a *shift cipher*) is also a simple substitution cipher. The key for a Caesar cipher is a shift, which can be any integer. For example, if we choose a shift of 3, every letter gets shifted forward by 3 during encryption: `A` becomes `D`, `B` becomes `E`, and so forth. What happens with `X`? There is no letter that is 3 after `X`, but we simply cycle back to beginning so that `X` becomes `A`, `Y` becomes `B`, and `Z` becomes `C`. This is reversed during decryption. 

<div class="element" id="sagecell-caesar-knob">
<span class="label">SageCell</span>
It may be useful to understand a Caesar cipher in terms of a dial with 26 notches; hit "Run" below to see such a dial. Each notch is labeled with a letter of the alphabet, both on the dial itself (in black in the picture below) and outside the dial (in gray in the picture below), and the "shift" is the number of notches through which the dial is turned (clockwise in the picture below). To encrypt, we convert each gray letter to the corresponding black letter. To decrypt, we go backwards.
<div class="sage">
<script type="text/x-sage">
from functools import reduce

@interact
def _(shift=slider(-5, 30, 1, 3, label="Shift")):
    # Empty graphics object
    graph = Graphics()
    
    # Parameters
    letters = list(build_alphabet(name="upper"))
    outer = 10
    color = "#444444"
    lighter = "#777777"
    
    # Derived parameters
    delta = outer / 10
    inner = outer - delta
    thickness = 2 * delta
    
    # List of relevant angles (in radians)
    angles = srange(0, 2*pi, 2*pi/len(letters))
    
    # Function to generate points along circle at above angles, at a specified radius
    distribute = lambda r: [(r*cos(x), r*sin(x)) for x in angles]
    
    # Dial and notches on the dial
    dial = circle((0, 0), outer, color=color, thickness=thickness)
    notches = [line([start, end], color=color, thickness=thickness)
              for start, end in zip(distribute(inner), distribute(outer))]
    graph = reduce(operator.add, notches, graph) + dial
    
    # Outer labels
    outer_labels = [text(x, p, color=lighter) for x, p in zip(letters, distribute(outer+delta))]
    graph = reduce(operator.add, outer_labels, graph)
    
    # Inner labels
    shifted_letters = [letters[(i + shift) % len(letters)] for i in range(len(letters))]
    inner_labels = [text(x, p, color=color) for x, p in zip(shifted_letters, distribute(inner-delta))]
    theta = angles[-shift % len(letters)]
    orient = line([(0, 0), (6*delta*cos(theta), 6*delta*sin(theta))], color=color, thickness=thickness)
    graph = reduce(operator.add, inner_labels, graph) + orient

    # Show
    show(graph, axes=False)
</script>
</div>
Play with the slider to try out a few different shifts. What's going on with a shift of 0? What about with a shift of 26? How does a shift of 27 compare with a shift of 1? What do negative shifts mean? What positive shift is a shift of $-1$ the same as?
</div>

As with rectangular transposition, it is typical to first encode the message by removing all non-alphabetic characters and capitalizing everything. Thus a message like 

```
University of California San Diego
```

would get encoded to:

```
UNIVERSITYOFCALIFORNIASANDIEGO

```

and we then apply a shift of 3 to get the ciphertext:

```
XQLYHUVLWBRIFDOLIRUQLDVDQGLHJR
```

<div class="element">
<span class="label">Exercise</span>

a. Using a shift of 3, encrypt the message `Meet at La Jolla Shores`.
b. Using a shift of 3, decrypt the message `PHHWDWVXQJRGODZQ`. 
c. Using a shift of 10, encrypt the message `On the Ides of March`.
</div>

<div class="element">
<span class="label">Exercise</span>
You are Eve. You have just intercepted the following message that Alice was trying to send to Bob: 

`Q TQDM IB QPWCAM`

You know that Alice used a Caesar cipher, but she didn't remove spaces before encrypting: she left the spaces in her original message as-is. What is the original message? *Hint*. Apparently just the single letter `Q` is the encryption of a word; what word(s) could it be? 
</div>

It might feel a little tedious to do these encryptions and decryptions by hand, and it is. But computers can do this very quickly! To understand how, we pause our discussion of ciphers to discuss a fundamental idea in number theory: modular arithmetic. 

