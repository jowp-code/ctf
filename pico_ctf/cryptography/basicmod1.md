![image](https://github.com/jowp-code/ctf/assets/121969489/8ed52ab1-e4e2-4add-9a70-78fddcc88a9e)
<br>
<p>Based on the hints we need to find the remainder from module 37 based on the individual numbers in our message.txt.</p>
<br>
<p>In this instance mine are</p>

```shell
350 63 353 198 114 369 346 184 202 322 94 235 114 110 185 188 225 212 366 374 261 213
```
<br>
<p>350, can take 37 9 times, with a remainder of 17, with our alphabet beginning at 0 and ending at 25, that makes A = 0, and Z = 25, meaning 17 = R.</p>
<br>
<p>63 can take 37 ones time, with a remainder of 26, or this first digit in the decmial range, or 0.</p>
<br>
<p>Using the following code we can extract the rest of the answer.</p>

```python
import string
alphabet = string.ascii_uppercase + string.digits + "_"
code = "350 63 353 198 114 369 346 184 202 322 94 235 114 110 185 188 225 212 366 374 261 213"
words = code.split(" ")
result = ""
for word in words:
    result += alphabet[int(word) % 37]
print(result)
```
<br>
<p>My message.txt array spits out R0UND_N_R0UND_ADD_REDACTED</p>
<br>

```shell
Making the flag picoCTF{R0UND_N_R0UND_ADD_REDACTED}
```
