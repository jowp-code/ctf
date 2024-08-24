![image](https://github.com/jowp-code/ctf/assets/121969489/9aad4dd4-7a5f-42f4-b1e3-3be6618affb9)

<details>
  <summary>Hint 1</summary>

  Submit your answer in our flag format. For example, if your answer was 'hello', you would submit 'picoCTF{hello}' as the flag.

</details>
<br>
<p>We don't have a lot going on here, but we have an opportunity to do something to help us in later challenges, we can easily looking 0x70 and find that it is the lower case letter 'p'. But we can also use python to save us time searching online. This code is from <a href="https://codeigo.com/python/convert-hex-to-string/">codeigo</a> and modifed to accept an input rather than a static piece of code. Note we are also going to strip the '0x' from the hex code</p>
<br>

```Python
import codecs
 
my_string = input('Enter Hex Code: ')
my_string_bytes = bytes(my_string, encoding='utf-8')
 
binary_string = codecs.decode(my_string_bytes, "hex")
print(str(binary_string, 'utf-8'))

```

<br>
<p>Running the script with python we get a prompt for data input, where we can enter our hex '70'</p>
<br>

```shell
└─$ python3 solve.py
Enter Hex Code: 70
p            
```

<br>
<p>We recieve the expected value of 0x70, a lower case 'p'. picoCTF{p} is therefore our flag, to take testing one step further what would the value be for the following hex code, (0x68, 0x65, 0x6c, 0x6c, 0x6f) if we strip the 0x away we are left with a hexadecimal code of 68656c6c6f.</p>
<br>

```shell
└─$ python3 solve.py
Enter Hex Code: 68656c6c6f
hello
```
<br>
<p>We are met with the string 'hello' We could take it a step further and remove the 0x in python itself, but this is simple enough for now.</p>
