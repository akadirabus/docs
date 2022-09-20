# Coding Standard (SQL)

## Database

- Veritabanı isimleri tamamen büyük harflerden oluşmalıdır (`UPPER_CASE`), eğer birden fazla kelimeden oluşuyor ise alt tire ile kelimeler ayrılmalıdır.<br />
Örneğin: `EXAMPLE`, `EXAMPLE_DATABASE_NAME` şeklinde
- Veritabanlarının `Collation` bilgisi veritabanı yaratılır iken `TURKISH_CI_AI` olarak belirtilmelidir.
- Veritabanların `Owner` bilgisi veritabanı yaratılır iken `sa` kullanıcısı seçilmelidir.

## Table

- Tablo isimleri tamamen büyük harflerden oluşmalıdır (`UPPER_CASE`), eğer birden fazla kelimeden oluşuyor ise alt tire ile kelimeler ayrılmalıdır.<br />
Örneğin: `EXAMPLE`, `EXAMLE_TABLE_NAME`, `USER_LOGIN`  vb. şeklinde
- Tablo içerisinde kolon olarak başka bir tablonun referans id bilgisi var ise `ref` ile başlamalıdır. Örneğin : `refUser`, `refCountry`, `refCustomer` vb.
- Tablo kolonlarını `foreign key` olarak işaretlemiyoruz, foreign key ilişkilerini kendimiz yönetiyoruz
- Tablo kolon isimleri `CamelCase` olmalıdır. bunun tek istisnası `id` ve `ref` ile başlayan kolonlardır. Bunların isimlendirmeleri `lowerCamelCase` dir.<br />Örnek kolon isimlendirmeleri : `NameSurname`, `Address`, `Age`, `refUser`, `id` vb.
- Bir tablonun log tablosu var ise tablo isminin sonunda `_LOG` konulmalıdır. Örneğin : `USER` tablosunun log tablosu ismi `USER_LOG` şeklindedir.<br />
Log tablolarının ilk iki kolonu şunlar olmalıdır : `LogId` (Auto Increment) ve `Operation` (char(1) `I`, `U`, `D` değerlerini alabilir, kaydın insert, update veya delete olduğunu belirtir), kalan kolon bilgileri diğer tablo ile birerbir aynı olmalıdır.
- Tabloların gerekli alanlarına muhakkak `index` konulmalıdır. Eğer unique olması gereken kolon veya kolon grupları var ise `unique index` oluşturulmalıdır. Indexin ismi şu şekilde olmalıdır `ix_TABLE_NAME_ColumnName` örneğin: `ix_USER_Email` (Kullanıcı tablosu Email kolonunun indexi)

## View

- View isimleri de tablolar gibi tamamen büyük harflerden oluşmalıdır (`UPPER_CASE`), eğer birden fazla kelimeden oluşuyor ise alt tire ile kelimeler ayrılmalıdır.<br />
Örneğin: `VIEW_EXAMPLE_TABLE`, `VIEW_USER_EXAMPLE_TABLE`, `VIEW_EXAMLE_TABLE_NAME` şeklinde
- View isimleri `VIEW_` prefixi ile başlamalıdır.

## Function

- Fonksiyon isimleri `lower_case` olmalıdır ve tüm fonksiyon isimleri `fn_` prefixi ile başlamalıdır. Eğer birden fazla kelimeden oluşuyor ise alt tire ile kelimeler ayrılmalıdır.<br />Örneğin : `fn_example`, `fn_server_time`, `fn_user_name_surname` vb.

## Stored Procedure

- Stored procedure isimleri `lower_case` olmalıdır. Eğer birden fazla kelimeden oluşuyor ise alt tire ile kelimeler ayrılmalıdır.<br />
Örneğin : `web_user_login`, `web_user_logout`, `api_user_authentication`, `service_application_list` vb.
- Store procedure parametreleri ve içerisindeki değişkenler `lowerCamelCase` olmalıdır. Örneğin : `@refUser`, `@nameSurname`, `@password` vb.
- Store procedure içerisinde eğer `insert`, `update`, `delete` işlemleri yapılıyor ise muhakkak `BEGIN TRANSACTION` ve `COMMIT TRANSACTION` arasına alınmalıdır.
- Store procedurelerin ilk satırında `SET XACT_ABORT ON` kodu olmalıdır
- Store procedure un başında ilgili validasyonlar ve güvenlik kontrolleri yapılmalı, sonrasında işlem gerçekleştirilmelidir.
- Store procedure içerisinde boşluk ve tab kullanım standardına dikkat edilmelidir. Aşağıda örnekler bulabilirsiniz.

