![image](https://github.com/jowp-code/ctf/assets/121969489/f3753f65-07da-4bb9-ae2a-c080ed5a1302)
<br>
<p><a href"https://en.wikipedia.org/wiki/Cron">Cron</a> jobs are scheduled tasks on linux servers, they can be used to schedule tasks down to the minute, days, weeks, and months. It's a powerful tool and one that can be abused for persistence, or privilege escalation.</p>
<br>
<p>If we use cd and tab we can see what 'cron' directories and files exist in the /etc/ folder, there are 5 folders we can look at, but one file, we may as well check out the file first.</p>

```shell
picoplayer@challenge:~$ cat /etc/crontab
# picoCTF{REDACTED}
```
<br>
<p>Our flag is right there, picoCTF{REDACTED}.</p>
