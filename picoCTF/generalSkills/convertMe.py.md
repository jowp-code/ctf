![image](https://github.com/jowp-code/ctf/assets/121969489/1aa82251-3720-415b-a259-236a7a5fd0ad)
<br>
<p></p>
<br>

```shell
└─$ wget https://artifacts.picoctf.net/c/22/convertme.py
```
<br>
<p>The code indicates it is going to provide a random integer, we are then tasked with converting that decimal to binary and feeding it back into the command line. In a prior room we used code to convert decimal to binary, we can use that again here.</p>
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
<p>Running convertme.py gives me the number 34</p>
<br>

```shell
└─$ python3 dec_to_bin.py 
Enter Decimal: 34
100010    
```
<br>
```shell
└─$ python3 convertme.py
If 34 is in decimal base, what is it in binary base?
Answer: 100010
That is correct! Here's your flag: picoCTF{REDACTED}
```
<br>
<p>Our flag is picoCTF{REDACTED}</p>
