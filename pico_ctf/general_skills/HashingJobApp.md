![image](https://github.com/jowp-code/ctf/assets/121969489/095c07b1-859c-4e68-8fed-743cc46e1f25)
<br>
<p>Connecting to the system we are met with a prompt, to produce and md5 hash of random text. My randomized words were 'computer hackers', 'Cleopatra', and 'bad dogs'. We can use online tools to solve these md5sums, I used the CLI.</p>
<br>

```shell
└─$ echo -n computer hackers | md5sum
1034abc8025edcc22f58c35abc21e36f

└─$ echo -n Cleopatra | md5sum
f8cbe5a99675fff11ed4d83fc16e2071

└─$ echo -n bad dogs | md5sum
60cc96ffdc458c98395d6e7b6878a6e9

```
<br>
<p>Moving quickly I pasted these outputs into the NC shell as it times out after a short period.</p>
<br>

```shell
└─$ nc saturn.picoctf.net 52679
Please md5 hash the text between quotes, excluding the quotes: 'computer hackers'
Answer: 
1034abc8025edcc22f58c35abc21e36f
1034abc8025edcc22f58c35abc21e36f
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'Cleopatra'
Answer: 
f8cbe5a99675fff11ed4d83fc16e2071
f8cbe5a99675fff11ed4d83fc16e2071
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'bad dogs'
Answer: 
60cc96ffdc458c98395d6e7b6878a6e9
60cc96ffdc458c98395d6e7b6878a6e9
Correct.
picoCTF{REDACTED}
```
<br>
<p>Our flag is picoCTF{REDACTED}</p>
