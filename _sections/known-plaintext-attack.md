So far in this section, we've studied a few techniques for conducting ciphertext-only attacks on various ciphers, ie, ciphers where the only information Eve knows to begin with is which cipher was used to encrypt a message and has no information about the message itself. In this section, we'll describe a method for conducting a known-plaintext attack on simple substitution, ie, an attack where Eve has some partial information about the plaintext itself. Specifically, we'll consider the situation where Eve already knows that that a certain word appears at least once in the plaintext. As it turns out, we can use the $G$ test statistic to help us make good guesses here.

Suppose, for example, that Eve intercepts the following ciphertext, known to be encrypted using a simple substitution. 

<textarea readonly rows=10 style="width: 100%;">{{ twocities_encrypted }}</textarea>

It's the same ciphertext we studied in the "Frequency Analysis" section above, but this time, suppose further that Eve knows from the outset that the word `LONDON` occurs at least once in the plaintext. Let's take a look at how just knowing this can significantly speed up the codebreaking process.

Notice that, in the word `LONDON`, the first 4 letters are all distinct, the 5th letter is the same as the 2nd, and the 6th letter is the same as the 3rd. This pattern will be preserved by simple substitution, so somewhere in the ciphertext, we should find such a sequence. If we can find such a pattern, it will suggest how the letters `L`, `O`, `N`, and `D` should be matched! As it turns out, there are 18 sequences in this ciphertext that fit this pattern: 

```
SCUPCU
RACZAC
OFGJFG
ZUDRUD
ZUDRUD
SFZOFZ
ARZURZ
UDCZDC
RAFJAF
GKROKR
MUFJUF
SXQAXQ
SFZOFZ
UQZOQZ
SFZOFZ
RZUCZU
GCZWCZ
QZWCZW
```

There are some repeats in this list (`ZUDRUD` occurs twice and `SFZOFZ` occurs thrice), but even after eliminating repeats, there are still 15 distinct possibilities for sequences that might correspond to `LONDON`.

That's already pretty good: if we were just brute forcing through possible substitutions, the number of possibilities for matching the letters `L`, `O`, `N`, and `D` would be 
$$ 26 \cdot 25 \cdot 24 \cdot 23 = 358,800, $$
so we've already drastically decreased our search space (by a factor of $358,800/15 = 23,920$). But it turns out that we can do even better!

The key observation is that the relative frequencies of the letters `L`, `O`, `N` and `D` in some sample of plaintext English should match the relative frequencies of the corresponding ciphertext letters. We can measure deviations between frequencies using $G$, so we can compute $G$ for each of the 15 possible matchings and use that to help guide our choices. 

Let's describe how to do one of these calculations of $G$ in detail. Start by looking at some sample English text (eg, the Declaration of Independence) and throwing out all of the letters *except* `L`, `O`, `N`, and `D`. If we do so, we find the following distribution: 

<table style="margin: auto; width: 100%; text-align: center;">
<tr>
<th width="25%"></th>
<th width="15%">L</th>
<th width="15%">O</th>
<th width="15%">N</th>
<th width="15%">D</th>
<th width="15%">Total</th>
</tr>

<tr>
<th>Count in Sample</th>
<td>228</td>
<td>513</td>
<td>483</td>
<td>252</td>
<td>1476</td>
</tr>

<tr>
<th>Percent in Sample</th>
<td>15.4%</td>
<td>34.8%</td>
<td>32.7%</td>
<td>17.1%</td>
<td>100%</td>
</tr>
</table>

Suppose we think that the first possibility we listed above, `SCUPCU`, corresponds to `LONDON`. This would mean that `S` is matched to `L`, and `C` to `O`, and `U` to `N`, and `P` to `D`. We can then go through our ciphertext and count instances of `S`, `C`, `U` and `P` (throwing out all other letters), which lets us fill in the first row in the table below. 

<table style="margin: auto; width: 100%; text-align: center;">
<tr>
<th width="25%"></th>
<th width="15%">S</th>
<th width="15%">C</th>
<th width="15%">U</th>
<th width="15%">P</th>
<th width="15%">Total</th>
</tr>

<tr>
<th>Observed in Ciphertext</th>
<td>146</td>
<td>293</td>
<td>429</td>
<td>83</td>
<td>951</td>
</tr>

<tr>
<th>Expected in Ciphertext</th>
<td>146.9</td>
<td>330.5</td>
<td>311.2</td>
<td>162.4</td>
<td>951</td>
</tr>
</table>

