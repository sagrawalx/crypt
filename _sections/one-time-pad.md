The *one-time pad* is a special case of the Vignère cipher where the key sequence is: 

* never re-used,
* at least as long as the plaintext,
* "unrelated to the plaintext," and
* "totally random," in the sense that each number 0 through 25 is equally likely in each position of the key.

If you understand how the Vignère cipher works, you also understand the one-time pad works. The only noteworthy difference is that the key sequence must not be generated using a key words, because words won't have the property that each letter is equally likely!

We'll return to the one-time pad again. We'll want to make precise what we mean by "unrelated to the plaintext" and "totally random" above, and we'll see that it has a property called *perfect secrecy*, which means that the security of the one-time pad can be mathematically guaranteed!

<div class="element">
<span class="label">Exercise</span>
While "perfect secrecy" sounds great, the one-time pad is generally not very practical. Why not? *Hint*. If Alice and Bob have a way of sharing a secret key that's at least as long as the plaintext that they want to share...
</div> 
