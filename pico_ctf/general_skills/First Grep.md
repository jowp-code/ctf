![image](https://github.com/jowp-code/ctf/assets/121969489/2985096c-03e4-4c6a-b8e5-8af9983b421b)
<br>
<p>Easy challenge here, we're going to grab the file, run cat on it and pipe the grep command with it searching for the string 'pico'.</p>

```shell
└─$ wget https://jupiter.challenges.picoctf.org/static/515f19f3612bfd97cd3f0c0ba32bd864/file
```
<br>
<p>Now we can run the command to read the file and report back if finds our partial string.</p>

```shell
└─$ cat file | grep 'pico'
picoCTF{REDACTED}
```
<br>
<p>Our flag is picoCTF{REDACTED}.</p>
<br>
<p>Another alternative is pipe grep alongside strings</p>

```shell
└─$ strings file | grep 'pico'
picoCTF{REDACTED}
```