To fill in the second row, we use the percentages we calculated in our table above. For example, we saw that 15.4% of the letters `L`, `O`, `N`, and `D` should be `L`, so if `L` corresponds to `S` in the ciphertext, we would expect $0.154 \cdot 951 \approx 146.9$ letters of the ciphertext to be `S`. Now we can compute $G$. 
$$ \begin{aligned} G &= 2 \sum O \ln \left(\frac{O}{E}\right) \\
&\approx 2 \left( 146 \cdot \ln \left( \frac{146}{146.9} \right) + 
293 \cdot \ln \left( \frac{293}{330.5} \right) + 
429 \cdot \ln \left( \frac{429}{311.2} \right) + 
83 \cdot \ln \left( \frac{83}{162.4} \right) \right) \\
&\approx 91.6. \end{aligned} $$

<div class="element">
<span class="label">Exercise</span>
The values of $G$ are approximated by a chi-square distribution with $k$ degrees of freedom. What is $k$? What is the value of the integral
$$ \int_{91.6}^\infty f_k(x)\, dx, $$
ie, the p-value? Based on this, does it seem likely that `SCUPCU` is a correct match for London?
</div>

We can then do analogous calculations of $G$ for the other 14 possibilities. Here are the results: 

<table width="50%" style="text-align: center; margin: auto;">
<tr>
<th width="66%">Ciphertext Match</th>
<th width="33%">$G$</th>
</tr>

<tr>
<td>`SCUPCU`</td>
<td>91.6</td>
</tr>

<tr>
<td>`RACZAC`</td>
<td>602.9</td>
</tr>

<tr>
<td>`OFGJFG`</td>
<td>34.5</td>
</tr>

<tr>
<td>`ZUDRUD`</td>
<td>442.5</td>
</tr>

<tr>
<td>`SFZOFZ`</td>
<td>7.8</td>
</tr>

<tr>
<td>`ARZURZ`</td>
<td>160.5</td>
</tr>

<tr>
<td>`UDCZDC`</td>
<td>354.4</td>
</tr>

<tr>
<td>`RAFJAF`</td>
<td>597.0</td>
</tr>

<tr>
<td>`GKROKR`</td>
<td>490.8</td>
</tr>

<tr>
<td>`MUFJUF`</td>
<td>39.3</td>
</tr>

<tr>
<td>`SXQAXQ`</td>
<td>284.8</td>
</tr>

<tr>
<td>`UQZOQZ`</td>
<td>223.0</td>
</tr>

<tr>
<td>`RZUCZU`</td>
<td>444.8</td>
</tr>

<tr>
<td>`GCZWCZ`</td>
<td>162.4</td>
</tr>

<tr>
<td>`QZWCZW`</td>
<td>533.3</td>
</tr>
</table>

Recall now that smaller vaues of $G$ mean that the distributions are closer together, so the most likely match for `LONDON` appears to be `SFZOFZ`. Of course, it's possible that `SFZOFZ` is not the correct match, but if we make that guess and then it seems like the decryption is turning out to be nonsense, we can just go back and try the one option with the next lowest value of $G$. 

As we've seen, it's usually easy to guess which letters correspond go `E` and `T`. We now have a systematic way of identifying the letters `L`, `O`, `N`, and `D` as well. We've made significant process! 