```sql
-- IF blogu örnekleri
-- BEGIN END ler bir TAB içeride başlamalıdır aşağıdaki gibi
-- Keywordler uppercase olmalıdır IF, BEGIN, END vb. gini

IF dbo.fn_user_is_active(@refUser) = 0
   BEGIN
   SELECT dbo.fn_status_code_forbidden() as [Status], 'Invalid credentials' as [Description]
   RETURN
   END

IF @amount <= 0
   BEGIN
   SELECT dbo.fn_status_code_bad_request() as [Status], 'Invalid amount' as [Description]
   RETURN
   END
```

```sql
-- Select sorgusu ornekleri
-- Yine tab ve boşluklar aşağıdaki örneklerdeki şekilde olmalıdır

SELECT id, NameSurname, Email, Picture
   FROM USER

SELECT TOP 1 id, NameSurname, Email, Picture
   FROM USER

SELECT id, NameSurname, Email, Picture
   FROM USER
   ORDER BY id DESC

SELECT id, NameSurname, Email, Picture
   FROM USER
   WHERE NameSurname LIKE '%Example%'
     AND RecordTime > '2022-01-01'
     AND RecordTime < '2022-12-01'
   ORDER BY id DESC

SELECT MAX(id) as UserCount
   FROM USER
   WHERE NameSurname LIKE '%Example%'
     AND RecordTime > '2022-01-01'
     AND RecordTime < '2022-12-01'
   GROUP BY RecordTime
```

```sql
-- Insert, Update, Delete örnekleri

INSERT INTO USER (NameSurname, Email, RecordTime)
   VALUES (@nameSurname, @email, dbo.fn_server_time())

UPDATE USER SET NameSurname = @nameSurname, Email = @email
   WHERE id = @refUser

UPDATE USER SET NameSurname = @nameSurname, Email = @email
   WHERE id = @refUser
     AND refCountry = @refCountry

DELETE FROM USER
   WHERE id = @refUser

DELETE FROM USER 
   WHERE id = @refUser
     AND Email = @email
     AND refCountry = @refCountry
```

```sql
-- Transaction kullanımı örneği

BEGIN TRANSACTION
INSERT INTO USER (NameSurname, Email, RecordTime)
   VALUES (@nameSurname, @email, dbo.fn_server_time())

DECLARE @refUser bigint = SCOPE_IDENTITY()

INSERT INTO USER_ADDRESS (refUser, Address, PostalCode)
   VALUES (@refUser, @address, @postalCode)
COMMIT TRANSACTION
```

## Trigger

- Trigger isimleri `trigger_` ile başlamalıdır.
- Triggerların isimlendirme formatı şu şekildedir, `trigger_TABLE_NAME_after_insert`, `trigger_TABLE_NAME_after_delete`, `trigger_TABLE_NAME_instead_of_insert` gibi

## Temp Table

- Store procedure içerisinde `Temp Table` kullanacak iseniz, bu temp tablolar `#TMP_` prefixi ile başlamalıdır.<br />
Örneğin: `#TMP_USER`, `#TMP_CUSTOMER` vb.

## Diğer Hususlar

- Sınıf, tablo, sp, fonksiyon isimlendirmeleri için benzer isimlerin sırası önemlidir ve birbirleri ile uyumlu olmalıdır.
- İsimlendirmelerde kullanılacak syntax kuralı şu şekildedir 

`[platform]_[class1]_[class2]_[class..n]_[property]_[action]`

* **Platform** : web, api, ws (birden fazla platformun bulunduğu veritabanlarında bu bilgi splere eklenmeli)
* **Property** : settings, bills, products
* **Class** : class1 > class2 > class3 …

örn : COMPANY > USER > SUBUSER

```cs
//Örnek sp isimlendirmeleri
web_company_create
web_company_update
web_company_product_insert
web_company_product_update
web_company_user_create
web_company_user_update
web_company_user_product_authorize
web_company_user_product_revoke
web_company_user_subuser_create
```

Bu şekilde componentler düzenli ve sıralı bir şekilde listelenecek ve aranan tablo veya sınıf daha kolay bulunabilecektir.

```cs
//Örnek DB Tabloları
USER
SETTINGS_USER
SETTINGS_MESSAGE_USER
MESSAGE_REPORT_USER
```

```cs
//Yerine
USER
USER_SETTINGS
USER_MESSAGE_SETTINGS
USER_MESSAGE_REPORT
```

```cs
//Yine bu kuralla benzer olarak örnek isimlendirme (stored procedure) 
get_company_settings_web
update_company_settings_web
```

```cs
//yerine
web_company_settings_get
web_company_settings_update    
```

olmalıdır.
