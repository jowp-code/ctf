![image](https://github.com/jowp-code/ctf/assets/121969489/61ca07b5-0f9d-4dca-9f1c-f5f50af841a6)
<br>
<p>As usual we need to download the file and check it out.</p>
<br>

```shell
└─$ wget https://artifacts.picoctf.net/c/4/fixme2.py 
```
<br>
<p>Let's run it and see what's broken.</p>
<br>

```shell
└─$ python3 fixme2.py
  File "/home/kali/pico/fixme2.py", line 22
    if flag = "":
       ^^^^^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```
<br>
<p>Syntax error on line 22, and it's suggesting some fixes, let's check it out.</p>
<br>

```python
# Check that flag is not empty
if flag = "":
  print('String XOR encountered a problem, quitting.')
```
<br>
<p>We can see from the code that there is one equal sign here, we are saying that if flag is nothing instead of if flag is equal to nothing. python is looking for two == to validate this line we need to add another.</p>
<br>

```python
# Check that flag is not empty
if flag == "":
  print('String XOR encountered a problem, quitting.')
```
<br>
<p>Now let's try to run it again and see if that was the problem</p>
<br>

```shell
└─$ python3 fixme2.py
That is correct! Here's your flag: picoCTF{REDACTED}
```
<br>
<p>Our flag is picoCTF{REDACTED}</p>
<br>
