# Coding Standard

## 0) Başlangıç 
Bu dökümanı okumaya başlamadan önce; 

* SOLID, 
* KISS (Keep It Simple Stupid), 
* DRY (Don’t Repeat Yourself), 
* YAGNI (You Aren't Gonna Need It) 

prensiplerini araştırınız.
 

## 1) Kullanılacak Dil 
Aksi belirtilmedikçe kodlama dili olarak ingilizce kullanılmalıdır, 
issuelar ve commitler ise okuyan kişinin rahatça anlayabileceği açıklıkta türkçe olmalıdır.


## 2) İsimlendirmeler

**2.1 Genel İfadeler** 

* Değişkenler fonksiyon ve sınıf isimlendirmeleri, yapılan işi veya sınıfı tam olarak ifade edecek uygunlukta ve basit anlaşılır olacak şekilde seçilmelidir.  
Değişkenler ve fonksiyonlar için kolayca telaffuz edilebilen isimler kullanın. Değişken ve fonksiyon adlarında kısaltma kullanmayın. Değişken adını tam olarak kullanın, böylece kolayca telaffuz edilebilir ve herkes onu anlayabilir.

```cs
// Kötü isimlendirme
public int msgcnt { get; set; }
public string cmt { get; set; }
```

```cs
// Doğru isimlendirme
public int MessageCount { get; set; }
public string Comment { get; set; }
```

* İsimlendirmelerde kelime tekrarından kaçınılmalıdır. <br />

```cs
//Tekrarlı isimlendirme
User.CreateUser();
User.UserCreate();
User.UserLogin();

User.UserName;
User.UserSurname;
```

```cs
//Doğru isimlendirme
User.Create();
User.Login();

User.Name;
User.Surname;
```

**2.2 Tutarlılık**

* Metod isimlerinde tutarlı olun, benzer işlevler için bir kelime kullanın. Örneğin, Bir sınıfta "get", başka bir sınıfta aynı işlev için "fetch" kullanmayın. 

**2.3 Yazım Stili**

Değişken, sınıf, fonksiyon vb. isimlendirmeleri kullandığınız programlama dilinin standartına uygun yazınız. 

C# için;
* Sınıf ve Enum, Interface isimlendirmeleri **CamelCase** dir
* Sınıf elemanları **CamelCase** dir
* Fonksiyon isimlendirmeleri **CamelCase** dir
* Lokal değişken ve parametre isimlendirmeleri **lowerCamelCase** dir
* Enum elemanlarının isimlendirmeleri **ALL_UPPER_CASE** dir

Java, Javascript, PHP için;
* Sınıf ve Enum, Interface isimlendirmeleri **CamelCase** dir
* Sınıf elemanları **CamelCase** dir 
* Fonksiyon isimlendirmeleri **lowerCamelCase** dir
* Lokal değişken ve parametre isimlendirmeleri **lowerCamelCase** dir
* Enum elemanlarının isimlendirmeleri **ALL_UPPER_CASE** dir

CSS için;
* Class ve ID isimlendirmeleri **css-class-name** şeklinde hepsi küçük harf ve kelime aralarında tire olacak şekilde olmalıdır. (internetten CSS ABEM standartına göz atabilirsiniz)

CSS sınıf isimlendirmeleri `hypen-seperated-lowercase` şeklinde olmalıdır. Örneğin `.list`, `.list-container` vb.

SQL için kodlama standartı [dokümanına](https://github.com/figensoft/figensoft/blob/dev/Procedure%20-%20Coding%20(SQL).md) göz atınız.

## 3) Fonksiyonlar 

* Fonksiyonlar olabildiğince kısa ve öz olmalıdır. Single Repsonsibility prensibinde olduğu gibi her fonksiyon tek bir işden sorumlu olmalıdır ve sadece o işi yerine getirmelidir. 
* Fonksiyonlar ya bir şeyi cevaplamalı ya da bir şey yapmalı, ancak ikisini birden yapmamalıdır.
* Fonksiyonlara boolean değişken geçmekten kaçının. Neden bir boolean geçmeniz gerekiyor? Bir fonksiyon içinde birden fazla şey mi yapmanız gerekiyor? (Single Responsibility’e aykırı)
* Bir fonksiyon içerisinde handle edilmemiş case olmamalıdır. null kontrolleri, try catch kullanımı vb. doğru şekilde tüm senaryoları karşılayacak şekilde kodlanmalıdır.
* Değişkenler ihtiyaç olduğu en yakın kapsam kod bloğu içerisinde tanımlanmalı yani bir yerde ihtiyaç duyulan değişken lokal tanımlanmalı eğer gerekli değil ise sınıf üyesi yapılmamalı 

## 4) Spagetti kod 
* Spagetti kod kullanımından kaçınınız, iç içe ifler vb. yapılar kodu karmaşıklaştırır. Yazdığınız kodlar yukarıdan aşağıya satır satır okunabilecek şekilde olmalıdır, kodu okuyan kişi kodu anlamak için bir yukarı bir aşağı bakmak zorunda olmamalı, grift if-else blockları gibi şeyler bulunmamalıdır. bunun önüne geçmek için aşağıdaki şekilde bir yöntem uygulayınız.


```cs
//Eğer aşağıdaki şekilde bir if else kullanımı yaparsanız, kod okunamaz hale gelir
public bool IsResponseValid(HttpResponseMessage response)
{
    if (response != null)
    {
         if (response.StatusCode == System.Net.HttpStatusCode.OK) 
         {
              if (response.Content != null)
              {
                    if (string.IsNullOrEmpty(response.Content.ReadAsStringAsync().Result))
                    {
                       return false;
                    }
                    else
                    {
                       return true;
                    }
              } 
              else
              {
                    return false;
              }
         }
         else
         {
              return false;
         }
    } 
    else
    {
         return false;
    }
}
```


```cs
//Doğru kodlama
//Tüm senaryolar için kontrollerimizi en baştan yaparak hem spagetti kodun önüne geçmiş hem de satır satır okunabilirliği sağladık. //Yukarıdaki spagetti kod ile arasındaki farkı görebiliyor musunuz?
public bool IsResponseValid(HttpResponseMessage response)
{
    if (response == null)
        return false;

    if (response.StatusCode != System.Net.HttpStatusCode.OK)
        return false;

    if (response.Content == null)
        return false;

    if (string.IsNullOrEmpty(response.Content.ReadAsStringAsync().Result))
        return false;

    return true;
}
```
 
## 5) Mükerrer Kod 
* Kodu tekrar etmeyin. Ne zaman bir fonksiyon yazsanız, kendinize benzer bir şeyin daha önce yapılıp yapılmadığını sorun. Sürekli tekrar eden kod blocklarınız var ise o blockları fonksiyon haline getirmek mantıklı olabilir.
 
## 6) Single Repsonsibility 
* Yine Single Repsonsibility prensibine göre bir sınıf bir iş yapmalıdır. Örneğin sınıf kullanıcı içinse, tüm fonksiyonları ve özellikleri tamamen kullanıcı ile alakalı olacak şekilde yazılmalıdır. 

## 7) Yarı Değişkenler 
* Uygulama içerisindeki Connection String, bağlantı kurduğu api urlleri, kullanıcı adı ve şifreler gibi değişkenler için kesinlikle hard code kodlama yapmayınız!! Değerler hard codelamak yerine config den veya dbden okunmalıdır.
* Değişken, sabit veya config kullanmak kodu sadece okunabilir hale getirmekle kalmaz, aynı zamanda birden çok yerde kullanılıyorsa değiştirilmesini de kolaylaştırır. 


