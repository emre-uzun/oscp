# BOF Windows
> OSCP sınavı BOF adımları

## Overview
uygulamayı analiz etmek için 3 method vardır.
* Kaynak kod analizi
* Reverse engineering
* Fuzzing

## Fuzzing

SLMail uygulaması için ornek fuzzing

for @ ("A"*30)

#### Hangi Karakterde patladığını bulma

- Uygulamayı admin olarak aç
- Immumity Debugger ı aç
- File > Attach SLmail OK
- (optional) Font değişikliği: Sağ tık > Appearance > Font > OEM fixed
- (optional) Hex 8->16 byte: Sağ tık > Hex > Hex/ASCI 16 bytes

Immumity debugger defaultta stop durumdadır. 
- Start'a tıklanır >
- Fuzzing script çalıştırılır.
- App crash olur, debugger durur.
- Fuzzing script hangi byte da crash olduguna gore script duzenlenebilir.

> Not: OSCP sınavında poc.py kodu verilmektedir. Buraya kadarki kısım ozet BOF anlatımı içindir.


## Crash Replication

```
buffer = 'A'*2700
```

#### NOT: Uygulama her seferinde task manager dan kapatılmalı ve immunity debuuger a attach edilmelidir.

## Controlling EIP (OSCP Soru Baslangic)

Kali:
```
locate pattern_create
/usr/share/metasploit-framework/tools/pattern_create.rb 2700
```

- Bu karakterler serisi alınır ve poc kodu degistirilir: 

```
buffer = '<kopyalananSeri>'
```

- Exploit kodu çalıştırılır ve EIP'deki deger alınır

Kali:
```
/usr/share/metasploit-framework/tools/pattern_offset.rb <EIPdeger>

Dönen Örnek Cevap:

Offset 2606
```

POC.py`değiştirilir.
```
buffer = 'A'*2606 + 'B'*4 + 'C'*90
```
Not: 90 olması toplamda 2700 yapmak icin.

- Uygulama tekrar attach edilir. 
- EIP register 42424242 olmalı, yani BBBB.


