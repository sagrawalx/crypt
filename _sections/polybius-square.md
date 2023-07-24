The *Polybius square* is yet another simple substitution cipher, invented in Ancient Greece and made famous by the scholar Polybius (200--118 BCE). The same strategy was used [in Japan by Uesugi samurai clan](https://en.wikipedia.org/wiki/Japanese_cryptology_from_the_1500s_to_Meiji). During World War I, the German Imperial Army combined a Polybius square with rectangular transposition, resulting in the so-called [ADFGVX Cipher](https://en.wikipedia.org/wiki/ADFGVX_cipher). 

The Polybius square is still a simple substitution cipher in that it replaces single letters of plaintext with ciphertext and the substitution remains fixed over the entire message, but there is a a minor twist: each letter of plaintext is replaced by *two* letters of ciphertext. Nonetheless, we'll see later that a Polybius square can be broken in exactly the same way as the simple substitution ciphers described in the previous section. 

The key for a Polybius square is a table with labeled rows and columns, and the alphabet for the messages we're encrypting lives inside the table. For example, suppose the alphabet we're encrypting includes the capital letters `A` through `Z` and the digits `0` through `9`. That's 36 letters, which will fit perfectly inside a $6 \times 6$ grid. Following the Imperial German Army during World War I, let's label the rows and columns with the letters `ADFGVX` to get a table as follows. 

<table width="40%" style="margin: auto; font-family: monospace; text-align: center;">
<tr>
<th></th>
<th>A</th>
<th>D</th>
<th>F</th>
<th>G</th>
<th>V</th>
<th>X</th>
</tr>

<tr>
<th>A</th>
<td>N</td>
<td>A</td>
<td>1</td>
<td>C</td>
<td>3</td>
<td>H</td>
</tr>

<tr>
<th>D</th>
<td>8</td>
<td>T</td>
<td>B</td>
<td>2</td>
<td>O</td>
<td>M</td>
</tr>

<tr>
<th>F</th>
<td>E</td>
<td>5</td>
<td>W</td>
<td>R</td>
<td>P</td>
<td>D</td>
</tr>

<tr>
<th>G</th>
<td>4</td>
<td>F</td>
<td>6</td>
<td>G</td>
<td>7</td>
<td>I</td>
</tr>

<tr>
<th>V</th>
<td>9</td>
<td>J</td>
<td>0</td>
<td>K</td>
<td>L</td>
<td>Q</td>
</tr>

<tr>
<th>X</th>
<td>S</td>
<td>U</td>
<td>V</td>
<td>X</td>
<td>Y</td>
<td>Z</td>
</tr>
</table>

This table is the key. To encrypt a message, we convert each letter in our plaintext to a pair of letters indicating the row and column of that letter in the table above. For example, the letter `K` would be replaced by `VG` because `K` appears in row `V` and column `G`. Similarly, the letter `S` would be replaced by `XA`. Suppose that Alice wants to encrypt the message

```
Storm the gates at 14:37.
```

She first encodes by dropping punctuation and capitalizing: 

```
STORMTHEGATESAT1437
```

She then goes through and replaces each letter by the corresponding pair as we described above: 

```
XADDDVFGDXDDAXFAGGADDDFAXAADDDAFGAAVGV
```

This is the ciphertext. Bob, who knows the table, can then undo this process to decrypt the message. 

<div class="element">
<span class="label">Exercise</span>
Using the square given above: 

* Encrypt the message `High tide at 7:01am`. 
* Decrypt the message `XAAAADVGFAFVADDDAXADDDDGFVDX`. 
</div>
 
