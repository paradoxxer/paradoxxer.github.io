---
title : "[TR] Shellshock Zafiyeti ve Sömürülmesi"
search : true

---

Merhabalar bu yazımda sizlere shellshock zafiyeti nasıl sömürülür, ne gibi tehlikeler oluşturabilir, nasıl çalışır gibi konulardan bahsedeceğim ve sonrasında sanal bir makinede nasıl sömürülür göstereceğim.

Bash(Bourne Again Shell) genelde web uygulamaları tarafında mevcut değildir.Fakat bu zafiyetin bulundugu bir sistemde modifiye edilmiş bir HTTP isteği ile komut çalıştırabilir ve eğer istersek, zafiyetli sistemden interaktif bir reverse shell alabiliriz.

Zafiyetin nasil sömürüldüğüne geçmeden önce neden bu tür bir zafiyet söz konusu oldugunu anlayalım, bunun için CGI nedir bundan bahsetmek gerekiyor;

Common Gateway Interface (CGI) : Cgi web sunuculari için programların konsol uygulamaları gibi çalıştırılabilmesini saglayan bir standart sunar.Bu tür programlara CGI scriptleri denir.Bu scriptlerin nasil çalıştırılacağının kararın sunucu verir.Genelde CGI scriptleri bir HTTP isteginden sonra çalıştırılır.CGI çagırıldığında web sunucusu yeni bir proses başlatıp CGI’i çalıştırır.Bu durumda bir bash prosesi başlatılıp ardından CGI scripti çalışır.Daha detaylı bilgi için;

[https://en.wikipedia.org/wiki/Common_Gateway_Interface](#)

Sanal makinedeki hedef sistemimiz bir Apache web sunucusu olduğundan, Apache CGI ile nasıl etkileşime giriyor bakalım.

İstemci tarafından istek yapıldığında, Apache web sunucu CGI scriptine elindeki bilgiyi aktarmak durumundadır.Bunun için çevre değişkenlerini kullanır(Enviroment Variable).Bu degişkenler CGI scriptinin içinde bulunurlar ve Apache’nin elindeki bilgiyi aktarmasini saglar.


Buradaki sorunun kaynağı bash'in çevre degişkeni içerisinde bir fonksiyon tanımlayabilmesidir.Bu sayede fonksiyon tanımlandıktan sonra sistemde komut çalıştırabiliriz.

Eger bir makinede bu tür bir zafiyet var mı yok mu belirlemek istiyorsak, önce web sunucusundan cgi-bin/status adında bir dizin çağırılacak mı bakmaliyiz eger makine zafiyetli ise ve cgi-bin/status dizini çağırılıyorsa basit bir HTTP istegi ile makinede komut çalıştırabiliriz.

Dizinin çağrılıp çağrılmadığını kontrol etmek için burp proxy kullanıyorum.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/shellshock/cgi-bin.jpg)

Varsayılan istek;

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/shellshock/varsayılan_istek.jpg)

Dizin çağrıldığına göre modifiye edilmiş HTTP isteğimi yollayıp zafiyet var mı yok mu görebilirim.

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/shellshock/modifiye_istek.jpg)

Dönen cevap ve komutun çalışması;

![alt]({{ site.url }}{{ site.baseurl }}/assets/img/shellshock/yanıt.jpg)

Bir sonraki yazıda görüşmek dileğiyle.







