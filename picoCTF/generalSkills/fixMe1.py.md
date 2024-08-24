![image](https://github.com/jowp-code/ctf/assets/121969489/7817be46-f5b5-41aa-9c49-415170c275e3)
<br>
<p>Let's grab the file and see what we are working with.</p>
<br>

```shell
└─$ wget https://artifacts.picoctf.net/c/27/fixme1.py
```
<br>
<p>Let's run it and see what the debugger has to say.</p>
<br>

```shell
└─$ python3 fixme1.py   
  File "/home/kali/pico/fixme1.py", line 20
    print('That is correct! Here\'s your flag: ' + flag)
IndentationError: unexpected indent
```
<br>
<p>It appears we have an indentation error on line 20, this is a very common complaint from python.</p>
<br>

```python
flag = str_xor(flag_enc, 'enkidu')
  print('That is correct! Here\'s your flag: ' + flag)
```
<br>
<p>Looks like the print command is indented two spaces, instead of being aligned to the left. Removing those two spaces should fix the script.</p>
<br>

```python
flag = str_xor(flag_enc, 'enkidu')
print('That is correct! Here\'s your flag: ' + flag)
```
<br>
<p>Running the code should now produce the flag.</p>
<br>

```shell
└─$ python3 fixme1.py
That is correct! Here's your flag: picoCTF{REDACTED}
```
<br>
<p>Our flag is picoCTF{REDACTED}</p>
<br>
