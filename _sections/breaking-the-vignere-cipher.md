With the index of coincidence in hand, we're now ready to break the Vignère cipher. Recall that this cipher was considered unbreakable for about 300 years...! The method we will describe is not [the first method that was discovered](https://en.wikipedia.org/wiki/Kasiski_examination), but it makes good use of the mathematical machinery we've developed. 

The idea is as follows. Suppose we start with following ciphertext which we know to be encrypted using the Vignère cipher.

<textarea readonly rows=10 style="width: 100%;">{{ wuthering_vignere }}</textarea>

We make a guess for the "period," ie, the length of the key word. Suppose we guess a period of 5. We then break the ciphertext into rows of that length. 

<div style="font-family: monospace; overflow-wrap: break-word; width: 3.5em;">
IKGWE
RTZON
XEULT
EZWHW
XTYHS
ZKEG…
</div>

If the period we've guessed is correct, each column of this rectangle was encrypted using a Caesar cipher with the same shift. This means that we should be able to detect that our guess for the period is correct by examining indices of coincidence! If every column has an index of coincidence in the range that's expected of English text, our guess for the period is probably correct. Otherwise, we should try a different period. 

In our example, if we guess a period of 5, it turns out that the indices of coincidence of the five columns are 1.13, 1.11, 1.18, 1.15, and 1.15, respectively. This is well outside the range that's expected of English text, so our guess 5 is probably wrong. So then we try a period of 6, and we find indices of coincidence of 1.75, 1.77, 1.73, 1.72, 1.79, and 1.74 --- all in the right range! This means there's an excellent chance that the period is 6. 

Once we have a guess for the period, we're in the clear! We then just have to figure out what the shift is for each column, and we can do this using basic frequency analysis: the most frequent letter in each column should correspond to `E`, so we should guess that the shift for that column is the shift that moves `E` to the most frequent letter. (If that doesn't work we might try shifting so that the second most frequent letter in that column moves to `E` instead.)

<div class="element" id="sagecell-breaking-vignere">
<span class="label">SageCell</span>
Here is a tool to help you break a Vignère cipher using these ideas. Hit "Run." You'll see the following: 

* An "Input" box for ciphertext. It defaults to the ciphertext at the beginning of this section. 

* A box labeled "Period" for you to guess a period. It defaults to 5, but we've already seen above that 5 is not the right guess, so you'll want to change this. 

* A box labeled "Shifts" to guess what the shifts were used to encrypt each column. It must be a list (enclosed in square brackets) of the same length as the period you guessed. If you're changing the period and aren't ready to guess shifts yet, just make this a list of 0s of the right length!

Below these input fields, the code outputs several things. 

* First you'll see a table listing the indices of coincidence for each column. Remember that your guess for period is probably correct when the indices are all approximately 1.75. 

* Next, you'll see a large table labeled "Ranks." This table ranks the letters that appear in each column by frequency, from most frequent to least frequent. Below each letter is the Caesar shift that would encrypt `E` into that letter. You'll want to use this table to construct your guess for the shifts. Most likely, the shift for column $i$ will be the number that appears below the most frequent letter in that column. 

* Finally, you'll see the result of decrypting using your proposed guesses! 

Decrypt the ciphertext! Hint: It's one of the sample passages we've seen above. 

<div class="sage">
<script type="text/x-sage">
from re import sub
from collections import Counter
from itertools import cycle

ciphertext = """{{ wuthering_vignere }}"""

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

# Decrypt a string using the given key sequence  
def decrypt(text: str, shifts: list):
    nums = numerify(text.upper())
    transformed = [(x - shift) % 26 for x, shift in zip(nums, cycle(shifts))]
    return denumerify(transformed)

# Calculate index of coincidence of given text
# Use dictionary of counts if supplied    
def coincidence(text: str, c=None):
    n = len(text)
    if c is None:
        c = Counter(text)
    return 26.0 * sum([(m*(m-1))/(n*(n-1)) for x, m in c.items()])
    
def shift_to_E(letter):
    return (ord(letter) - ord("E")) % 26

# Prints an output div aligning with the interact controls   
def output_div(label: str, content: str):
    s = '<div class="sagecell_interactControlCell" style="width: 100%;">'
    s += f'<label class="sagecell_interactControlLabel">{label}</label>'
    s += f'<div class="sagecell_interactControl">{content}</div>'
    s += '</div>'
    pretty_print(html(s))

