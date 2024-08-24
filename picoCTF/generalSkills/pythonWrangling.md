![image](https://github.com/jowp-code/ctf/assets/121969489/902b93ef-e5ba-475d-bffb-4e3be4f0f196)
<br>
<details>
  <summary>Hint 1</summary>
  
Get the Python script accessible in your shell by entering the following command in the Terminal prompt:
  
  ```shell
  └─$ wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.py
  ``` 
</details>
<details>
  <summary>Hint 2</summary>
  
  ```shell
  └─$ man python
  ``` 
</details>
<br>
<p>Downloading the python script, password and file to decrypt all at once.</p>

```shell
└─$ wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.py | wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/pw.txt | wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/flag.txt.en
```

<p>The flag.txt.en appears to be encrypted. Base64 with a <a href="https://cryptography.io/en/latest/fernet/">fernet</a> secret key.</p>
<br>

```python
    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)
```

<p>It's very likely the string from pw.txt, so let's run the python script and see what we have.</p>
<br>

```shell
└─$ python3 ende.py
Usage: ende.py (-e/-d) [file]
```
<p>So there are 2 options here -e (encrypyt) and -d (decrypt) for our file. Let's run it in decrypt mode against our file.</p>
<br>

```shell
└─$ python3 ende.py -d flag.txt.en
Please enter the password:
```
<p>Using the password from pw.txt, we are granted our flag.</p>
<br>

```shell
└─$ python3 ende.py -d flag.txt.en
Please enter the password:aa821c16aa821c16aa821c16aa821c16
picoCTF{REDACTED}
```
