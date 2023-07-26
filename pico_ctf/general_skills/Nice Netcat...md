![image](https://github.com/jowp-code/ctf/assets/121969489/43341a8e-1d86-473c-b2e1-e86d26167e03)

<details>
  <summary>Hint 1</summary>

  You can practice using netcat with this picoGym problem: <a href="https://play.picoctf.org/practice/challenge/34">what's a netcat?</a>

</details>

<details>
  <summary>Hint 2</summary>

  You can practice reading and writing ASCII with this picoGym problem: <a href="https://play.picoctf.org/practice/challenge/22">Let's Warm Up</a>

</details>

<a href="https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwj346HO26yAAxWzk4kEHY0TAikQFnoECCgQAQ&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FNetcat&usg=AOvVaw2WTHEUvlGpAAbOQfmQLYBj&opi=89978449">Netcat</a> is a computer networking utility for reading from and writing to network connections using TCP or UDP.

```shell
└─$ nc mercury.picoctf.net 43239
```
It appears to be returning ascii code, there are many options <a href="https://www.duplichecker.com/ascii-to-text.php">online</a> for figuring out what this means. However, I am going to solve it using python. We can copy and paste the dump, or capture the output to a file:

```shell
└─$ nc mercury.picoctf.net 43239 > ascii.txt
```
<br>

```Python
cat_text = [112, 105, 99, 111, 67, 84, 70, 123, 103, 48, 48, 100, 95, 107, 49, 116, 116, 121, 33, 95, 110, 49, 99, 51, 95, 107, 49, 116, 116, 121, 33, 95, 55, 99, 48, 56, 50, 49, 102, 53, 125, 10]
 
res = ""
for val in cat_text:
    res = res + chr(val)
 
print ("The flag is: ", str(res))
```
<br>

```shell
└─$ nc python3 solve.py
The flag is: picoCTF{REDACTED}
```
My cat_text will output the wrong flag for you. It's necessary to paste your own array in, generally the last part of the flag unique.