@interact
def _(text=input_box(default=ciphertext, label="Input", height=5, width=70),
      period=input_box(default="5", label="Period", width=62),
      shifts=input_box(default="[0,0,0,0,0]", label="Shifts", width=62)):
    if not text.isalpha() or not text.isupper():
        raise Exception("Ciphertext must consist only of uppercase characters")
    if int(period) != period or period < 1:
        raise Exception("Period must be a positive integer")
    
    alphabet = list(build_alphabet(name="upper"))
    cols = ["".join([text[x] for x in range(i, len(text), period)]) for i in range(period)]
    counts = [Counter(c) for c in cols]
    indices = [coincidence(s, c) for s, c in zip(cols, counts)]
    ranked = [sorted(alphabet, key=lambda x: c.get(x, 0), reverse=True) for c in counts]
    
    o = "<table style='text-align: center;'><tr><td>Col</td><td>IC</td></tr>"
    for i in range(period):
        o += f"<tr><td>{i+1}</td>"
        o += f"<td>{indices[i]:.2f}</td>"
    o += "</table>"
    output_div("ICs", o)
    
    o = "<table style='text-align: center; font-size: 0.7em;'>"
    for i in range(period):
        o += f"<tr><td rowspan=2 style='padding-right: 0.5em;'>Col {i+1}</td>"
        o += "<td style='padding-right: 0.5em;'>Letter</td>"
        for j in range(26):
            o += f"<td style='padding-right: 0.5em;'>{ranked[i][j]}</td>"
        o += "</tr><tr><td style='padding-right: 0.5em;'>Shift</td>"
        for j in range(26):
            o += f"<td style='padding-right: 0.5em;'>{shift_to_E(ranked[i][j])}</td>"
        o += "</tr>"
    o += "</table>"
    output_div("Ranks", o)
    
    if len(shifts) != period:
        output = "Cannot decrypt; number of shifts must match period"
    else:
        output = decrypt(text, shifts)
    
    output_div("Output", f'<div style="inline-size: 45em; overflow-wrap: break-word;">{ output }</div>')
</script>
</div>
</div>

