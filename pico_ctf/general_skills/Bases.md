![image](https://github.com/jowp-code/ctf/assets/121969489/5c428ee7-1312-419a-bbfc-17fd893546e8)
<br>
<p>Off the bat I recognize that as a <a href="https://en.wikipedia.org/wiki/Base64">Base64</a> string, there are many popular tools for decoding strings like this. I really like <a href="https://gchq.github.io/CyberChef/">cyberchef</a></p>
<br>
<p>We can also use the command line to decode this string for us.</p>

```shell
└─$ echo bDNhcm5fdGgzX3IwcDM1 | base64 -d
REDACTED
```
<br>
<p>The flag is therefore picoCTF{REDACTED}.</p>
