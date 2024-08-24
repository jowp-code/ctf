![image](https://github.com/jowp-code/ctf/assets/121969489/c39ecfea-6c46-4bc7-89cc-6b3c5d5cb1bc)

<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/56ffcf23-63f5-4546-9c05-f135957eb51e)

<br>
<p>Looking at the file, we are can see the numbers, knowing the format of the flag helps us here. picoCTF{flag}, 16 9 3 15 3 20 6. P is the 16th letter of the alphabet, 9th is I, 3rd is C, 15th is O. So we are working with a simple number letter substitution cipher.</p>
<br>
<p>We can solve this with online tools such as <a href="https://www.dcode.fr/letter-number-cipher">dcode</a>, but what about a script we could modify later on for another type of substitution cypher?</p>
<br>

```python
alpha = "abcdefghijklmnopqrstuvwxyz"
length = len(alpha)
the_numbers = [16, 9, 3, 15, 3, 20, 6, "{", 20, 8, 5, 14, 21, 13, 2, 5, 18, 19, 13, 1, 19, 15, 14, "}"]
for i in the_numbers:
    if(not isinstance(i, int)):
        print(i, end = '')
    else:
        print(alpha[i-1], end = '')
print() 
```
<br>
<p>This outputs our flag picoCTF{REDACTED}</p>
<br>

```shell
└─$ python3 thenumbers.py
picoctf{REDACTED}
```
<br>

