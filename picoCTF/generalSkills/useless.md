![image](https://github.com/jowp-code/ctf/assets/121969489/9958f6c8-441e-4645-b2f4-3ecbc92afd3c)
<br>
<p>Lets get the machine started and ssh in using the provided credentials.</p>
<br>

```shell
ssh -p 64146 picoplayer@saturn.picoctf.net
```
<br>
<p>Using 'ls' we can see what's in our home directory, as promised there is a file sitting. Let's check it out.</p>
<br>

```shell
picoplayer@challenge:~$ ls
useless
picoplayer@challenge:~$ cat useless
#!/bin/bash
# Basic mathematical operations via command-line arguments

if [ $# != 3 ]
then
  echo "Read the code first"
else
        if [[ "$1" == "add" ]]
        then 
          sum=$(( $2 + $3 ))
          echo "The Sum is: $sum"  

        elif [[ "$1" == "sub" ]]
        then 
          sub=$(( $2 - $3 ))
          echo "The Substract is: $sub" 

        elif [[ "$1" == "div" ]]
        then 
          div=$(( $2 / $3 ))
          echo "The quotient is: $div" 

        elif [[ "$1" == "mul" ]]
        then
          mul=$(( $2 * $3 ))
          echo "The product is: $mul" 

        else
          echo "Read the manual"
         
        fi
fi

```
<br>
<p>Not much going on with the code, so let's do as we're told and read the manual.</p>
<br>

```shell
picoplayer@challenge:~$ man useless

useless
     useless, — This is a simple calculator script

SYNOPSIS
     useless, [add sub mul div] number1 number2

DESCRIPTION
     Use the useless, macro to make simple calulations like addition,subtraction, multiplication and division.

Examples
     ./useless add 1 2
       This will add 1 and 2 and return 3

     ./useless mul 2 3
       This will return 6 as a product of 2 and 3

     ./useless div 6 3
       This will return 2 as a quotient of 6 and 3

     ./useless sub 6 5
       This will return 1 as a remainder of substraction of 5 from 6

Authors
     This script was designed and developed by Cylab Africa

     picoCTF{REDACTED}

```
<br>
<p>We have our flag.</p>
<br>
