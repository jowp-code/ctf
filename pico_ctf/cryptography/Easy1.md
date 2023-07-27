![image](https://github.com/jowp-code/ctf/assets/121969489/a4786b80-9470-4464-870a-19c6b0594366)
<br>
<p> As explained <a href="https://www.tutorialspoint.com/cryptography_with_python/cryptography_with_python_one_time_pad_cipher.htm">here</a> a one time pad is One-time pad cipher is a type of Vignere cipher which includes the following features, it is an unbreakable cipher, the key is exactly same as the length of message which is encrypted, and the key is made up of random symbols. As the name suggests, key is used one time only and never used again for any other message to be encrypted.</p>
<br>
<p>In this instance we are provided with the key, table, and cipher text. We can solve using the table by taking the letters of the key down the left side of the table, and then going out to the cipher text letter, then going up to find the plain text.</p>
<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/18da8546-3dbf-40a1-a052-8fe23d63d67c)
<br>
<p>S O L V E C R Y P T O</p>
<p>U F J K X Q Z Q U N B</p>
<br>
<p>Using our table</p>
<br>

![image](https://github.com/jowp-code/ctf/assets/121969489/a079a95a-20d7-4fcc-8415-596f47e81e3e)
<br>
<p>We take the S from out key, and go down the left of the table to find S, we then go across the table until we find U, then we go straight up and that letter is our plain text.</p>
<br>
<p>S > U = C</p><p>O > F = R</p><p>L > J = Y</p><p>V > K = P</p><p>E > X = T</p><p>C > Q = O</p><p>R > Z = I</p><p>Y > Q = S</p><p>P > U = F</p><p>T > N = U</p><p>O > B = N</p>
<br>
<p>Finally we have our flag picoCTF{CRYPTOISFUN}</p>
