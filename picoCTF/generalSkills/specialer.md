![image](https://github.com/jowp-code/ctf/assets/121969489/3c3650a0-90c8-4a05-815d-dca4595337b9)
<br>
<p>Great, they got rid of the spellchecker! But what else do they have up their sleeve? Let's SSH in and find out.</p>
<br>

```shell
└─$ ssh -p 62045 ctf-player@saturn.picoctf.net
```
<br>
<p>Well, it seems like nothing works...</p>
<br>

```shell
Specialer$ clear
-bash: clear: command not found
Specialer$ ls
-bash: ls: command not found
Specialer$ id
-bash: id: command not found
Specialer$ sudo -l
-bash: sudo: command not found
Specialer$ cls
-bash: cls: command not found
Specialer$ CLEAR
-bash: CLEAR: command not found
Specialer$ Clear.
-bash: Clear.: command not found
Specialer$ 'clear'
-bash: clear: command not found
Specialer$ "clear"
-bash: clear: command not found
Specialer$ '/bin/ls'

```
<br>
<p>After poking around a little more I found a few commands that worked, 'echo' and 'cd'.</p>
<br>

```shell
Specialer$ echo

Specialer$ echo *.*
*.*
Specialer$ cd ../
Specialer$ echo *. *
*. ctf-player
Specialer$ echo *.* 
*.*
Specialer$ echo * .*
ctf-player . ..
Specialer$ cd ctf-player
Specialer$ echo * .*
abra ala sim . .. .hushlogin .profile
```
<br>
<p>We can use echo to read from files, so that replaces 'cat' and we can 'cd' into directories to see what's in them! After exploring I found the potential flag located in the 'ala' directory. I then had to figure out what sort of syntax 'echo' needed to pull it out of there. After some trial and error we got it.</p>
<br>

```shell
Specialer$ echo "$(~/home/ctf-player/ala/kazam.txt)"
-bash: /home/ctf-player/home/ctf-player/ala/kazam.txt: No such file or directory

Specialer$ echo "$(~/ctf-player/ala/kazam.txt)"
-bash: /home/ctf-player/ctf-player/ala/kazam.txt: No such file or directory

Specialer$ echo "$(</ctf-player/ala/kazam.txt)"
-bash: /ctf-player/ala/kazam.txt: No such file or directory

Specialer$ echo "$(<home/ctf-player/ala/kazam.txt)"
-bash: home/ctf-player/ala/kazam.txt: No such file or directory

Specialer$ echo "$(</home/ctf-player/ala/kazam.txt)"
return 0 picoCTF{REDACTED}
```

<p>Finally we have our flag, this one was a bit of a head scratcher.</p>
