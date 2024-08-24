![image](https://github.com/jowp-code/ctf/assets/121969489/f9ad9d1b-e3ee-4f79-aa52-eba30c7045ee)

<details>
  <summary>Hint 1</summary>

  Finding a cheatsheet for bash would be really helpful!

</details>
<br>
<p>So we have to connect to a remote system using ssh, but first we have to initialize our instance via Launch Instance button on the right side of the pop up. Then using out terminal can initiate a secure shell connection.</p>
<br>

```shell
└─$ ssh ctf-player@venus.picoctf.net -p 51031
```
<br>
<p>Carnegie Mellon University is a well respected University, so we don't have to worry about bad actors here, so we can accept the fingerprint, and enter the password provided to gain access to the machine. On login we are instructed to type 'ls' to begin (ls is used to list files and folders and has a few flags we can modify for hidden files etc).</p>
<br>

```shell
ctf-player@pico-chall$ ls
1of3.flag.txt  instructions-to-2of3.txt
```
<br>
<p>We can use cat to grab the contents of '1of3.flag.txt' and check the contents of instructions-to-2of3.txt</p>
<br>

```shell
ctf-player@pico-chall$ catt 1of3.flag.txt
picoCTF{REDACTED_
```
<br>

```shell
ctf-player@pico-chall$ cat instructions-to-2of3.txt 
Next, go to the root of all things, more succinctly `/`
```
<br>
<p>The root of all things, would be the main directory for example the C:\ on a Windows computer, once we get to that directory we can us 'ls' again to see the contents of the folder.</p>
<br>

```shell
ctf-player@pico-chall$ cd /
ctf-player@pico-chall$ ls
2of3.flag.txt  bin  boot  dev  etc  home  instructions-to-3of3.txt  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

<br>
<p>Another piece of the flag, and instructions to part 3 of 3</p>
<br>

```shell
ctf-player@pico-chall$ cat 2of3.flag.txt 
REDACTED
```
<br>

```shell
ctf-player@pico-chall$ cat instructions-to-3of3.txt 
Lastly, ctf-player, go home... more succinctly `~`
```
<br>
<p>Last but not least we are heading to our home directory for the last part of the flag.</p>
<br>

```shell
ctf-player@pico-chall$ cd ~
ctf-player@pico-chall$ ls
3of3.flag.txt  drop-in
```
<br>

```shell
ctf-player@pico-chall$ cat 3of3.flag.txt
REDACTED}
```
<br>
The final part of our flag.
