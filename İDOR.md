**IDOR (Insecure Direct Object Reference)**



**Nedir?**



IDOR, bir kullanıcının yetkisi olmadan başka bir kullanıcıya ait verilere erişebilmesi veya bu veriler üzerinde işlem yapabilmesi durumudur. Bu açık, genellikle doğrudan nesne referanslarının (ID, kullanıcı numarası, dosya adı vb.) kontrolsüz şekilde kullanılmasıyla oluşur.



**Basit Tanım**



Eğer bir uygulama, kullanıcıların erişmesi gereken verileri sadece ID gibi tahmin edilebilir değerlerle ayırıyor ve ek bir yetkilendirme kontrolü yapmıyorsa IDOR açığı oluşabilir.



**Örnek**



Bir profil sayfası şu şekilde çalışıyorsa:



```

profile?id=1

```



Eğer bu değer değiştirilerek:



```

profile?id=3

```



başka bir kullanıcıya ait bilgiler görüntülenebiliyorsa, bu bir IDOR zafiyetidir.



**Etkileri**



\* Başka kullanıcıların verilerini görüntüleme

\* Hassas bilgilere erişim

\* Veri değiştirme (bazı durumlarda)

\* Yetkisiz işlem yapma



\## **Sebep**



\* Yetkilendirme kontrolünün eksik olması

\* Sadece client-side güvenliğe güvenilmesi

\* Tahmin edilebilir ID yapıları



\## **Korunma Yöntemleri**



\* Her istekte server-side authorization kontrolü yapmak

\* Direct object reference yerine dolaylı referanslar kullanmak

\* Kullanıcıya ait kaynakları oturum (session) üzerinden doğrulamak