<div class="element" id="sagecell-known-plaintext-attack">
<span class="label">SageCell</span>
Here's a SageCell to play with. It's very similar to the [Frequency Analysis SageCell](#sagecell-frequency-analysis) we had earlier, but now there is one additional input: a textbox labeled "Known" which should contain plaintext words you know to be present in the encrypted message. Correspondingly, there is one additional output: a table labeled "Match" which shows all the patterns which match the known word, together with their corresponding $G$ values and the pairs that would need to be added to the guessed keys in order to make that pattern fit. 

Note that, as you make guesses for the key, the pattern matches could decrease. For example, if you guess that ciphertext letter `S` corresponds to plaintext letter `S`, only 3 of the 15 matches described above will remain --- namely, the 3 in which the first letter is `S`! Similarly, if you guess that the ciphertext letter `R` corresponds to plaintext letter `E`, then all pattern match containing the letter `R` will be eliminated since `E` does not occur in the word `LONDON`. 

Try decrypting. It's the same message as last time, but you should find that the knowledge of what corresponds to `LONDON` will speed up the process significantly. 

<div class="sage">
<script type="text/x-sage">
from re import sub
from copy import deepcopy
from collections import Counter

epsilon = 0.1
sampletext = """{{ declaration }}"""
ciphertext = """{{ twocities_encrypted }}"""

alphabet = list(build_alphabet(name="upper"))
unassigned = "*"
key_start = "\n".join(["".join(alphabet), "".join([unassigned] * 26)])

# Remove all non alphabetic characters and capitalize
def encode(text: str):
    stripped = sub(r"[^a-zA-Z]", "", text)
    return stripped.upper()

# Decrypt a string using the given key    
def decrypt(text: str, key: dict):
    text = encode(text)
    o = ""
    for x in text:
        if key[x] == "*":
            o += f'<span style="color: #dddddd;">{x}</span>'
        else:
            o += key[x]
    return o
    
# Compute the G test statistic: 
# * Both arguments are dicts with values being counts
# * Keys are used to match corresponding counts
# * Sum of the expected and observed counts need not match
def calculate_G(observed, expected):
    observed_total = sum(observed.values())
    expected_total = sum(expected.values())

    lnsum = 0
    for x, o in observed.items():
        e = expected.get(x, 0) * observed_total / expected_total
        if e == 0:
            e = epsilon
        lnsum += o * ln(1.0 * o / e)
    return 2 * lnsum
    
# Returns a frequency dictionary corresponding to a count dictionary
def counts_to_freqs(counts):
    total = sum(counts.values())
    return {k: (1.0*n/total) for k, n in counts.items()}
    
# Separates a string into groups of n letters for convenience
def separate(text: str, n=5):
    return " ".join(text[i:i+n] for i in range(0, len(text), n))

# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))
    
# Find all patterns in the text that match the given words pattern    
def find_candidates(text: str, word: str, init_key: dict):
    matches = []
    keys = []
    for i in range(len(text)-len(word)+1):
        segment = text[i:i+len(word)]        
        if segment not in matches:
            keep = True
            key = deepcopy(init_key)
            for x, y in zip(segment, word): 
                a = key[x] == unassigned
                b = y not in key.values()
                c = key[x] == y
                
                if a and b:
                    key[x] = y
                elif (a and (not b)) or ((not a) and (not c)):
                    keep = False
                    break
            if keep:
                matches.append(segment)
                keys.append(key)
    return matches, keys
    
# Validate the input key
def validate(input_key):
    if len(input_key) != len(key_start):
        raise Exception("Do not add or remove characters to the key")
    if input_key[:27] != key_start[:27]:
        raise Exception("Do not change the first row of the key")
    valid = alphabet + [unassigned]
    if len([x for x in input_key[27:] if x not in valid]):
        raise Exception(f"Every character in second row of key must be a capital letter or {unassigned}")
    counts = Counter([x for x in input_key[27:] if x != unassigned])
    for n in counts.values():
        if n > 1:
            raise Exception("Do not repeat letters in the second row")
    return {x: y for x, y in zip(alphabet, input_key[27:])}

@interact
def _(cipher=input_box(default=ciphertext, label="Input", height=5, width=70),
      known=input_box(default="LONDON", label="Known", height=2, width=70),
      sample=input_box(default=sampletext, label="Sample", height=5, width=70),
      input_key=input_box(default=key_start, label="Key", height=2, width=70)):
    if not cipher.isalpha() or not cipher.isupper():
        raise Exception("Ciphertext must consist only of uppercase characters")
    
    # Validate key
    key = validate(input_key)
    
    # Calculate frequencies
    alphabet = list(build_alphabet(name="upper"))
    sample = encode(sample)
    sample_freq = counts_to_freqs(Counter(sample))
    cipher_freq = counts_to_freqs(Counter(cipher))
    
    o = '<table width="90%" style="font-size: 0.6em; text-align: center;"><tr><td></td>'
    for x in alphabet:
        o += f"<td>{x}</td>"
    o += "</tr>"
    for i, f in enumerate([cipher_freq, sample_freq]):
        y = "Cipher" if i == 0 else "Sample"
        o += f"<tr><td>{y}</td>"
        for x in alphabet:
            o += f"<td>{100*f.get(x, 0):.1f}</td>"
        o += "</tr>"
    
    o += "</table>"
    output_div("Percent", o)
    
    # Calculate and filter ranked lists
    sample_sorted = sorted(alphabet, key=lambda x: sample_freq.get(x, 0), reverse=True)
    cipher_sorted = sorted(alphabet, key=lambda x: cipher_freq.get(x, 0), reverse=True)
    sample_ranked = "".join(x for x in sample_sorted if x not in key.values())
    cipher_ranked = "".join(x for x in cipher_sorted if key[x] == unassigned)
    
    s = "(Cipher) "
    s += separate(cipher_ranked)
    s +="<br/>"
    s += "(Sample) "
    s += separate(sample_ranked)
    output_div("Ranked", s)
    
    # Calculate Gs for known word patterns
    matches, keys = find_candidates(cipher, known, key)
    
    if len(matches) == 0:
        s = "<div>There are no pattern matches for the known word that are compatible with proposed key!</div>"
    else:
        s = "<table width='93%' style='text-align: center;'><tr><td width='30%'>Match</td><td width='30%'>G</td><td width='40%'>Key Additions<br/>(Ciphertext = Plaintext)</td></tr>"
        for m, k in zip(matches, keys):
            observed = Counter([x for x in cipher if k[x] != unassigned])
            observed = {k[x]: n for x, n in observed.items()}
            expected = Counter([x for x in sample if x in k.values()])
            g = calculate_G(observed, expected)
            s += "<tr>"
            s += f"<td>{m}</td>"
            if g == Infinity:
                s += "<td>+Inf</td>"
            else:
                s += f"<td>{g:.1f}</td>"
            s += "<td>"
            s += ", ".join([f"{x} = {y}" for x, y in k.items() if y != unassigned])
            s += "</td>"
            s += "</tr>"
    output_div("Match", s)
    
    # Display decrypted text
    output = decrypt(cipher, key)
    output_div("Output", f'<div style="inline-size: 45em; overflow-wrap: break-word;">{ output }</div>')  
</script>
</div>
</div>

{% capture brothersk %}JIFBPRDPKAYJAOPHARDQTPZPCPZTDSKFBTPOPRBIFYWXDPRWTIQTDQIFYWXSARHFMTAKBTAYWSDRTAKXDTPNAISPKPCPZTDSQPKDVPBZYJQTPZOAHTZXDDVMDBZDWTDBIOMYDZDYJPXPRWIRDWZTDBTAYWICTAKOPSSAPHDQAZTPWDYPWPANPRINRPRIZCSIOOPYABDRISXDBPFKDICTAKOPZSAOIRAPYHSADNPRBDKXFZKAOMYJXDBPFKDTDCISHIZTAOQTAYDTDQPKQDPSJARHDNDSJIRDQAZTTAKZDPSKPRWBIOMYPARZKPRWZFSRARHTAKTIFKDARZIPKAREICWDXPFBTDSJPCPAZTCFYKDSNPRZICZTDCPOAYJHSAHISJZIIEZTDZTSDDJDPSIYWOAZJPARZITAKBPSDACTDTPWRZYIIEDWPCZDSTAOZTDSDQIFYWTPNDXDDRRIIRDDNDRZIBTPRHDZTDXPXJKYAZZYDKTASZAZTPMMDRDWOISDINDSZTPZZTDBTAYWKSDYPZAIRKIRTAKOIZTDSKKAWDCISHIZTAOZIIPZCASKZTAKHSPRWCPZTDSQPKRIYIRHDSYANARHTAKQAWIQOAZJPKHSPRWOIZTDSTPWOINDWZIOIKBIQPRWQPKKDSAIFKYJAYYQTAYDTAKWPFHTZDSKQDSDOPSSADWKIZTPZOAZJPSDOPARDWCISPYOIKZPQTIYDJDPSARIYWHSAHISJKBTPSHDPRWYANDWQAZTTAOARZTDKDSNPRZKBIZZPHDXFZACTAKCPZTDSTPWSDODOXDSDWTAOTDBIFYWRIZARWDDWTPNDXDDRPYZIHDZTDSFRPQPSDICTAKDVAKZDRBDTDQIFYWTPNDKDRZTAOXPBEZIZTDBIZZPHDPKZTDBTAYWQIFYWIRYJTPNDXDDRARZTDQPJICTAKWDXPFBTDSADKXFZPBIFKARICOAZJPKOIZTDSMJIZSPYDVPRWSINAZBTOAKINTPMMDRDWZISDZFSRCSIOMPSAKTDYANDWCISOPRJJDPSKPCZDSQPSWKPXSIPWXFZQPKPZZTPZZAODLFAZDPJIFRHOPRPRWWAKZARHFAKTDWPOIRHZTDOAKINKPKPOPRICDRYAHTZDRDWAWDPKPRWICDFSIMDPRBFYZFSDQTITPWXDDRARZTDBPMAZPYKPRWPXSIPWZIQPSWKZTDDRWICTAKYACDTDXDBPODPYAXDSPYICZTDZJMDBIOOIRARZTDCISZADKPRWCACZADKARZTDBIFSKDICTAKBPSDDSTDTPWBIODARZIBIRZPBZQAZTOPRJICZTDOIKZYAXDSPYODRICTAKDMIBTXIZTARSFKKAPPRWPXSIPWTDTPWERIQRMSIFWTIRPRWXPEFRARMDSKIRPYYJPRWARTAKWDBYARARHJDPSKQPKNDSJCIRWICWDKBSAXARHZTDZTSDDWPJKICZTDMPSAKSDNIYFZAIRICCDXSFPSJTARZARHZTPZTDTAOKDYCTPWPYOIKZZPEDRMPSZARZTDCAHTZARHIRZTDXPSSABPWDKZTAKQPKIRDICZTDOIKZHSPZDCFYSDBIYYDBZAIRKICTAKJIFZTTDTPWPRARWDMDRWDRZMSIMDSZJICPXIFZPZTIFKPRWKIFYKZISDBEIRARZTDIYWKZJYDTAKKMYDRWAWDKZPZDYPJIRZTDIFZKEASZKICIFSYAZZYDZIQRPRWXISWDSDWIRZTDYPRWKICIFSCPOIFKOIRPKZDSJQAZTQTABTMJIZSPYDVPRWSINAZBTXDHPRPRDRWYDKKYPQKFAZPYOIKZPKKIIRPKTDBPODARZIZTDDKZPZDBIRBDSRARHZTDSAHTZKICCAKTARHARZTDSANDSISQIIWBFZZARHARZTDCISDKZAWIRZERIQDVPBZYJQTABTTDSDHPSWDWAZPKTAKWFZJPKPBAZAGDRPRWPOPRICBFYZFSDZIIMDRPRPZZPBEFMIRZTDBYDSABPYKTDPSARHPYYPXIFZPWDYPWPANPRINRPQTIOTDICBIFSKDSDODOXDSDWPRWARQTIOTDTPWPZIRDZAODXDDRARZDSDKZDWPRWYDPSRARHICZTDDVAKZDRBDICOAZJPTDARZDSNDRDWARKMAZDICPYYTAKJIFZTCFYARWAHRPZAIRPRWBIRZDOMZCISCJIWISMPNYINAZBTTDOPWDZTDYPZZDSKPBLFPARZPRBDCISZTDCASKZZAODPRWZIYWTAOWASDBZYJZTPZTDQAKTDWZIFRWDSZPEDZTDBTAYWKDWFBPZAIRTDFKDWYIRHPCZDSQPSWKZIZDYYPKPBTPSPBZDSAKZABZIFBTZTPZQTDRTDXDHPRZIKMDPEICOAZJPCJIWISMPNYINAZBTYIIEDWCISKIODZAODPKZTIFHTTDWAWRIZFRWDSKZPRWQTPZBTAYWTDQPKZPYEARHPXIFZPRWDNDRPKZTIFHTTDQPKKFSMSAKDWZITDPSZTPZTDTPWPYAZZYDKIRARZTDTIFKDZTDKZISJOPJTPNDXDDRDVPHHDSPZDWJDZAZOFKZTPNDXDDRKIODZTARHYAEDZTDZSFZTCJIWISMPNYINAZBTQPKPYYTAKYACDCIRWICPBZARHICKFWWDRYJMYPJARHPRFRDVMDBZDWMPSZKIODZAODKQAZTIFZPRJOIZANDCISWIARHKIPRWDNDRZITAKIQRWASDBZWAKPWNPRZPHDPKCISARKZPRBDARZTDMSDKDRZBPKDZTAKTPXAZTIQDNDSAKBTPSPBZDSAKZABICPNDSJHSDPZRFOXDSICMDIMYDKIODICZTDONDSJBYDNDSIRDKRIZYAEDCJIWISMPNYINAZBTMJIZSPYDVPRWSINAZBTBPSSADWZTDXFKARDKKZTSIFHTNAHISIFKYJPRWQPKPMMIARZDWQAZTCJIWISMPNYINAZBTUIARZHFPSWAPRICZTDBTAYWQTITPWPKOPYYMSIMDSZJPTIFKDPRWYPRWYDCZTAOXJTAKOIZTDSOAZJPWAWARCPBZMPKKARZIZTAKBIFKARKEDDMARHXFZPKZTDYPZZDSTPWRICPOAYJICTAKIQRPRWPCZDSKDBFSARHZTDSDNDRFDKICTAKDKZPZDKQPKARTPKZDZISDZFSRPZIRBDZIMPSAKTDYDCZZTDXIJARBTPSHDICIRDICTAKBIFKARKPYPWJYANARHAROIKBIQAZBPODZIMPKKZTPZKDZZYARHMDSOPRDRZYJARMPSAKTDZIICISHIZZTDBTAYWDKMDBAPYYJQTDRZTDSDNIYFZAIRICCDXSFPSJXSIEDIFZOPEARHPRAOMSDKKAIRIRTAKOARWZTPZTDSDODOXDSDWPYYZTDSDKZICTAKYACDZTDOIKBIQYPWJWADWPRWOAZJPMPKKDWARZIZTDBPSDICIRDICTDSOPSSADWWPFHTZDSKAXDYADNDTDBTPRHDWTAKTIODPCIFSZTZAODYPZDSIRAQIRZDRYPSHDFMIRZTPZRIQPKAKTPYYTPNDOFBTZIZDYYYPZDSICCJIWISMPNYINAZBTKCASKZXISRPRWOFKZBIRCARDOJKDYCRIQZIZTDOIKZDKKDRZAPYCPBZKPXIFZTAOQAZTIFZQTABTABIFYWRIZXDHAROJKZISJARZTDCASKZMYPBDZTAKOAZJPISSPZTDSWOAZSACJIWISINAZBTQPKZTDIRYJIRDICCJIWISMPNYINAZBTKZTSDDKIRKQTIHSDQFMARZTDXDYADCZTPZTDTPWMSIMDSZJPRWZTPZTDQIFYWXDARWDMDRWDRZIRBIOARHICPHDTDKMDRZPRASSDHFYPSXIJTIIWPRWJIFZTTDWAWRIZCARAKTTAKKZFWADKPZZTDHJORPKAFOTDHIZARZIPOAYAZPSJKBTIIYZTDRQDRZZIZTDBPFBPKFKQPKMSIOIZDWCIFHTZPWFDYPRWQPKWDHSPWDWZIZTDSPREKDPSRDWMSIOIZAIRPHPARYDWPQAYWYACDPRWKMDRZPHIIWWDPYICOIRDJTDWAWRIZXDHARZISDBDANDPRJARBIODCSIOCJIWISMPNYINAZBTFRZAYTDBPODICPHDPRWFRZAYZTDRHIZARZIWDXZTDKPQPRWERDQTAKCPZTDSCJIWISMPNYINAZBTCISZTDCASKZZAODIRBIOARHICPHDQTDRTDNAKAZDWIFSRDAHTXISTIIWIRMFSMIKDZIKDZZYDQAZTTAOPXIFZTAKMSIMDSZJTDKDDOKRIZZITPNDYAEDWTAKCPZTDSTDWAWRIZKZPJYIRHQAZTTAOPRWOPWDTPKZDZIHDZPQPJTPNARHIRYJKFBBDDWDWARIXZPARARHPKFOICOIRDJPRWDRZDSARHARZIPRPHSDDODRZCISCFZFSDMPJODRZKCSIOZTDDKZPZDICZTDSDNDRFDKPRWNPYFDICQTABTTDQPKFRPXYDPCPBZQISZTJICRIZDFMIRZTAKIBBPKAIRZIHDZPKZPZDODRZCSIOTAKCPZTDSCJIWISMPNYINAZBTSDOPSEDWCISZTDCASKZZAODZTDRZTAKZIIKTIFYWXDRIZDWZTPZOAZJPTPWPNPHFDPRWDVPHHDSPZDWAWDPICTAKMSIMDSZJCJIWISMPNYINAZBTQPKNDSJQDYYKPZAKCADWQAZTZTAKPKAZCDYYARQAZTTAKIQRWDKAHRKTDHPZTDSDWIRYJZTPZZTDJIFRHOPRQPKCSANIYIFKFRSFYJICNAIYDRZMPKKAIRKAOMPZADRZPRWWAKKAMPZDWPRWZTPZACTDBIFYWIRYJIXZPARSDPWJOIRDJTDQIFYWXDKPZAKCADWPYZTIFHTIRYJICBIFSKDCISPKTISZZAODKICJIWISMPNYINAZBTXDHPRZIZPEDPWNPRZPHDICZTAKCPBZKDRWARHTAOCSIOZAODZIZAODKOPYYWIYDKARKZPYYODRZKARZTDDRWQTDRCIFSJDPSKYPZDSOAZJPYIKARHMPZADRBDBPODPKDBIRWZAODZIIFSYAZZYDZIQRZIKDZZYDFMIRBDCISPYYQAZTTAKCPZTDSAZZFSRDWIFZZITAKPOPGDODRZZTPZTDTPWRIZTARHZTPZAZQPKWACCABFYZZIHDZPRPBBIFRZDNDRZTPZTDTPWSDBDANDWZTDQTIYDNPYFDICTAKMSIMDSZJARKFOKICOIRDJCSIOCJIWISMPNYINAZBTPRWQPKMDSTPMKDNDRARWDXZZITAOZTPZXJNPSAIFKPHSDDODRZKARZIQTABTTDTPWICTAKIQRWDKASDDRZDSDWPZNPSAIFKMSDNAIFKWPZDKTDTPWRISAHTZZIDVMDBZPRJZTARHOISDPRWKIIRPRWKIIRZTDJIFRHOPRQPKINDSQTDYODWKFKMDBZDWWDBDAZPRWBTDPZARHPRWQPKPYOIKZXDKAWDTAOKDYCPRWARWDDWZTAKBASBFOKZPRBDYDWZIZTDBPZPKZSIMTDZTDPBBIFRZICQTABTCISOKZTDKFXUDBZICOJCASKZARZSIWFBZISJKZISJISSPZTDSZTDDVZDSRPYKAWDICAZXFZXDCISDAMPKKZIZTPZKZISJAOFKZKPJPYAZZYDICCJIWISMPNYINAZBTKIZTDSZQIKIRKPRWICZTDASISAHAR{% endcapture %}
{% capture botchan %}GCWJKBCLHJMACXCDUSJXQXCWYOCBBMCBBUAJZCGCCMROJQUMTJOFJQBJOLBUMTTJICBUMWCIQWAUODALLDDKXUMTIQTXJIIJXBWALLODJQBUFJBLMWCOJUDKRHLXJGLKSJFCCYGQEKIRUMTHXLISACBCWLMDBSLXQLHSACBWALLOGKUODUMTBLICIJQJBYFAQUWLIIUSSCDBKWAJXJBAJWSSACXCFJBMLRJXSUWKOJXXCJBLMHLXDLUMTBKWAJSAUMTCNWCRSUAJRRCMCDSLGCOLLYUMTLKSUMSLSACQJXDHXLISACBCWLMDHOLLXLHSACMCFOQGKUOSBWALLOALKBCFACMLMCLHIQWOJBBIJSCBELYUMTBALKSCDJSICBJQQLKGUTGOKHHUOOGCSQLKWJMSEKIRDLFMHXLISACXCLQLKWAUWYCMACJXSAJAJBLUEKIRCDDLFMSACEJMUSLXLHSACBWALLOAJDSLWJXXQICALICLMAUBGJWYJMDFACMIQHJSACXBJFICACQCOOCDDCXUBUZCOQFAJSJHCOOLFQLKJXCSLTLJMDTCSQLKXGLMCBDUBOLWJSCDGQEKIRUMTLMOQHXLIJBCWLMDBSLXQUOOBCCUDLMSTCSDUBOLWJSCDMCNSSUICUJMBFCXCDLMCLHIQXCOJSUZCBLMWCRXCBCMSCDICFUSAJRCMYMUHCUFJBBALFUMTUSSLIQHXUCMDBXCHOCWSUMTUSBRXCSSQGOJDCBJTJUMBSSACXJQBLHSACBKMFACMLMCLHSACIWAUICDUMSAJSSACGOJDCBTOCJICDJOOXUTASGKSBCCICDXJSACXDKOOHLXWKSSUMTFUSAXJSACXDKOOBCCUHSACQDLMSWKSUXCSLXSCDWKSQLKXHUMTCXSACMACWAJOOCMTCDJMDFUSAHUMTCXMLSAUMTACXCTLCBUWKSIQSAKIGBOJMSFUBCHLXSKMJSCOQSACYMUHCFJBBIJOOJMDSACGLMCLHSACSAKIGAJXDCMLKTABLSACSAKIGUBBSUOOSACXCGKSSACBWJXFUOOGCSACXCKMSUOIQDCJSAJGLKSSFCMSQBSCRBSLSACCJBSCDTCLHLKXTJXDCMSACXCFJBJILDCXJSCBUVCDZCTCSJGOCQJXDXUBUMTSLFJXDSACBLKSAJMDUMSACWCMSXCLHFAUWABSLLDJWACBSMKSSXCCFAUWAFJBDCJXCXSLICSAJMOUHCUMSACBCJBLMFACMSACWACBSMKSBFCXCXURCUKBCDSLBOURLKSLHSACALKBCHXLISACGJWYDLLXCJXOQUMSACILXMUMTSLRUWYKRSACWACBSMKSBFAUWAAJDHJOOCMDKXUMTSACMUTASJMDCJSSACIJSSACBWALLOLMSACFCBSBUDCLHSACZCTCSJGOCQJXDFJBSACJDELUMUMTTJXDCMLHJRJFMBALRWJOOCDQJIJBAUXLQJSAUBBALRYCCRCXBBLMFJBJGLQJGLKSLXQCJXBLODMJICDYJMSJXLYJMSJXLFJBUSAJRRCMBJILOOQWLDDOCMCZCXSACOCBBACAJDSACSCICXUSQSLWLICLZCXSACHCMWCSLLKXQJXDJMDBSCJOIQWACBSMKSBLMCWCXSJUMCZCMUMTUAUDIQBCOHGCAUMDJHLODUMTTJSCLHSACHCMWCJMDWJKTASAUIUMSACJWSAJZUMTAUBXCSXCJSWKSLHHACTXJRROCDFUSAICUMDCBRCXJSULMACFJBJGLKSSFLQCJXBLODCXSAJMUJMDSALKTAFCJYYMCCDFJBRAQBUWJOOQSACBSXLMTCXFAUOCUFJOOLRRCDAUIACRKBACDAUBACJDJTJUMBSIQGXCJBSJMDGQWAJMWCUSBOURRCDUMBUDCIQBOCCZCJBSAUBAUMDCXCDSACHXCCJWSULMLHIQJXIUSXUCDSLBAJYCAUIOLLBCSALKTAAUBACJDDJMTOCDSACHKXSACXUMBUDCJMDGCUMTMLOLMTCXJGOCSLBSJMDSACBSUHOUMTWLIGJSACGUSIQGJXCJXIUSFJBRJUMHKOUACODAUIHJBSJTJUMBSSACHCMWCJMDGQJDCNSCXLKBHLLSSFUBSBCMSAUIDLFMHOJSLMAUBGJWYYJMSJXLGXLYCSACHCMWCJMDJBSACTXLKMDGCOLMTUMTSLQJIJBAUXLQJFJBJGLKSBUNHCCSOLFCXSAJMSACZCTCSJGOCQJXDACHCOOACJDOLMTSLAUBLFMSCXXUSLXQFUSAJSAKDJBACXLOOCDLHHACSLXCJFJQSACBOCCZCUMFAUWAAUBACJDAJDGCCMCMFXJRRCDJMDIQJXIXCWLZCXCDJBKDDCMHXCCDLILHILZCICMSSAJSMUTASFACMIQILSACXFCMSSLQJIJBAUXLQJSLJRLOLTUVCBACGXLKTASGJWYSAJSBOCCZCGCBUDCBSACJGLZCUDUDIJMQLSACXIUBWAUCHBFUSAYJMCYLLHJWJXRCMSCXBALRJMDYJYKLHJHUBAIJXYCSULMWCXKUMCDJWJXXLSRJSWALHLMCILBJYKSACBRXLKSBFCXCEKBSBALLSUMTLKSJMDSACRJSWAFJBWLZCXCDFUSABSXJFBSLCMBKXCSACUXCZCMACJOSAQTXLFSAKRLMSAUBBSXJFWLZCXCDRJSWAFCSAXCCFXCBSOCDHLXHKOOQAJOHJDJQJMDWLMBCPKCMSOQSALXLKTAOQBIJBACDJOOSACBRXLKSBJOBLULMWCHUOOCDKRJFCOOFAUWAFJSCXCDBLICXUWCHUCODBLFMCDGQLMCHKXKYJFJJMDACHLOOLFCDICFUSAYUWYBSACFCOOFJBBLDCZUBCDSAJSHXLIJOJXTCGJIGLLRLOCBKMYDCCRUMSLSACTXLKMDSACFJSCXUBBKCDJMDUXXUTJSCDSACXUWCHUCODBUTMLXJMSLHSACICWAJMUWJOBUDCLHSAUBUXXUTJSUMTICSALDJSSAJSSUICUBSKHHCDSACGJIGLLRLOCFUSABSLMCBJMDBSUWYBJMDBJSUBHUCDSAJSMLILXCFJSCXWJICKRUXCSKXMCDALICJMDFJBCJSUMTBKRRCXFACMHKXKYJFJHUCXQXCDFUSAJMTCXGKXBSUMSLLKXALKBCFUSAALFOUMTRXLSCBSBUGCOUCZCSACJHHJUXFJBBCSSOCDLMLKXRJQUMTHLXSACDJIJTCHJSACXDUDMLSOUYCICUMSACOCJBSJMDILSACXJOFJQBBUDCDFUSAIQGUTGXLSACXSAUBGXLSACXBHJWCFJBRJOUBAFAUSCJMDACAJDJHLMDMCBBHLXSJYUMTSACRJXSLHJMJWSXCBBJSSACSACJSXCSAUBHCOOLFFUOOMCZCXJILKMSSLIKWAHJSACXKBCDSLXCIJXYFACMACBJFICACBBLXCWYOCBBSAJSUFLXXQJGLKSAUBHKSKXCULHSCMACJXDILSACXBJQLHICCNJWSOQUAJZCMCZCXJILKMSCDSLIKWAUJIEKBSJBQLKBCCICMLFLMDCXIQHKSKXCKBCDSLWJKBCJMNUCSQSLIQILSACXUJIOUZUMTFUSALKSGCWLIUMTGKSJEJUOGUXD{% endcapture %}

<div class="element">
<span class="label">Exercise</span>
The following ciphertext was encrypted using a simple substitution. The plaintext is known to contain the word `CARPENTER`. Find the corresponding plaintext.
<textarea readonly rows=10 style="width: 100%;">{{ botchan }}</textarea>
</div>

<div class="element">
<span class="label">Exercise</span>
The following ciphertext was encrypted using a simple substitution. The plaintext is known to contain the word `MOSCOW`. Find the corresponding plaintext.
<textarea readonly rows=10 style="width: 100%;">{{ brothersk }}</textarea>
</div>

<div class="element">
<span class="label">Harder Sage Exercise with No Programming Involved</span>
When you did the above codebreaking exercises, you may have noticed a changing $G$ value associated to a given pattern match as you made guesses for the decryption key. Look through the code and explain why this is happening. Does this behavior make sense to you?
</div>




