![image](https://github.com/jowp-code/ctf/assets/121969489/67bf6203-d6ba-42ce-aba8-9888ce5d49bc)

<br>
<p>Downloading the files they are both filled with static, Neither shows any promise with 'binwalk' or 'exiftool' so lets try stegsolve</p>
<br>
<p>Stegsolve lets us combine images together using a bunch of different techniques</p>
<br>

```shell
└─$ ./stegsolve.jar
```
<br>
<p>This will start the gui</p>
<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/0e677687-f261-4855-8762-2b0df7eab387)
<br>
<p>First image loaded, now we need to combine the second image</p>
<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/7ae25f4a-1bf5-4f76-8561-89d0eb2aa4c5)
<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/cd60a53d-e03f-4a5b-83e1-ec74fa441753)
<br>
<p>We get our flag using the ADD function.</p>



