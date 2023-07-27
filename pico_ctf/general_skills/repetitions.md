![image](https://github.com/jowp-code/ctf/assets/121969489/981fe3bb-e9de-4741-9534-1b740cbba155)
<br>
<p>Based on the title alone, I'm wondering how many time's I'm going to have to run base64 -d... keeping in mind we can pipe commands using '|' can make this process easier.</p>
<br>

```shell
└─$ cat enc_flag | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
picoCTF{REDACTED}
```
<br>
<p>6 times is the answer... not too bad.</p>
