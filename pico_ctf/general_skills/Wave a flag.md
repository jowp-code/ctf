![image](https://github.com/jowp-code/ctf/assets/121969489/d68078ea-b865-486d-902d-274be3d334e6)
<details>
  <summary>Hint 1</summary>
  
 This program will only work in the webshell or another Linux computer.

```shell
└─$ Enter Command Here
```
</details>
<details>
  <summary>Hint 2</summary>
  
To get the file accessible in your shell, enter the following in the Terminal prompt:

```shell
└─$ wget https://mercury.picoctf.net/static/a14be2648c73e3cda5fc8490a2f476af/warm
```
</details>
<details>
  <summary>Hint 3</summary>
  
 Run this program by entering the following in the Terminal prompt: 
 
```shell
└─$ chmod +x warm
└─$ ./warm
```
</details>
<details>
  <summary>Hint 4</summary>
  
-h and --help are the most common arguments to give to programs to get more information from them!

</details>
<details>
  <summary>Hint 5</summary>
  
Not every program implements help features like -h and --help.
</details>
<br>
<p>Lets get the file downloaded and get started</p>

```shell
└─$ wget https://mercury.picoctf.net/static/a14be2648c73e3cda5fc8490a2f476af/warm
```
<br>
<p>Make it exectutable using chmod.</p>

```shell
└─$ wget chmod +x warm
```
<br>
<p>Setting the exectuable loose</p>

```shell
└─$ ./warm
Hello user! Pass me a -h to learn what I can do!
```
Okay, let's wave the flag '-h' at it and see what it can do.

```shell
└─$ ./warm -h
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{REDACTED}
```
Well it gives us a flag, but it doesn't do much else...

<details>
  <summary>alternative solution</summary>
  
```shell
└─$ strings warm
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{REDACTED}
```
</details>
