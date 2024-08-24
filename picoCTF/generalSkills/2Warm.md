![image](https://github.com/jowp-code/ctf/assets/121969489/5eb15b4b-92cf-44de-8298-908e5449b634)
<br>
<p>A number of calculators can be found <a href="https://www.rapidtables.com/convert/number/hex-to-decimal.html">online</a> but we can code it in python quickly.</p>
<br>

```python
def dec2bin(num):
    if num > 1:
        dec2bin(num // 2)
    print(num % 2, end='')
bin = int(input("Enter Decimal: "))
dec2bin(bin)
```
<br>
<p>The above code is modified from <a href="https://beginnersbook.com/2018/02/python-program-to-convert-decimal-to-binary/">BeginnersBook</a>.</p>
<br>

```shell
└─$ python3 dec_to_bin.py 
Enter any decimal number: 42
101010
```
<br>
Our flag is picoCTF{101010}.
