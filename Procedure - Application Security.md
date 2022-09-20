# Security Checklist

* Tüm veri iletişimi `SSL` üzerinden yapılıyor mu?  

* Hassas verileri _(kullanıcı adı, parola, kredi kartı, tckn, vb.)_ içeren isteklerde veri `POST` metoduyla iletiliyor mu?  
* Uygulamanın login kısmında ve public olan tüm formlarında sürekli giriş denemesine karşı önlem alındı mı? _(ip blocklama, rate limiting, captcha vb.)_
* Uygulamanın login, parolamı unuttum veya benzeri kısımlarında `şifre hatalı`, `böyle bir kayıtlı eposta bulunamadı` gibi saldırı yapan kullanıcıyı yönlendirici (bu ifadeden kullanıcı adının doğru olduğu anlaşılıyor örn.) ifadeler bulunmuyor olması lazım, buna karşı hata mesajları doğru şekilde oluşturuldu mu? 
* Kullanıcı şifre mekanizması oluşturuldu mu? Şifrelerin yeterince kompleks ve yeterli güvenli uzunlukta olması, gerekli kontrollerle sağlanıyor mu? 
* Veritabanında kullanıcının şifre vb. tüm hassas bilgileri geri çözülemeyecek şifreleme algoritmaları kullanılacak şekilde şifrelenerek tutuluyor mu? 
* Oturum yöntemi olarak oAuth2.0 vb. yapıda `TOKEN` bazlı bir giriş mekanizması oluşturuldu mu? 
* Oturum yönetimi için kullanılan ve uygulamayı kullanan bütün kullanıcılar için tekil olması gereken değerlerin (session id, token v.b.) güçlü bir rastgele veri üretecinden temin edildiği ve tahmin edilemez derecede karmaşık olduğu kontrol edilmelidir. 
* Oturum bilgisi zaman aşımına uğrayacak şekilde yapılandırılmalıdır. 
* Uygulamalarda başarılı kimlik doğrulama ve tekrarlayan kimlik doğrulama (re-authentication) neticesinde her zaman yeni bir oturum bilgisi oluşturulmalıdır. Çıkış işleminden sonra da var olan oturum bilgisi geçersizleştirilmelidir. 
* Oturum yönetimi vb. işlemler için çerezler gerekli olmadıkça kullanılmamalıdır, kullanılıyorsa da kullanılan çerez değerleri için `httponly` parametresi tanımlı olmalıdır. Buna ek olarak, HTTPS protokolü kullanılan bağlantılarda kullanılan çerez değerleri için secure parametresi tanımlı olmalıdır. 
* Kritik işlemlerde CSRF saldırılarına karşı `Token` veya `CAPTCHA` gibi güvenlik önlemleri alınıyor mu? 
* OTP doğrulama kullanılan projelerde OTP kodu karşılaştırması sunucu tarafında güvenli bir şekilde yapılıyor mu?  
* OTP doğrulama kullanılan projelerde, her istekte tekrar tekrar OTP şifresi göndertilmesi gibi durumlara karşı önlem alındı mı? 
* OTP doğrulama kullanılan projelerde, OTP kodunun kısa bir expire olma süresi yapısı kullanıldı ve kontrolü düzgün şekilde sağlanıyor mu? 
* Kullanıcıdan alınan her input, hem client-side da hem server side da veri uygunluğu kontrolünden geçiyor mu? 
* Kullanıcıdan alınan her input için, `XSS` ataklarını önlemek için inputun html, script vb. zararlı metin içermesine karşın önlenmler alındı mı?  
* Kullanıcıdan gelen tüm girdiler sunucu tarafında pozitif veri kontrolünden geçmelidir. Girdiler veri kontrolünden geçmeden önce `canonicalization` işlemine tabi tutulmalıdırlar 
* Sadece kullanıcıya özel olması gereken her ekran için, oturumu bulunan başka bir kullanıcının, diğer kullanıcıların verilerine erişebilmesinin _(örn : A kullanıcısının bir verisine, B kullanıcısı URL deki id yi veya başka yerdeki bir parametreyi manuel olarak değiştirip, izinsiz erişim sağlanması gibi?)_ önüne geçildi ve buna karşın gerekli yetki kontrolü önlemleri alındı mı? HTTP parametreleri değiştirilerek üçüncü şahısların bilgilerine yetkisiz olarak erişilmemelidir. 
* Kısıtlı erişim gerektiren bütün URL'lere, fonksiyonlara, obje referanslarına, servislere, uygulama verilerine, kullanıcı bilgilerine, güvenlik yapılandırma dosyalarına erişim denetlenmelidir. 
* Resim vb. gibi kullanıcıya özel kaynaklara erişimde yetki kontrolleri sağlandı mı? _(Örneğin kullanıcıya özel olması gereken bir resmin url si bilindiğinde başka kullanıcıların o url ye erişemiyor olması gerekir.)_
* Kullanıcıdan gelen veriler işletim sistemi komut satırına girmeden kontrol edilmeli ve düzgünleştirme işleminden `(escape)` geçirilmelidir. 
* Kullanıcıdan gelen herhangi bir veri olduğu şekliyle bir ekranda gösterilme veya olduğu şekliyle alıp hassas bir kısımda kullanılmaması gerekiyor, buna dikkat edildi mi? _(örneğin kullanıcıdan aldığınız bir bilgiyi, sunucuda bir path oluşturmak için kullanırsınız, bunu farkeden zararlı bir kullanıcı, bu bilgiyi keşfedip dosya adına ../../ gibi veriler ekleyip örneğin dosya sisteminize izinsiz erişebilir. Veya bir projeye yorum modülü koydunuz, kullanıcıdan aldığınız veriyi yorum listelemede otomatik olduğu gibi koyarsanız, kullanıcı oraya zararlı bir kod veya metin yazabilir ve o yorumu okuyan diğer tüm kullanıcılar riske girmiş olur)_ bunun önüne geçildi mi? 
* XSS saldırılarına karşı bütün kullanıcı girdileri dışarı aktarılmadan önce sunucu tarafında özel karakter kodlama (`output encoding`) işleminden geçirilmelidir. Güvenlik seviyesini artırmak için bu işlem kullanıcı girdilerinin tip, uzunluk, içerik denetlemesi yapılarak desteklenebilir 
* Projelerde, excel, resim, media vb. içerikler, webden erişme açık olan bir url altından ulaşılabilir şekilde olmamalı, buna dikkat edildi mi? Bu ve benzeri tarzda upload edilen veya generate edilen dosyalar web e erişimi olmayan yetkisiz kişiler ulaşamayacağı bir dizinde tutuluyor mu? 
* Projelerdeki ekranlara erişimde yetki mekanizması oluşturuldu mu, kullanıcı izin ve yetki seviyelerine göre bu erişimler doğru bir şekilde kısıtlanıyor mu? 
* Uygulamaların üzerinde koştukları sunucular, servis verdikleri dizinlerin içeriklerini listelememelidir, bunun için gerekli önlemler alınıyor mu? 
* Proje backoffice vb. modül içeriyorsa bu modüle erişimde IP kısıtlaması, sertifika kısıtlaması vb. yapılar ile public olarak erişelemeyecek şekilde bir yapıda oluşturuldu mu? 
* Resim vb. media içeriklerinde dosya isimleri uzun GUID ler verilerek oluşturuldu mu? 
* Projelerde string birleştirme ile sql sorgusu, dosya pathi vb. sql injection gibi güvenlik açıklarına sebep olacak işlemler yapılmıyor olması gerekiyor bunlara karşı önlemler alındı mı? 
* Projelerde _(servis, api, web api, web backend vb.)_, http istekleri, veritabanı bağlantıları vb. connection yapılan yapılarda, try-catch-finally vb. yapılar kullanılarak, bağlantıların düzgün şekilde open ve close edilmesi sağlanıyor mu? Bu tarz bağlantılarda sql i veya isteklerin kitlenmemesi için, gerekli timeout parametreleri düzgün bir şekilde configure ediliyor mu? 
* Genelde uygulamaların arama özelliğini kötüye kullanarak veritabanı üzerinde çok detaylı arama yaptırarak işlemciyi meşgul eden SQL genel arama karakter (%,* v.b.) saldırılarına karşı arama süresini kısıtlamak suretiyle önlem alınmalıdır. 
* Karşıdan dosya yükleme işlemlerinde yüklenilen dosya üzerinde isim, boyut, tip ve içerik kontrolü yapılmalıdır. 
* Upload edilen dosyalarda, dosyanın uzantısı, content type ı, media type ı boyutu vb. ilgili güvenlik kontrolleri yapılıyor mu? (Örneğin: resim upload edilen yerden batch script gibi zararlı dosyalar yüklenebilir bu gibi durumlara karşı önlemler alındı mı sunucu tarafında?) 
* Kullanıcıların bir yazılım veya bot aracılığıyla sistemlerimize sürekli istek yapmasının önüne geçiliyor mu? Belirli işlem sayılarından sonra `429 Too Many Request` gibi hata dönüyor muyuz engelleyip? 
* Projedeki yapı kararına göre, bazı projelerde aynı anda oturum açılamaması, veya yeni oturum açıldığında eski oturumun düşürülmesi gibi yapılar doğru şekilde oluşturuluyor mu? 
* Projelerde login logları, yapılan istek logları vb. loglamalar düzgün şekilde kayıt altına alınıyor mu? 
* Yazılan tüm SQL fonksiyonlarında erişilmek, güncellenmek veya silinmek istenen verilerin işlemlerini gerçekleştirmeden önce yetkili kişi tarafından, doğru veriye erişimi olan kişi tarafından yapıldığı kontrol ediliyor mu? 
* Veri bütünlüğünü sağlamak adına SQL sorgularında `BEGIN TRANSACTION` - `COMMIT TRANSACTION` lar doğru şekilde ilgili her yerde kullanılıyor mu? 
* Projelerde sonsuz döngü oluşturabilecek yapılardan kaçınılıyor mu? Veya döngüler kullanılsa bile sonsuz döngü kitlenmesine kesin olarak engel olacak mekanizmalar uygulanıyor mu? 
* Projelerin konfigurasyon dosyası, web e erişimi olmayan yetkisiz erişimlere açık olmayan, güvenli bir path de tutuluyor mu? 
* Yazılımlarda API Keyler, bağlantı şifreleri, connection string vb. gibi hassas bilgiler, güvenli bir şekilde mi tutuluyor? 
* Web projelerinde, beklenmedik hata durumlarında custom errors mekanizması oluşturuldu ve bir hata durumunda hata alan kullanıcının ekranında hatanın gerçek içeriğinin gözükmemesi sağlanarak, yazılım açığının ve hatanın expose olmaması sağlandı mı? 
* Projelerde global exception handling, ve error handling kısımları doğru bir şekilde kodlanıyor mu? 
* Web, uygulama ve veritabanı sunucularının sistem bileşenleri hakkındaki kritik bilgiler (sunucu adı ve sürümü, kullanılan program sürümü v.b.) gizleniyor mu? 
* Uygulamada oluşan hatalar ve uygulama sunucusu ön tanımlı hata mesajları kullanıcıya detaylı olarak gösterilmemesi sağlanıyor mu? 
* Uygulama üzerinden yapılan kritik işlemler hem uygulama seviyesinde hem de sunucu seviyesinde kayıt altına alınıyor mu? 
* Race-Condition'lara engel olmak için kritik kaynaklara, objelere, metotlara senkronizasyon sağlanarak eş zamanlı erişime izin verilmemelidir. 
* Parola güncelleme vb. kritik işlemlerde her zaman eski parolayı sorma veya OTP doğrulama vb. gibi gerekli önlemler alınarak gerçekleştirilmelidir. 
* Parola unuttum formları, gizli soru ve benzeri ek argümanlarla desteklenmelidir. 
* Parola unuttum işlemlerinden sonra kullanıcıya gönderilen eposta, kullanıcı adı ve parola bilgisi içermemelidir. Bunun yerine sınırlı yaşam süresi bulunan bir link gönderilip, o link üzerinden açılan sayfadan parola değiştirme işlemi gerçekleştirilmelidir. 
* Uygulama domain isimlerine ait hassas bilgilerin google/bing gibi arama motorları tarafından indekslenmediği kontrol edilmelidir 

AYRICA WEB GÜVENLİKLİĞİYLE İLGİLİ KONULARDA DÜZENLİ OLARAK `OWASP (Open Web Application Security Project)` TAKIP EDILMELIDIR. 
https://owasp.org/


 
