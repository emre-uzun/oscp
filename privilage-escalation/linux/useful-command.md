
# Linux Privilage Escalation Notes

* **Guide**

https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

* **General Command**

```
cat /etc/issue
uname -a
cat /proc/version
find / -perm -u=s -type f 2>/dev/null
find / -perm -g=s -type f 2>/dev/null
find / -perm -2 ! -type l -ls 2>/dev/null @Yazma yetkisi olan dosyalar
```

