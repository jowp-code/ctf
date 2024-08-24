![image](https://github.com/jowp-code/ctf/assets/121969489/097c01c2-ecb4-4a27-973d-5e55cac4ecfd)

<br>
<p>As with the Let's Warm Up challenge, there are a number of calculators to solve this <a href="https://www.rapidtables.com/convert/number/hex-to-decimal.html">online</a>, but if we don't have access to them for some reason why not script it?</p>
<br>

```python
my_string = input('Enter Hex Code: ')
dec = int(my_string, 16)
print(str(dec))
```
<br>
<p>It's simple code and accepts a hexadecimal value upon run, if we enter in our value of 0x3d (remember to strip the '0x') we get the following results.</p>
<br>

```shell
└─$ python3 hex_to_dec.py
Enter Hex Code: 3D
61
```
<br>
<p>Our flag is therefore picoCTF{61}.</p>
