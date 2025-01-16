# Versiyon Kontrolü



## Semantic Versioning

Yapılan değişikliğe göre versiyonumuzu değiştiririz.

Semantic Versioning, yazılım projelerinde versiyon numaralarını sistematik bir şekilde artırmak için kullanılan bir standarttır. Üç ana bileşenden oluşur:

**x.y.z**

1. x (Major Version):

    -Geriye dönük uyumluluğu bozan değişiklikler yapıldığında artırılır.

    -Örnek: API tamamen değiştiğinde veya yazılımın temel yapısında büyük bir değişiklik olduğunda.

    -Örnek: 2.0.0
2. y (Minor Version):

    - Geriye dönük uyumluluğu koruyan yeni özellikler eklendiğinde artırılır.

    - Örnek: Yeni bir modül eklendiğinde, ancak mevcut işlevsellik etkilenmediğinde.

    - Örnek: 2.1.0
3. z (Patch):

    - Küçük düzeltmeler yapıldığında artırılır. Bu düzeltmeler, hata giderme veya performans iyileştirmesi gibi 
    değişikliklerdir.

    - Örnek: Bir hata düzeltildiğinde.

    - Örnek: 2.1.1

### Özel Sürüm Etiketleri (Pre-release ve Build Metadata):

**Pre-release:** Bir sürümün tamamlanmamış veya test aşamasında olduğunu gösterir. Genellikle alpha, beta, rc gibi etiketler kullanılır.

Örnek: 1.0.0-alpha, 1.0.0-beta, 1.0.0-rc.1

**Build Metadata:** Dağıtımdan sonra bir sürüme özgü bilgileri içerir.

Örnek: 1.0.0+build12345

# Git

Git bir versiyon kontrol aracıdır.

```bash
git init #Boş bir git deposu oluşturduk.
```
projemizde gizli bir git dosyası oluşur.

```bash
git log #Kaydettiğimiz değişiklikleri görebiliriz.

git status #Durumumuzu gösterecek.

git add "dosyadı" #Bir dosyamızı staging areaya gönderiyoruz.

git commit -m "Commit açıklaması burda yazılması gereklidir." #Eklenen değişikliklerin ne olduğu ve nasıl yapıldığı ile ilgili bir açıklama gibi düşünülebilir.Commitler ingilizce olmalı.

git config --global user.name "İsim" #Bununla beraber ismimizi değiştirebiliriz.

```

Eğer versiyonlar arasında gezinmek eskiye gitmek istiyorsak Git ile bunu kolayca yapabiliriz. 

```bash

git checkout "commitid" #Girdiğimiz commitid ile beraber kaydedilen o versiyona geri dönebiliriz. 
```
```bash
git remote add origin "github Repository urlsi" #Origin diye bir sunucu var ve yeri bu diye belirtiyoruz. 
```
```bash
git push -u origin main #Main branchinde yapılan değişiklikleri Githuba gönderiyoruz.
```
Projemizde uzun soluklu değişimler veya farklı yenilikler denemek istiyorsak farklı bir branch üstünde çalışabiliriz.Branch ismimiz yapılacak yenilikle ilgili olmalı.
```bash
git branch "branch ismi" #Bir branch oluşturulur

git checkout "brancismi" #Brenchler arasında bu komut ile geçişler yapılabilir.En son commite gider.

git push -u origin "branchismi" #Yaptığımız değişiklikleri Githuba gönderiyoruz. 
```

Her dosyanın değişikliğinin izlenmesini istemeyebiliriz. Bu sebeple gitignore kullanmalıyız.
Projemize .gitignore sayfasını ekleyerek izlenmesini istemediğimiz dosyaların adını girmemiz yeterli olacaktır.


**Commitlerimiz çalışan en küçük parça şeklinde olmalı**


```bash
git branch "branch ismi" #Bir branch oluşturulur

git checkout "brancismi" #Brenchler arasında bu komut ile geçişler yapılabilir.En son commite gider.

git push -u origin "branchismi" #Yaptığımız değişiklikleri Githuba gönderiyoruz. 
```
Bir branch ile main branchini birleştirme işlemine **Merge** denir. Bu işlemi komut satırı üzerinden de yapabiliriz github üzerinden de yapabiliriz.Github üzerinde yaptığımızı varsayarsak artık sunucuda bir değişiklik var fakat bilgisayarda bu değişiklik olmadı.Bunun için de main branchi üstünde aşağıdaki komutu yazmamız gereklidir.
```bash
 git pull #Artık sunucu ile bilgisayar aynı duruma gelmiş oldu.
 git merge main # başka bir branchdeyken bu komutu çalıştırmak şu demek; main branchini benim branchime getir. 
```
Merge işlemi yaparken çeşitli çatışmalar olabilir. Vscode üzerinde çakışmaların neler olduğu gözüküyor ve bu çakışmaları tamamen düzeltmemiz gerekiyor ki merge işlemi yapılabilsin.

## Git Pull ve Merge Senaryosu
Bir ekip **main** branch'inde çalışıyor.  
Siz, yeni bir özellik üzerinde çalışmak için bir **feature** branch'i oluşturdunuz ve bu branch'te bazı değişiklikler yaptınız. Daha sonra bu değişiklikleri **main** branch'ine eklemek istiyorsunuz.
### **Adım 1: Feature branch'ini oluşturup üzerine çalışın**
1. **Yeni bir branch oluşturun ve geçiş yapın:**
```bash
git branch feature
git checkout feature

```
2. **Kod değişikliklerinizi yapın ve commit oluşturun:**
```bash
git add .
git commit -m "New feature added."
```
### **Adım 2: Main branch'ine geçiş yapın**
1. **Feature branch'inden main branch'ine geçiş yapın:**

```bash
git checkout main

```
2. **Main branch'i güncelleyin (eğer gerekliyse):**

```bash
git pull origin main

```
### **Adım 3: Main branch'ine feature branch'ini merge edin**
1. **Feature branch'ini main branch'ine birleştirin:**
```bash
git merge feature

```
2. **Merge sırasında çakışma varsa (conflict), çakışmaları çözün:**
- Çakışma olan dosyaları düzenleyin.
- Düzenlemeyi kaydettikten sonra aşağıdaki komutları çalıştırın:
```bash
git add <dosya_adi>
git commit -m "Conflicts resolved and merge completed."

```
### **Adım 4: Değişiklikleri uzak depoya gönderin**
1. **Main branch'teki güncellemeleri uzak depoya gönderin:**

```bash
git push origin main

```
**`Feature branch'inde yaptığınız değişiklikler artık main branch'e başarıyla eklenmiş oldu.
İsterseniz feature branch'ini silebilirsiniz:`**

### Github Environment

GitHub Environment , bir depo içindeki belirli çalışma ortamlarını temsil eder. Bu özellik, bir uygulamanın veya sistemin farklı aşamalarını (örneğin: geliştirme, test, üretim) düzenlemek ve yönetmek için kullanılır. GitHub Environment'lar, özellikle GitHub Actions ile otomasyon süreçlerinde kullanılır ve bu süreçlerde ortamlar için kurallar, güvenlik kısıtlamaları ve gizli anahtarlar tanımlanabilir.
#### Ortamlar genellikle şu şekilde adlandırılır:
- **development:** Geliştirme ortamı
- **staging:** Test ortamı
- **production:** Üretim ortamı
