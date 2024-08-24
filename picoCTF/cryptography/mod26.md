![image](https://github.com/jowp-code/ctf/assets/121969489/d74d69f5-34a4-44e8-b41f-9b0b0b4f67d7)
<br>
<p><a href="https://en.wikipedia.org/wiki/ROT13">ROT13</a> is a simple letter <a href="https://en.wikipedia.org/wiki/Substitution_cipher">substitution cipher</a> the English alphabet has 26 letters, so we are essentially splitting the alphabet in two and substituting the letters as shown below, 'A' becomes 'N', 'M' becomes 'Z' etc.</p>
<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/ef35b6c3-4f40-4b7a-8d14-b3b1c02f0a81)

<br>
<p>Solving this online is easy with the help of <a href="https://gchq.github.io/CyberChef/">cyberchef</a> but I like to use the CLI when possible, using an example from <a href="https://stackoverflow.com/questions/5442436/using-rot13-and-tr-command-for-having-an-encrypted-email-address">stack overflow</a> I swapped out the input for the listed code</p>


```shell
└─$ echo 'fooman@example.com' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
sbbzna@rknzcyr.pbz

└─$ echo 'cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_jdJBFOXJ}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'

quote> '
cvpbPGS{arkg_gvzr_Vyy_gel_2_ebhaqf_bs_ebg13_jdJBFOXJ} | tr A-Za-z N-ZA-Mn-za-m
```
<br>
<p>That was unexpected. It asked me for a quote, so somewhere in this command there is a single quotation mark, and of course it's in the ROT13 string! So let's ditch the single quotes and use double quotes.</p>
<br>

```shell
└─$ echo "cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_jdJBFOXJ}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
picoCTF{REDACTED}
```
<br>
<p>It's nice to use command line for things like this because you run into errors like this and it can be a teachable moment rather than just throwing it into a tool online.</p>
