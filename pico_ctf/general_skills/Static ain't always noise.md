![image](https://github.com/jowp-code/ctf/assets/121969489/82b880b6-6e8d-4f00-8299-f579e31f3144)

<p>Lets grab the files and see what we are working with.</p>

```shell
 └─$ wget https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/static | wget https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/ltdis.sh
```
<p>We are met with a 64 bit ELF file and a shell script that appears to be used to perform disassemnbly. Let's make the file's executable and run them.</p>

```shell
 └─$ chmod +x ltdis.sh
 └─$ chmod +x static
```

<p>Beginning with 'static'</p>

```shell
└─$ ./static         
Oh hai! Wait what? A flag? Yes, it's around here somewhere!
```
<p>So that didn't do much, so let's follow up with the ltdis.sh script.</p>

```shell
└─$ ./ltdis.sh
Attempting disassembly of  ...
objdump: 'a.out': No such file
objdump: section '.text' mentioned in a -j option, but not found in any input file
Disassembly failed!
Usage: ltdis.sh <program-file>
Bye!
```
<p>Based on the Usage: we need to feed it a file name for processing.</p>

```shell
└─$ ./ltdis.sh static
Attempting disassembly of static ...
Disassembly successful! Available at: static.ltdis.x86_64.txt
Ripping strings from binary with file offsets...
Any strings found in static have been written to static.ltdis.strings.txt with file offset

```
<p>The shell code we ran has stripped strings out of the binary and seperated them into their own text file, in the other file is the dissasembled code. Checking in on the strings file we can find out flag.</p>

```shell
└─$ cat static.ltdis.strings.txt | grep picoCTF
   1020 picoCTF{REDACTED}
```

<p>Alternatively we could have simply run strings on the binary and the flag is found.</p>

```shell
└─$ cat strings static
picoCTF{REDACTED}

```
