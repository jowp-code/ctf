![image](https://github.com/jowp-code/ctf/assets/121969489/3d5b880d-c7e8-4ac2-99c7-8da609a6da22)
<br>
<p>I'm still learning python so I wasn't sure what was really at play here.</p>
<br>

```python
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

# Take each character and convert to binary, take the first 4 and last 4 bits and make them integers based on ALPHABET?.
def b16_encode(plain):
        enc = ""
        for c in plain:
                binary = "{0:08b}".format(ord(c))
                enc += ALPHABET[int(binary[:4], 2)]
                enc += ALPHABET[int(binary[4:], 2)]
        return enc

# Need to subtract t2 from t1?
def shift(c, k):
        t1 = ord(c) - LOWERCASE_OFFSET
        t2 = ord(k) - LOWERCASE_OFFSET
        return ALPHABET[(t1 + t2) % len(ALPHABET)]

# assert that the key is somewhere in the ALPHABET (16) so we would get at least 16 ouputs to dump all possible flags?
flag = "redacted"
key = "redacted"
assert all([k in ALPHABET for k in key])
assert len(key) == 1


# to get our encoded flag, we are shifting bits based on c, and our key?
b16 = b16_encode(flag)
enc = ""
for i, c in enumerate(b16):
        enc += shift(c, key[i % len(key)])
print(enc)
```
<br>
<p>At this point I had an incling as to what was going on, but I couldn't get a script going to catch the flag, thank fully soeone else did. This code was taken from <a href="https://github.com/scrymastic">scrymastic</a> I did not write it or modify it, except to add my own alphabet for decoding.</p>
<br>

```python
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

encrypted_flag = "ihjghbjgjhfbhbfcfjflfjiifdfgffihfeigidfligigffihfjfhfhfhigfjfffjfeihihfdieieih"

def b16_decode(plain):
    dec = ""

    for i in range(0, len(plain), 2):
        i1 = ALPHABET.index(plain[i])
        i2 = ALPHABET.index(plain[i+1])
        b1 = bin(i1)[2:].zfill(4)
        b2 = bin(i2)[2:].zfill(4)
        b = b1 + b2
        d = int(b, 2)
    
        dec += chr(d)
    return dec

def unshift(c, k):
    t1 = ord(c) - LOWERCASE_OFFSET
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET[(t1 - t2) % 16]

for key in ALPHABET:
    dec = ""
    for i, c in enumerate(encrypted_flag):
        dec += unshift(c, key)
    print(b16_decode(dec))
```
<br>
<p>Returning multiple outputs, one of which is our flag.</p>
<br>

```shell
qQqRY[YSVUT[UYWWWYUYTS
v`@`AHJHwBEDvCurJuuDvHFFFuHDHCvvBssv
REDACTED
TcNcd.N/&(&U #"T!SP(SS"T&$$$S&"&!TT QQT
CR=RS=DCBOBBCBCC@@C
2A,AB
321>112122??2
!01ûóõó"ýðÿ!þ -õ  ÿ!óñññ óÿóþ!!ý..!
/
/ ê
ëâäâìïîíäîâàààâîâíì
ùÙùÚÑÓÑÛÞÝÜ
           ÓÝÑßßßÑÝÑÜÛ


ÈèÉÀÂÀÿÊÍÌþËýúÂýýÌþÀÎÎÎýÀÌÀËþþÊûûþ
íü×üý·×¸¿±¿î¹¼»íºìé±ìì»í¿½½½ì¿»¿ºíí¹êêí
ÜëÆëì¦Æ§® ®Ý¨«ªÜ©ÛØ ÛÛªÜ®¬¬¬Û®ª®©ÜÜ¨ÙÙÜ
ËÚµÚÛµÌËÊÇÊÊËËËÈÈË
ºÉ¤ÉÊ¤»º¹¶¹¹º¹ºº··º
©¸¸¹st{}{ªuxw©v¨¥}¨¨w©{yyy¨{w{v©©u¦¦©
§§¨bcjljdgfelfjhhhjfjed
```
