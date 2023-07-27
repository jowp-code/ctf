![image](https://github.com/jowp-code/ctf/assets/121969489/b15e389d-9856-4f0d-89b1-8e0218564022)
<br>
<p>RSA can be intimidating, but in this instance the numbers in use are relatively small, <a href="https://www.youtube.com/watch?v=-ixz-2gi9r0&pp=ygUccGljb2N0ZiBtaW5kIHlvdXIgcHMgYW5kIHFzIA%3D%3D">this video</a> gives a great explanation, and provided the below code in python that I've used to beat many rsa challenges in CTFs.</p>
<br>
<p>To gather the information we need to fill out this code we need to factorize our N, we can do this using online tools such as alpertrons <a href="https://www.alpertron.com.ar/ECM.HTM">"Integer factorization calculator"</a>.</p>
<br>
<p>This will run for a short period of time until it finds two prime factors.</p>

```shell
Decrypt my super sick RSA:
c: 964354128913912393938480857590969826308054462950561875638492039363373779803642185
n: 1584586296183412107468474423529992275940096154074798537916936609523894209759157543
e: 65537
```
<br>
<p>Based on my 'n' I got: 2434792384523484381583634042478415057961 (40 digits) × 650809615742055581459820253356987396346063 (42 digits) </p>
<br>
<p>These two numbers be input at 'p' and 'q' respectively, we also need to add 'c' (our cipher code) and 'e' (common public key).</p>
<br>

```python
def egcd(a, b):
	if a == 0:
		return (b, 0 , 1)
	else:
		g, y, x = egcd(b % a, a)
		return (g, x - (b // a) * y, y)

def modinv(a, m):
	g, x, y, = egcd(a, m)
	if g != 1:
		raise Exception('modular inverse does not exist')
	else:
		return x % m

p=2434792384523484381583634042478415057961
q=650809615742055581459820253356987396346063
n=p*q
t=(p-1)*(q-1)

e=65537
d=modinv(e,t)
c=964354128913912393938480857590969826308054462950561875638492039363373779803642185
plain=pow(c,d,n)
print(bytearray.fromhex(hex(plain)[2:]).decode())
```
<br>
<p>Running the python code produces our flag.</p>
<br>

```shell
└─$ python3 solve.py
flag{REDACTED}
```
