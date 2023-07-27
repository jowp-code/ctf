![image](https://github.com/jowp-code/ctf/assets/121969489/7f904081-4011-4828-86b0-fc22ac9c7e2d)
<br>
<p>On the surface this should be a caeser cipher, but by how many digits are we shifting? Code borrowed from <a href="https://mregraoncyber.com/picoctf-writeup-caesar/">MrEgran</a></p>
<br>

```python
alphabet="abcdefghijklmnopqrstuvwxyz"
encrypted_flag = "gvswwmrkxlivyfmgsrhnrisegl"
length_text = len(encrypted_flag)
for j in range(26):
	for i in range(length_text):
		print(chr(((ord(encrypted_flag[i])-j)-97)%26+97), end='')
	print()
print()
```
<br>
<p>Outputs the following</p>
<br>

```shell
└─$ python3 selve.py
gvswwmrkxlivyfmgsrhnrisegl
furvvlqjwkhuxelfrqgmqhrdfk
etquukpivjgtwdkeqpflpgqcej
dspttjohuifsvcjdpoekofpbdi
crossingtherubicondjneoach
bqnrrhmfsgdqtahbnmcimdnzbg
apmqqglerfcpszgamlbhlcmyaf
zolppfkdqeboryfzlkagkblxze
ynkooejcpdanqxeykjzfjakwyd
xmjnndiboczmpwdxjiyeizjvxc
wlimmchanbylovcwihxdhyiuwb
vkhllbgzmaxknubvhgwcgxhtva
ujgkkafylzwjmtaugfvbfwgsuz
tifjjzexkyvilsztfeuaevfrty
sheiiydwjxuhkrysedtzdueqsx
rgdhhxcviwtgjqxrdcsyctdprw
qfcggwbuhvsfipwqcbrxbscoqv
pebffvatgurehovpbaqwarbnpu
odaeeuzsftqdgnuoazpvzqamot
nczddtyrespcfmtnzyouypzlns
mbyccsxqdrobelsmyxntxoykmr
laxbbrwpcqnadkrlxwmswnxjlq
kzwaaqvobpmzcjqkwvlrvmwikp
jyvzzpunaolybipjvukqulvhjo
ixuyyotmznkxahoiutjptkugin
hwtxxnslymjwzgnhtsiosjtfhm
```
<br>
<p>At first glance it's giberish, but on closer inspection there is one line that is at least in English</p>
<br>
<p>picoCTF{crossingtherubicondjneoach}</p>
