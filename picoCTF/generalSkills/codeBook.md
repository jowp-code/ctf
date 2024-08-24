![image](https://github.com/jowp-code/ctf/assets/121969489/390349b4-1f8a-4cab-bba0-afcd65515b77)
<br>
<p>Let's get started by grabbing both files.</p>
<br>

```shell
└─$ wget https://artifacts.picoctf.net/c/3/code.py | wget https://artifacts.picoctf.net/c/3/codebook.txt
```
<br>
<p>Verify that the downloads are complete.</p>
<br>

```shell
└─$ ls
codebook.txt  code.py
```
<br>
<p>Looking over the codebook it appears to be a xor encrypted string. Checking out the python script, it's going to decode the string and reorder it. We are tasked with running code.py in the same directory as codebook.txt, so lets do that.</p>

```shell
└─$ python3 code.py      
picoCTF{REDACTED}
```
<br>
<p>Our flag is picoCTF{REDACTED}</p>

