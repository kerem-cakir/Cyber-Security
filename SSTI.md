**Server-Side Template Injection (SSTI)**



**Nedir?**



Server-Side Template Injection (SSTI), web uygulamasının kullanıcıdan aldığı veriyi güvenli şekilde metin olarak işlemek yerine, şablon motoru (template engine) tarafından kod gibi değerlendirmesi sonucu ortaya çıkan bir güvenlik açığıdır.



Normalde kullanıcı girdisi yalnızca ekranda gösterilecek içerik olmalıdır. Ancak SSTI durumunda bu veri, sunucu tarafında çalıştırılabilir bir ifadeye dönüşebilir.



**Nasıl Oluşur?**



SSTI genellikle şu durumlarda ortaya çıkar:



\* Kullanıcı girdisi doğrudan template içine eklenir

\* Girdi sanitizasyonu (temizleme/filtreleme) yapılmaz

\* Template engine üzerinde dinamik değerlendirme (render/eval benzeri yapı) kullanılır



**Örnek mantık:**

Kullanıcıdan gelen veri “sadece metin” olarak değil, template expression olarak işlenirse açık oluşur.



**Basit Anlatım**



*Bunu şöyle düşünebiliriz:*



Bir sistem sana gelen mesajı sadece yazı olarak göstermek yerine, içindeki komutları da çalıştırmaya başlıyor.

Bu durumda kullanıcı “veri” değil, “talimat” göndermiş olur.



**## Tespit Yöntemleri**



SSTI olup olmadığını anlamak için bazı basit testler yapılabilir:



\* Girdiye matematiksel ifadeler yazmak (örnek: `7\*7`)

\* Template expression karakterlerini denemek (kullanılan dile göre değişir)

\* Beklenen çıktının yerine hesaplanmış sonuç dönmesi



Eğer sistem girilen ifadeyi düz metin olarak değil, işlenmiş sonuç olarak döndürüyorsa şüphe oluşur.



**## Etkileri**



*SSTI açığının seviyesi kullanılan template engine ve yapılandırmaya bağlıdır:*



\* Bilgi sızdırma (dosya, environment değişkenleri)

\* Sistem içi hassas verilerin okunması

\* Bazı durumlarda uzaktan kod çalıştırma (RCE)



**Önemli Not**



Her SSTI açığı doğrudan RCE anlamına gelmez. Etki seviyesi kullanılan teknoloji ve sistem izinlerine göre değişir.



**## Korunma Yöntemleri**



\* Kullanıcı girdisini asla doğrudan template içine gömmemek

\* Input validation ve sanitization uygulamak

\* Güvenli template kullanım kurallarına uymak

\* Auto-escape özelliklerini aktif kullanmak



**## Notlarım**



Bu konu, özellikle web uygulama güvenliğinde önemli bir zafiyet türüdür.

Farklı frameworklerde (Jinja2, Twig, Freemarker vb.) farklı davranışlar gösterebilir.



