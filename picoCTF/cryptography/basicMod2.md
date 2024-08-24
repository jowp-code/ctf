![image](https://github.com/jowp-code/ctf/assets/121969489/187268cc-a906-4cd0-be86-3e2749096704)
<br>
<p>This time we're working with modular inverse, instead of modulo.</p>
<br>

```python
import string
characters = string.ascii_uppercase
characters += "0123456789_"

flag_enc = [104, 372, 110, 436, 262, 173, 354, 393, 351, 297, 241, 86, 262, 359, 256, 441, 124, 154, 165, 165, 219, 288, 42]
flag = ""
for char in flag_enc:
    mod = pow(char, -1, 41)
    flag += characters[mod - 1]
print(flag)
```
<br>
<p>Outputs my flag as picoCTFpicoCTF{1NV3R53LY_REDACTED}</p>
<br>
