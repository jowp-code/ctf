![image](https://github.com/jowp-code/ctf/assets/121969489/9aad4dd4-7a5f-42f4-b1e3-3be6618affb9)

<details>
  <summary>Hint 1</summary>

  Submit your answer in our flag format. For example, if your answer was 'hello', you would submit 'picoCTF{hello}' as the flag.

</details>
<br>
<p>We don't have a lot going on here, but we have an opportunity to do something to help us in later challenges, we can easily looking 0x70 and find that it is the lower case letter 'p'. But we can also use python to save us time searching online. This code is from <a href="https://codeigo.com/python/convert-hex-to-string/">codeigo</a> and modifed to accept an input rather than a static code</p>
<br>

```Python
import codecs
 
my_string = input('Enter Hex Code: ')
my_string_bytes = bytes(my_string, encoding='utf-8')
 
binary_string = codecs.decode(my_string_bytes, "hex")
print(str(binary_string, 'utf-8'))

```

<br>
<p></p>
<br>

```shell

```

<br>
<p></p>
<br>

```shell

```
