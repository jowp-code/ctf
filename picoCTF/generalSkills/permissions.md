![image](https://github.com/jowp-code/ctf/assets/121969489/69567aba-a690-4b97-a097-e4d94cc21247)
<br>
<p>I was hoping for some privilege escalation here or to abuse misconfigured permissions, unfortunately not. Dropping out of home and using 'ls' to check permissions I see two directories of interst, root and challenge.</p>
<br>
<p>We can't do much with root yet (if at all). So we'll check out /challenge.</p>
<br>

```shell
picoplayer@challenge:/$ ls -lah
drwxr-xr-x    1 root   root     21 Mar 16 02:29 challenge
drwx------    1 root   root     23 Mar 16 02:29 root
```

```shell
picoplayer@challenge:/$ cd challenge
picoplayer@challenge:/challenge$ ls
metadata.json
picoplayer@challenge:/challenge$ cat metadata.json
{"flag": "picoCTF{REDACTED}", "username": "picoplayer", "password": "8nVVw6hmD7"}
```
<br>
<p>The flag hints at the intended path, however the flag was right there and I was able to read it via 'cat'.</p>
<br>
