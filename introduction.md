---
title: Introduction
---

# Introduction {#intro}

## Brief History

The word *cryptography* comes to us from the Ancient Greek words κρυπτός (kruptós), meaning "hidden, secret," and γραφή (graphḗ), meaning "writing." It refers broadly to a variety of techniques for secure communication. 

Cryptography has a long [history](https://en.wikipedia.org/wiki/History_of_cryptography). The earliest recorded use of cryptography dates to about 1900 BCE, in Egypt. Further records of cryptography are found in classical works of people from many other regions around the world, including Greece, Arabia, Persia, and India. The first known printed books on cryptography are *Steganographia* and *Polygraphiae* by [Johannes Trithemius](https://en.wikipedia.org/wiki/Johannes_Trithemius) (1462--1516). His work included occult writing systems, like the [Theban alphabet](https://en.wikipedia.org/wiki/Theban_alphabet) depicted below. 

<figure>
<img src="https://upload.wikimedia.org/wikipedia/commons/c/c6/Theban.jpg"/>
</figure>

Cryptography advanced dramatically with the advent of computers, and it has become ubiquitous in the information age. We are constantly sending sensitive information (passwords, bank information, credit card numbers, etc) over the internet, and all of these communications are protected by suites of cryptographic protocols. It is not much of an overstatement to say that contemporary human society would not exist without cryptography! 

## Exercises

You'll find exercises scattered throughout the text. Do them when you run into them! Doing exercises is crucial to actually developing an understanding of the concepts. 

## SageCells

As mentioned above, many of the most exciting parts of cryptography only became possible after the advent of computers. There's a good reason for this: cryptography relies crucially on computations that are difficult to do, so trying to do the calculations by hand frequently gets very tedious! 

You don't need to have prior programming experience for this course. But, to make things interesting and interactive, this text incorporates "SageCells." Sage is a thin wrapper for the programming language Python that makes mathematical computations especially convenient. A SageCell contains some code, and hitting "Run" below the code will run the code and display the output of the last line. 

The code in a SageCell can be edited, and I *urge* you to play with these code snippets, even if you don't know how to program! Try tweaking things and see what happens. Part of the reason for the popularity of Sage/Python is that it's a very beginner-friendly programming language, and the best way to learn to program is to just try things. If you break something, no worries: just hit reload and try again. 

I promise it'll be fun!

<div class="element">
<span class="label">SageCell</span>
Here is your first SageCell, which introduces you to basic arithmetic in Sage. Hit "Run" to compute `1+1`. Then try replacing the code with things like `100-2` or `2*6` or `6/4` or `2^100`. You'll have to hit "Run" after each time you change the code. 

<div class="sage">
<script type="text/x-sage">
1+1
</script>
</div>

You may or may not be disappointed in the output of `6/4`. Division is a little quirky in Sage (and different from vanilla Python) because Sage prefers keeping things exact. If you really want a decimal output, you can put a decimal somewhere in your input: try typing in something like `6.0/4`.
</div>

<div class="element">
<span class="label">SageCells</span>
Here are three more things to know about Sage for now. First, comments: lines beginning with a `#` sign are "comments" and don't do anything. They're used to write human-readable sentences in there to explain what's going on. For example: 
<div class="sage">
<script type="text/x-sage">
# The following line of code adds 1 to itself
1+1
</script>
</div>
Second, variables: you can assign and work with variables. For example, in the following code, the first line assigns the value 2 to the variable `x`, and we then add 15 to `x`.
<div class="sage">
<script type="text/x-sage">
x = 2
x + 15
</script>
</div>
Third, functions: you can define functions which take something as input and output something based on that input. Python and Sage come with a lot of useful pre-built functions, but it's still often useful to define some of your own. For a weird example, suppose you want to `add_fifteen` to be a function that adds 15 to the input. You would do this as follows: 
<div class="sage">
<script type="text/x-sage">
# This is how you define a function which
# takes the variable x as input and returns x + 15. 
def add_fifteen(x):
    return x + 15

# This is how you call the function you've just defined
add_fifteen(2)
</script>
</div>
You can name your variables and functions whatever you like, but it's usually best to give them names that describe what they do. 
</div>

## Terminology

This section contains a whole slew of words. This is a little tedious, but fixing terminology at the outset will help us communicate better and identify patterns as we go through the course. 

A cryptographic method for confidential communication is called a *cryptosystem* or a *cipher*. It includes algorithms for *encryption* and *decryption*, which are inverse processes that convert between plainly readable information called *plaintext* and unintelligible information called *ciphertext*. A *sender*, often named "Alice" in abstract cryptographic discussions, encrypts her plaintext into ciphertext. She then sends the ciphertext to *receiver*, often named "Bob," who must then decrypt (or decipher) the ciphertext back into plaintext. Bob will often use a *key* involved during decryption (also called a *private key* or a *decryption key*). 

Before encryption, there will usually be a preliminary step where the message is converted into a format which can then be encrypted; this is called *encoding*. Encoded text is not secure; it is only secure after encryption. If a message had to be encoded before encryption, it will also have to be *decoded* after decryption. Again, this is usually easy. Be careful: the words "encode" and "encrypt" sound similar, but mean different things in technical parlance!

Of course, we would like for it to be the case that only Bob can decrypt the ciphertext.[^only-bob] If the ciphertext is intercepted in transit by *adversary* (often named "Eve") who does not have access to Bob's decryption key at the beginning but who does know what cryptosystem was used to encrypt the message, the hope is that she should be unable to decrypt the message. If she manages to figure out the plaintext, she has *broken* the code! 

[^only-bob]: Well, maybe it's also okay for Alice to be able to decrypt the ciphertext, but Alice *already knows* the original plaintext, so maybe this goes without saying. 

<figure>
<svg width="500" height="350">
<defs>
    <marker
      id="arrow"
      viewBox="0 0 10 10"
      refX="5"
      refY="5"
      markerWidth="6"
      markerHeight="6"
      orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" />
    </marker>
</defs>
<circle cx="100" cy="100" r="30" 
    stroke="#444"
    fill="none" />
<!-- Alice: x=cx, y=cy -->
<text x="100" y="100"
    text-anchor="middle"
    dominant-baseline="middle">
    Alice
</text>
<!-- PT: x=cx-2r -->
<rect x="40" y="50" width="15" height="20" 
    stroke="#444" fill="none"/>
<!-- CT: x=cx+2r-width -->
<rect x="145" y="50" width="15" height="20" 
    stroke="#444" fill="#444"/>
<!-- encrypt-line: y=y+10, xstart=PTx+25 xend=CTx-10 -->
<polyline
    points="65,60 135,60"
    fill="none"
    stroke="#444"
    marker-end="url(#arrow)" />
<!-- encrypt-text: y=encrypt-line-y-2.5 -->
<text x="100" y="55"
    text-anchor="middle"
    dominant-baseline="bottom"
    font-size="0.75em">
    encrypt
</text>
<circle cx="400" cy="100" r="30" 
    stroke="#444"
    fill="none" />
<text x="400" y="100"
    text-anchor="middle"
    dominant-baseline="middle">
    Bob
</text>
<rect x="340" y="50" width="15" height="20" 
    stroke="#444" fill="#444"/>
<rect x="445" y="50" width="15" height="20" 
    stroke="#444" fill="none"/>
<polyline
    points="365,60 435,60"
    fill="none"
    stroke="#444"
    marker-end="url(#arrow)" />
<text x="400" y="55"
    text-anchor="middle"
    dominant-baseline="bottom"
    font-size="0.75em">
    decrypt
</text>
<!-- send arrow-->
<polyline
    points="170,60 330,60"
    stroke="#444"
    marker-end="url(#arrow)" />
<text x="250" y="55"
    text-anchor="middle"
    dominant-baseline="bottom"
    font-size="0.75em">
    send
</text>
<circle cx="400" cy="300" r="30" 
    stroke="#444"
    fill="none" />
<text x="400" y="300"
    text-anchor="middle"
    dominant-baseline="middle">
    Eve
</text>
<rect x="340" y="250" width="15" height="20" 
    stroke="#444" fill="#444"/>
<rect x="445" y="250" width="15" height="20" 
    stroke="#444" fill="none"/>
<polyline
    points="365,260 435,260"
    stroke="#444" stroke-dasharray="2,2"
    marker-end="url(#arrow)" />
<text x="400" y="255"
    text-anchor="middle"
    dominant-baseline="bottom"
    font-size="0.75em">
    codebreak
</text>
<path d="M 180,60 C 270,60 230,260 330,260"
    stroke="#444"
    fill="none"
    marker-end="url(#arrow)" />
<text x="215" y="175"
    text-anchor="middle"
    dominant-baseline="bottom"
    font-size="0.75em">
    intercept
</text>
</svg>
</figure>

An *attack model* specifies what Eve is allowed to do in order to break the code. Here are some of the most common attack models: 

* Ciphertext-only attack: Eve must recover the plaintext using only the ciphertext.
* Known-plaintext attack: Eve may have access to some information about the plaintext (eg, knowledge of portions of the plaintext), and she may use this to recover the plaintext. 
* Chosen-plaintext attack: Eve can request or generate ciphertexts corresponding to any plaintext message of her choosing, and she can use this information to recover the plaintext. 

Classical cryptography was mostly concerned with assuring security against ciphertext-only and/or known-plaintext attacks. Modern cryptography tries to assure security against chosen-plaintext attacks as well. 

## Course Overview 

We'll start with a discussion of classical cryptosystems (like the Caesar cipher). Many of these are no longer secure, and we'll then discuss the mathematics of breaking these classical cryptosystems. Finally, we'll move on to modern cryptosystems (like RSA or elliptic curve cryptography). Along the way, we'll see lots of interesting mathematical theory that is useful in cryptography, including the basics of number theory, probability theory, and information theory. 

Let's get started!

