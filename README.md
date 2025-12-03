#  Solana Price Prediction – Lineer Regresyon ile Fiyat Tahmini

Bu proje, Makine Öğrenmesine Giriş dersi için hazırladığım kapsamlı bir çalışma olup Solana’nın (SOL) geçmiş fiyat verilerini analiz ederek temel bir tahmin modeli oluşturmayı amaçlıyor. Amacım; veri hazırlama, özellik mühendisliği, model eğitimi ve değerlendirme süreçlerini baştan sona deneyimlemek ve başlangıç seviyesi bir makine öğrenmesi modeliyle kripto piyasası hakkında bir fikir üretmenin ne kadar mümkün olduğunu görmekti.

---

##  Projenin Amacı

Kripto para piyasaları çok değişken olduğu için fiyat tahmini oldukça zor bir problem. Bu projede şu sorulara cevap aradım:

- **SOL fiyatı zaman içinde nasıl değişiyor?**
- **Zaman, hacim ve piyasa değeri gibi metrikler fiyata ne kadar etki ediyor?**
- **Lineer regresyon gibi temel bir model bu karmaşık yapıda anlamlı tahminler üretebilir mi?**
- **Farklı model türleriyle kıyaslandığında lineer regresyonun avantajı ve sınırları neler?**

Bu sorulara cevap verebilmek için Solana’nın hem zincir içi hem de piyasa verilerinden oluşan geniş bir veri seti üzerinde çalıştım.

---

##  Dataset Açıklaması

Projede kullanılan veri seti SOL/USD geçmiş fiyatları ve piyasa verilerini içeriyor. Veri setinde bulunan başlıca alanlar:

###  Piyasa verileri
- Açılış – kapanış fiyatları
- Günün en yüksek ve en düşük fiyatları
- Toplam işlem hacmi (total_volume)
- Piyasa değeri (market_cap)
- Tarih bilgisi (snapped_at)

###  Tarih yapısından türetilen yeni özellikler

Veriyi daha zengin hale getirmek için zaman bilgisinden yeni kolonlar ürettim:

| Yeni Kolon   | Açıklama           |
|--------------|--------------------|
| DayOfYear    | Yılın kaçıncı günü |
| Month        | Ay bilgisi         |
| Year         | Yıl                |
| WeekOfYear   | ISO haftası        |

Bu dönemsel değişkenlerin fiyatı etkileyip etkilemediğini gözlemlemek istedim.

---

##  Veri Hazırlama Süreci

Projede izlediğim veri hazırlama adımları:

- CSV dosyasını yükledim.
- Tarih kolonunu datetime formatına çevirdim.
- Veri setinden gerekli tarihsel özellikleri türettim.
- Bazı kolonlarda eksik değer olup olmadığını kontrol ettim.
- Hedef değişken olarak **price** kolonunu seçtim.

Model giriş verilerini (**features**) belirledim. Bu değişkenleri seçme nedenim:

- **market_cap ve total_volume:** fiyatla ilişkisi literatürde sıkça incelenen metrikler.
- **DayOfYear, Month, Year, WeekOfYear:** zaman bazlı tekrar olup olmadığını test etmek için.

---

##  Neden Lineer Regresyon?

Bu projede başlangıç modeli olarak lineer regresyon kullandım çünkü:

- Uygulaması hızlı ve anlaşılması kolay.
- Katsayılar yorumlanabilir, yani modelin nasıl karar verdiğini görebiliyoruz.
- Veri setinin ilişkilerini temel düzeyde keşfetmek için ideal bir giriş modeli.

Tabii ki kripto gibi değişken  bir piyasada çok yüksek doğruluk beklemek gerçekçi değil, ancak amaç ilk adımı oluşturmak ve modelleme mantığını oturtmaktı.

---

##  Modelin Eğitilmesi

Modeli eğitmek için veriyi **%80 eğitim – %20 test** olacak şekilde ayırdım. Ardından aşağıdaki adımları takip ettim:

- Lineer regresyon modeli kurdum.
- Eğitim verisiyle modeli uygun hale getirdim.
- Test verisi üzerinden tahminler aldım.
- MSE ve R² metrikleri hesapladım.

Özetle model şu parametrelerle eğittim:

- **Model:** LinearRegression()
- **Hedef:** price
- **Feature sayısı:** 6
- **Veri dönemi:** 2020–2024

---

##  Görselleştirmeler

### 1️ Gerçek vs Tahmin Scatter Plot

Bu grafik, model tahminlerinin gerçek fiyatlarla ne kadar uyumlu olduğunu gösteriyor.  
Dağılımın dağınık olması, doğrusal ilişkinin zayıflığını destekliyor.

### 2️ Katsayı Grafiği (Feature Importance)

Tüm feature’ları normalize ettikten sonra katsayıları çizerek hangi değişkenin modele daha fazla katkı sağladığını görselleştirdim.

Bu analiz sayesinde:

- **market_cap en etkili değişkenlerden biri,**
- **zaman bazlı değişkenlerin etkisi ise oldukça sınırlı**

olduğu ortaya çıktı.

---

##  Model Performansı

Elde edilen temel performans ölçümleri:

- **MSE:** Ortalama kare hata  
- **R²:** Modelin varyansı açıklama oranı  

R² skorunun düşük çıkması, fiyatların doğrusal bir modelle iyi açıklanamadığını gösteriyor.  
Kripto piyasasının yüksek volatilitesi düşünüldüğünde bu sonuç beklenen bir durum.

---

##  Lineer Regresyonu Diğer Modellerle Karşılaştırma

Aşağıdaki tablo, lineer regresyonun diğer popüler modellerle karşılaştırmasını gösteriyor:

| Model            | Doğruluk | Eğitim Süresi | Yorumlanabilirlik | Zaman Serisi Uygunluğu | Açıklama |
|------------------|----------|---------------|--------------------|-------------------------|----------|
| Lineer Regresyon | Düşük    | Çok Hızlı     | Kolay              | Zayıf                   | Temel model. Karmaşık piyasalarda doğrusal ilişki zayıf kalıyor. |
| Random Forest    | Orta–Yüksek | Orta        | Orta              | Orta                    | Doğrusal olmayan ilişkileri yakalar, ama yorumlaması daha zor. |
| XGBoost          | Yüksek   | Orta          | Zor                | Orta                    | Karmaşık veri ilişkilerini daha iyi modeller, genelde daha iyi tahmin verir. |
| LSTM (RNN)       | Çok Yüksek | Uzun        | Zor               | En Uygun                | Zaman serisi bağımlılıklarını öğrenebilir, kripto gibi dalgalı verilerde oldukça etkili. |

Bu tabloyu hazırlamamın sebebi, projenin yalnızca lineer regresyonla sınırlı olmadığını, model seçerken karşılaştırma yapmayı öğrendiğimi göstermek içindir.

---

##  Sonuç ve Değerlendirme

Bu proje boyunca:

- Veri setini tanıma  
- Veri temizleme ve özellik mühendisliği  
- Zaman bazlı özellik üretme  
- Temel bir modeli eğitme  
- Performans analizleri  
- Görselleştirme teknikleri  
- Model karşılaştırma mantığı  

konularında önemli deneyim kazandım.

Lineer regresyon, bu veri seti için yüksek performans göstermese de **neden yüksek performans göstermediğini anlamak**, bu projenin en değerli kısmı oldu.  
Kripto piyasası doğrusal olmayan, dalgalı ve karmaşık bir yapıya sahip. Bu yüzden daha gelişmiş zaman serisi modellerine geçmek ilerideki en mantıklı adım.

---