{% capture donjuan_vignere %}KEAFVQGKSQLSCBPRTCQWFRSVRIYQEEJJRKVPGMEAJRXUMPAUNOFVCTKURBXEJVLVVRGBMEZLIRPEJRTVMHKHRISLJCQWEFUIZERJFSREEPJYDRNDVIKNYURMNSZVCMWJASGEZIUJZCUEIDRLFVDCSMETCNPRVDLVJMTKHRQSNVPQWAAPFCVKCESRWIEGJGRSHSICFTKEGJPQVEAJRTGFHXRGNPAEPYEUYVRBYFPZXFVVDOZTVIDWDEQDEEUJGJSHSVFLLFVDOZEUDGTRBYFAFIIUFFNSXNYMUVWUJXVDYTSLRTXFFBQLTNHEZEQVKHRXECCQQWRREWKLAEFAAEGFERTRSGFHNZRJKHRSMTYRWIKRZGRINGKSPMEUZLURTVOKCZRVVRVOKNZRJXOYEEEUJCUEAXMKYEGDSJIMTYQRRRXMIUFLNPLRTWSIGNCINOXCPRJRNGIIZICAVSGIIPRJNKOYESWGYUJIBOWZERGESRCYKFDXRRVPYJJRACEFMMBVRJVIECIRLRAKHRZHZWDGIEQOIZKFGIIAULVZPYFRQTRFIRJVIEJHVRQDLTNOIOGPGJSVPRRCMQBAZPXZFLQIAAFQGYYUZSFFVMVBCJAPPQDVLVRRLVRIVQVIAVOIUCGEVNGJSLJKGCAADLFCWQIBNOXVIGPXTBULVZPYFRQTSEVQGVMREXFJYADYOFELKWJRSCPAVIRQIEXJRUCCVYESSSQVLJVAEUSWREGRNBULVIGNFVRUSIVNQJEBOWFWRELSUJSEJYPUTUJRBNGVYRNQXLICQWMLBHFICTJAGIMIUYPFVVDIRKRJVSRGXVJUCJIADPZECFKOOMYJYYVKHRCSKKMOFFZZLVRPVZFRFPTFKRLNPUMFEQJVSRFQVURQJALJEDRACKHBMMTRLFZFRBVYVJNSUGJPFMCAFUFPEYJMFVAEMCKYYVZWBVPUJYEIISJGVVRGINVUCKFWQLTUFJFLPVYEZQXPZLIRCHQSWTFKRNJJRVTPKVDUVVIRFHFRCMIRJSTVIOFKZEYPVWRYMJKCPTEJJXYVYEYDNXRWFPIVTSVPFWRJVPNTXJKGNCIAUSOZACKEQXMKYRJVVVPPVEAGFFLFWKVPFRYFQPVRQWIEFJIDSPCTENOINCGHVOSIEGGGPVSFBPZWCHZLYFHNZRJCOIFXYVUQDAATMKKGPXNRYXKFZGCVVEVFCMQBEQBXYZKYZTUGPRJFKEGRZIJJFGNAFTMCVLVZSUPYCUFCMEAPRVVBVFCNMPFEYDIAIPXFBGNCMLMSMVPKWHRBFREBQEEQNIKYCPJHRIEUCYWXHREFLKYEFMSJXUZQJFFZBVMVJQLSJPVBDYPJHVQARJQJRTGFVVUZGKWRFRYVPPVRIPYJWGPXEETAYVLCIELPYKFZGXRNOHULIGRSXFHKYCUZXGISWKFGGRVOGVNGVYAAFBGICUJIBOSWDSTUEEPYJXJGVOAIIICGRJAAEECFMMFFOBGTYYPRLVBRWICPQYVOLVICAVSNOHNYCPZSLPYIWYVYEEHSZEEVFDVFWRZBVYEFFZVERJCAHHLZEECEDGIVFNGPXHRSFFLOWVTGPHFEHWRNJJXYDYFUEAJRXTMSLEGSCJYCYRSNOMEEMEVNGZSLEEIZRYXLFNYURCPVWKFKGUTBQPRPUKKHFBGIVBVYIAHWFYBQETFQIRBMHZTPSMVURJVYBVRXRLFYAAEWFDCFFNWVEEKFGIEVTSECWQEEVNQFIRCCFNULVIGPKHRXSICBCEDHOJFIRWEAGFPPYCKJMVOIKYCUVVROAFDCPFFSFVIRPCKHRGVZVLFJOSESEASCEAAEXYVNTZNPFLZDQGCFTBZVRLGOCYBQRKGQEOSISIIMTKWBIYEUPGUYRBVJCYVVRHOHVIJQLIFYZNVJNSRRETVIQQESJPYCUFCMEYBYXYCFRTGIMJJYNCYOVXGVPJRPFBXKYCDVGVORZEEQWAAPVXPRJVMVOHYRBUKIYMEELLWJUNMHVXPGVOSMYTZBKKYQFWGZRGKHRIIRKMHKHRDEEUJGJTUFMEKCPJIGZSWKFGVMBUMFEQVYETPPURLFJIYWIIMYUVSGIIWLKGJOSXMEVBGJPVUIKYCXZSVPRFWPCMIFIMEXUQDEAQIIYYRJTUFVVJRKCLYVVBVBKETUFHVGRJJOSULVYCCITNMMKKJGFFGIEKICUGEPUJFIRJZNTTLLDYPRNQEMMZLGNHVDLJKPWXGYFWLERKCTUFVVMCNYAFEVFNLGUIGJRWCMQUSBGWGRPMCIAHAZECPVVRSXYVJGJSGIIWCMYVRFXIIVYNIENECTISUYEQULVVWGJWRSIJKCGGEQXMKYBTZNXBRUZLVFXVDEKZMPKODVSKVPCSEYBMJYYFIENDLVUCXVNGPXYVQCEDNMWZERJVPNVWVKFCKFBMPFNCFRDBPVFGCPVDNOHRJYVKHRGIRJRQWBNMXYRXCIGBEQREGHVSGFHYZKUVLSIIJVCOVDGPGFDKCEDEFGFXLKKIBORFNGPKHRQIIJMPFFNOSCUUJZTRIEZICFJEEWEEKUKKHHOWKVYFPGNJXREBFIAJOFIFUUYEROXVICFNIGIKCFMOPMVFRREBJZSYPSBJCGDEQUSSCGIYTGIIXRPNRNQTXYVPWSYPVTJKFGGYEBQZUQQWFEVMKJRJVBEJKYKLGJSBGXYVDGRSGULVXJQNOSULVRQVFNVTLVUDCTEFBRUKFGTOYPVJFDVYEPVWYZMPJDROXVUZAKHRXLZKCCIMFPJKYCYFMROXYVLJVCNTXRGYNCOIFVKYGUWOYMCSPQCPIAHMERFQCLBXZFZAGKHRTSCVKPNOEEWJZPAFUEGEKYCTZSQZMEXBQEJHBRIFQGDAXJRXREGJTHSIKFFKJGHFWKJUJZCUNMXYRDVTEBRJCYVVDRYGLJCOVTUJWUFCUEOGIEGGCPVVRSCURW{% endcapture %}

<div class="element">
<span class="label">Exercise</span>
The following ciphertext was encrypted using a Vignère cipher. Find the corresponding plaintext. Also state what the key word used for encryption was. 

<textarea readonly rows=10 style="width: 100%;">{{ donjuan_vignere }}</textarea>
</div>


