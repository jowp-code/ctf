![image](https://github.com/jowp-code/ctf/assets/121969489/d492f6c7-4ff3-4e4e-a9c0-9c0bc00009cd)
<br>
<p>In cryptography, the one-time pad is an encryption technique that cannot be cracked, but requires the use of a single-use pre-shared key that is larger than or equal to the size of the message being sent. In this technique, a plaintext is paired with a random secret key <a href="https://en.wikipedia.org/wiki/One-time_pad">(Wikipedia)</a>.</p>
<br>
<p>This <a href="https://picoctf2021.haydenhousen.com/cryptography/easy-peasy">article</a> really helped me get an idea of what I was looking at. Looking at the otp.py code there does appear to be an issue.</p>

```python
#!/usr/bin/python3 -u
import os.path

KEY_FILE = "key"
KEY_LEN = 50000
FLAG_FILE = "flag"


def startup(key_location):
        flag = open(FLAG_FILE).read()
        kf = open(KEY_FILE, "rb").read()

        start = key_location
        stop = key_location + len(flag)

        key = kf[start:stop]
        key_location = stop

        result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), flag, key))
        print("This is the encrypted flag!\n{}\n".format("".join(result)))

        return key_location

def encrypt(key_location):
        ui = input("What data would you like to encrypt? ").rstrip()
        if len(ui) == 0 or len(ui) > KEY_LEN:
                return -1

        start = key_location
        stop = key_location + len(ui)

        kf = open(KEY_FILE, "rb").read()

        if stop >= KEY_LEN:
                stop = stop % KEY_LEN
                key = kf[start:] + kf[:stop]
        else:
                key = kf[start:stop]
        key_location = stop

        result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), ui, key))

        print("Here ya go!\n{}\n".format("".join(result)))

        return key_location


print("******************Welcome to our OTP implementation!******************")
c = startup(0)
while c >= 0:
        c = encrypt(c)

```
<br>
<p>The code below is interesting, if stop is greater than or equal to KEY_LEN, then stop is equal to the modulus of KEY_LEN.</p>
<br>

```shell
       if stop >= KEY_LEN:
                stop = stop % KEY_LEN
                key = kf[start:] + kf[:stop]
```
<br>
<p>We can generate a template using "PWN", this will provide framework for contacting picoctf, and deploying our payload. The following python script will solve the ctf, the original and extremely well commented script is <a href"https://raw.githubusercontent.com/HHousen/PicoCTF-2021/master/Cryptography/Easy%20Peasy/script.py">here</a>. The payload was created by <a href="https://picoctf2021.haydenhousen.com/cryptography/easy-peasy">Hayden Housen</a>.</p>
<br>
<p>The following command will generate our template in preperation for our payload.</p>
<br>

```shell
pwn template --host mercury.picoctf.net --port 41934 otp.py
```
<br>
<p>Our extremely well commented payload is below I would try to explain some of it, but I wouldn't be doing it justice.</p>
<br>

```python
#===========================================================
#                    EXPLOIT GOES HERE
#===========================================================
io = start()
io.recvuntil("This is the encrypted flag!\n")
encrypted_flag = str(io.recvline(), "ascii").strip()
# The length of the flag is the length of the encrypted flag divided by 2
# because of line 19 (`"{:02x}".format(ord(p) ^ k)`) where string formatting
# is used to output the flag as a hexadecimal string where each character is
# represetned by 2 characters.
flag_len = int(len(encrypted_flag)/2)

# Create a filler payload to reset the `c` variable in `otp.py` to 0 so we
# can use the same key that was used to encrypt the flag.
filler = "a"*(50000-flag_len)
io.sendlineafter("What data would you like to encrypt? ", filler)

def xor_list_str(a, b):
    # `a` is a list and `b` is a string
    return ''.join(list(map(lambda p, k: chr(p ^ ord(k)), a, b)))

def hex_to_dec_list(input_hex):
    # Convert a hex string to a decimal array.
    # Split the hex string into groups of 2.
    input_hex = [input_hex[i:i+2] for i in range(0, len(input_hex), 2)]
    # Convert each two hex characters to decimal.
    output = [int(x, 16) for x in input_hex]
    return output

# Create a message that we know that is the same length as the flag so
# that the same key is used to encrypt it.
message = "a"*flag_len

io.sendlineafter("What data would you like to encrypt? ", message)
io.recvuntil("Here ya go!\n")

encrypted_message = str(io.recvline(), "ascii").strip()
# Convert the encrypted message output by the program to a list of decimal
# numbers.
encrypted_message = hex_to_dec_list(encrypted_message)
# Find the key by xoring the encrypted message with the known clear text
# message as described at https://cs.stackexchange.com/a/365.
key = xor_list_str(encrypted_message, message)
log.success("Found Key: %s" % key)

# Convert the encrypted flag output by the program to a list of decimal
# numbers that represent ascii characters.
encrypted_flag = hex_to_dec_list(encrypted_flag)
# Decrypt the flag by xoring it with the key.
flag = xor_list_str(encrypted_flag, key)

log.success("Found Flag: picoCTF{%s}" % flag)
```
<br>
<p>Running the payload against the one time pad produces our flag.</p>
<br>

```shell
└─$ python3 solve.py                                            
[+] Opening connection to mercury.picoctf.net on port 41934: Done
/home/kali/test.py:52: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  io.recvuntil("This is the encrypted flag!\n")
/home/kali/test.py:63: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  io.sendlineafter("What data would you like to encrypt? ", filler)
/home/kali/.local/lib/python3.11/site-packages/pwnlib/tubes/tube.py:840: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  res = self.recvuntil(delim, timeout=timeout)
/home/kali/test.py:81: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  io.sendlineafter("What data would you like to encrypt? ", message)
/home/kali/test.py:82: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  io.recvuntil("Here ya go!\n")
[+] Found Key: b'Q\xcb\x8a\xb4\x04\xf8\x00{\xdd
[+] Found Flag: picoCTF{REDACTED}
[*] Closed connection to mercury.picoctf.net port 41934
```

