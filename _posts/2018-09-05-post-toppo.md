---
title : "[TR] Vulnhub Toppo-1"
search : true

categories:
    -pentest
    
---

[https://www.vulnhub.com/entry/toppo-1,245/](#)

Öncelikle makinenin ip adresini öğrenmek için netdiscover aracını kullanarak arp taraması yapıyorum.
```
netdiscover -r 10.0.2.1/24
```

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/netdiscover.jpg)

Ping atarak makinenin ayakta olduğunu doğruluyorum.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/ping.jpg)

Sonrasında makinenin açık portlarını, işletim sistemini, çalışan servisleri ve bu servislerin sürüm bilgilerini öğrenmek için nmap taraması yapıyorum.Ayrıca nmap veritabanında bulunan bir zafiyeti var mı bunu da kontrol ediyorum.

```
nmap -sT -sV -sC -O -p- 10.0.2.5 --script vuln -o nmap_ciktisi
```
![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/nmap.jpg)

80 portunda çalışan bir web sunucusu olduğunu görüp siteyi ziyaret ediyorum.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/website.jpg)

Sitede yanlış yapılandırılmış bir dizin veya bir giriş sayfası bulabilmek amacıyla fuzzing yapıyorum.

```
gobuster -w usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.0.2.5
```
![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/fuzzing.jpg)

Admin dizini ilgimi çekiyor, ziyaret ediyorum.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/admin.jpg)

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/note.jpg)

Bu dizinde site yöneticisinin yazdığı bir notu buluyorum, bu notu görmemin sebebi site yöneticisinin siteyi doğru yapılandırmamış olması. Siber güvenlikte en zayıf halka genelde insan faktörü oluyor.Bu makinede de simüle edilmek istenen senaryo bu.

Elimizde 12345ted123 parolası bulunuyor bu şifreyi deneyebileceğim herhangi bir giriş paneli bulamadığım için şansımı -22.port açık olduğundan dolayı- ssh servisinde deniyorum.Elimizde bir kullanıcı adı yok fakat şifremizin içinde bariz bir ipucu var.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/ssh.jpg)

Ted kullanıcı adı ve sitede bulduğum parola ile makineye başarıyla giriş yapıyorum.

Yetki yükseltme saldırısı yapabilmek amacıyla ```uname -a``` yapıp linux sürümünü öğreniyorum. İnternette bulduğum scriptler çalışmayınca başka sömürü kodları bulabilmek amacıyla linux-exploit-suggester aracını makineye upload ediyorum.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/simplehttp.jpg)

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/privesc.jpg)

```
./linux-exploit-suggester.sh
```

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/cve.jpg)

Yukarıda görülen çoğu script'i deneyip başarısız olduktan sonra başka çözüm yolları aramaya başladım.İnternetten yaptığım araştırmalar sonrasında /etc/sudoers ' i kontrol ettim.

 ![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/sudoers.jpg)
 
 Sudoers dosyasında hangi kullanıcının hangi uygulamada ne kadar yetkisi var görebiliyoruz.Bu dosya sistem yöneticilerinin, kullanıcı yetkilerini belirlemesi için vardır. Mesela sistem yöneticisi A programını, B root olmayan normal kullanıcı için A programına özel root yetkileri verebilir Yani B kullanıcısı A programını root yetkileri ile çalıştırabilir hale gelir. 
 
 Yukarıda gördüğümüz gibi ted kullanıcısı olarak awk dilinin tüm yetkilerine sahibiz. Ayrıca bu yetkileri kullanmamız için ise herhangi bir parola girmemiz gerekmiyor(NOPASSWD).
 
 Bu bilgilerle awk araştırıldığında awk'ın bir scripting dili olduğunu veri manipülasyonu ve raporlama gibi işlemlerde kullandığını öğrendim.Benim için en önemlisi ise awk ile sistem komutları da çalıştırılabilmesiydi.Bu bilgileri elde ettikten sonra flag.txt 'yi okuyabildim.
 
![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/awk.jpg)
  
![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/root.jpg)

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/toppo/flag.jpg)














