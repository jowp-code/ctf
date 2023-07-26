![image](https://github.com/jowp-code/ctf/assets/121969489/3e6158f6-2de2-4e7b-85e2-f44d9d1f4fde)
<br>
<p>Another simple one, we can run strings against the binary and grep out the flag.</p>
<br>
<p>First lets grab the file.</p>
<br>

```shell
└─$ wget https://jupiter.challenges.picoctf.org/static/fae9ac5267cd6e44124e559b901df177/strings
```
<br>
<p></p>
<br>

```shell
└─$ strings strings | grep 'picoCTF'
picoCTF{REDACTED}
```
<br>
<p>Just like that we have our flag.</p>
