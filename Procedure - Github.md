## Github Kullanım Prosedürü

* Github repolarında `bin`, `obj`, `packages`, `npm paketleri`, `.idea`, `.vs` vb. gibi klasör ve dosyaların olmaması gerekiyor, repolarda sadece build edilebilir kod bulunmalıdır, kişisel proje dosyalarınız vb. olmamalıdır, `.gitignore` dosyalarınızın buna göre oluşturulduğundan emin olunuz.

* Repolar üzerinde geliştirme yaparken, repo ile alakalı `kurulum`, `amaç`, `nasıl çalışır`, `nasıl kullanılır` vb. bilgileri `README.md` dosyalarına yazılmalı, developer ve müşterilerin bilmesi gereken _önemli bilgiler_ reponun `wiki` bölümüne **dokümante** edilmelidir.
* Github üzerine `commit` `push` ederken, yapılan değişiklikleri doğru bir şekilde anlatacak formatta **başlık** ve **mesaj içeriği** girilmelidir. Başlık boş bırakılmamalıdır.
* Github üzerine `issue` açarken, yapılacak işi doğru ve tam olarak anlatan bir **başlık** ve varsa detayı girilmeli, ilgili `proje` ve `label` bilgileri seçilmelidir.
* Geliştirmeler `dev` branchi üzerinden yapılmalıdır.Eğer birden fazla kişi proje üzerinde çalışıyorsa, **feature bazlı branchler** açılıp, feature kodlaması bitince `dev` branchine `merge` edilmesi `conflict`lerin yönetilmesinde fayda sağlayacaktır.
* Geliştirme tamamlandığında, `dev` branchinden `test` branchine `pull request` açılmalıdır. Pull request açıldığında **code reviewer** ve **assignee** alanları seçilmeli, yapılan geliştirmeler liste halinde pull request **detayına** yazılmalıdır.
* Code reviewler **standartlarımıza** göre yapılacaktır, `figensoft` reposu wikisinden bu standartlara ulaşabilirsiniz.
* Code reviewda eğer bulgu bulunur ise bu maddelerle alakalı düzenlemeler yapılıp, bulgu kapatıldı (`resolve conversation`) butonu ile işaretlenmelidir. Eğer bulgu ile alakalı sorunuz olur ise, bulgu altına `comment` bırakabilir veya ilgili code review eden kişi ile iletişime geçebilirsiniz.
* Code review sonrası kod bulgusuz hale getirildiğinde, `test` branchine merge edilip, geliştirici **testlerini** gerçekleştirmelidir.
* Testler sonrası bulgu çıkmaz ise, `test` branchin den **deployment** için `prod` branchine `pull request` açılmalıdır. Pull request title ında **versiyon** (vX.X.X -> v1.0.2 gibi) yazılmalı, açıklama kısmına da maddeler halinde bu versiyonda yapılan **değişiklikler** yazılmalıdır.
* Repolar üzerinden `release` oluştururken, `prod` releaseleri için `tag` ve `release` ismi `vX.X.X` formatında **versiyon** olarak isimlendirilmeli ve referans branch olarak `prod` branchi seçilmelidir, `test` release i oluşturuluyor ise release ismi `vX.X.X-test` olarak isimlendirilip test branchi referans branch olarak seçilmeli ve `pre-relase` butonu kullanılarak işaretlenmelidir, `dev` üzerinde release oluşturuluyor ise release ismi `vX.X.X-dev` olarak isimlendirilip dev branchi referans branch olarak seçilmeli ve `pre-relase` butonu kullanılarak işaretlenmelidir. Tüm release dosyalarında sadece **sunucuya atılacak halde build edilmiş** dosyalar **tek bir zip dosyası** olarak upload edilmelidir.


## Haftalık Code Review

Her hafta `Cuma` günü saat `11:00` da GitHub üzerinde çalıştığınız projeler için `test` brachlerine `Haftalık Code Review` pull requestleri açılmalıdır. 
