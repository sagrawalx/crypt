We start with some classical cryptosystems. It's useful to group these into a few different types. To do this, let's introduce some more terminology before getting started. An *$n$-gram* is a sequence of $n$ letters: a 1-gram is just a single letter, a 2-gram (also called a *bigram*) is a pair of letters, and so forth. The encryption strategies we'll encounter below are the following: 

* *Transposition* involves rearranging units of plaintext according to some pattern. We'll see just one example of this type of cipher below: rectangular transposition. 
    
* *Substitition* involves replacing units of plaintext with units of ciphertext. Substitution ciphers can be grouped further into some subtypes: 
    
    - Simple substitution: In these ciphers, single letters of plaintext are replaced by ciphertext. The substitution scheme stays the same over the course of the entire message. We've already seen a simple substitution above: Trithemius's Theban alphabet. The simple substitition ciphers we'll meet below are: 
        
        - Masonic cipher. 
        - Caesar cipher.
        - Affine cipher. 
        - Polybius square. 
    
    - Polygraphic substitution: In these ciphers, groups of letters in the plaintext are replaced by ciphertext (a group of $n$ letters is called an "$n$-gram"). The substitution scheme stays fixed over the entire message. The polygraphic substitition ciphers we'll meet below are: 
        
        - Hill cipher. 
        - Playfair cipher. 
        
    - Polyalphabetic substitution: In these ciphers, single letters in the plaintext are replaced by ciphertext, and the substitution scheme changes over the course of the message. The polyalphabetic substitition ciphers we'll meet below are: 
    
        - Vignère cipher.
        - One-time pad.

Most cryptosystems used in practice employ a combination of these strategies. For example, we'll meet the ADFGVX cipher below; this was a cipher used by the Imperial German Army during World War I, and it combined a simple substitution (a Polybius square) with rectangular transposition.
