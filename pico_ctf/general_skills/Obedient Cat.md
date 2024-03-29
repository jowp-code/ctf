
# Obedient Cat

![image](https://github.com/jowp-code/ctf/assets/121969489/0d9a037d-2a6d-4e77-b88e-3a0605779f12)
<br>
<details>
  <summary>Hint 1</summary>
  
  Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.

```shell
└─$ Enter Command Here
```
</details>
<details>
  <summary>Hint 2</summary>
  
To get the file accessible in your shell, enter the following in the Terminal prompt: 
  
  ```shell
  └─$ wget https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c75/flag
  ``` 
</details>
<details>
  <summary>Hint 3</summary>
  
To get more information on the 'cat' command use the following: 
  
  ```shell
 └─$ man cat
  ``` 
</details>

Using our terminal we can download the flag, but how do we view the contents from the terminal? There are a few text editors that are preinstalled such as vim or nano, but in this instance CAT (concatenate) will do. First we need to download the file using wget or curl.
<br>
```shell
 └─$  wget https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c74/flag
```
Making sure we have the file using LS to check the current directory listing.
<br>
```shell
└─$  ls
flag
```
Read the flag using cat.
<br>
```shell
└─$  cat flag
picoCTF{REDACTED}
```
Alternatively you could simply use the GUI and download the file manually and open it via any text editor, it is "in the clear" after all.
