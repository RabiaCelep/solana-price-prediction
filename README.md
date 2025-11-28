# Solana Price Prediction

 **Introduction**  
Bu proje, Makine Öğrenmesine Giriş dersi için hazırladığım bir ödev çalışmasının sonucu olarak ortaya çıktı.  
Amacım, Solana blockchain’ine ait tarihsel verileri inceleyerek bir makine öğrenmesi modeli seçmek, modeli eğitmek ve bu veriler üzerinden temel tahminler yapmaktı.  

İlk adım olarak **Lineer Regresyon** yöntemini tercih ettim. Çünkü lineer regresyon, değişkenler arasındaki doğrusal ilişkileri anlamak için kullanılan en temel yöntemlerden biridir ve kripto fiyat tahminleri gibi karmaşık bir konuda bile işe başlangıç için oldukça iyi bir zemin sağlıyor.  
Bu proje, hem veri hazırlama hem de temel modelleme süreçlerini deneyimlememi sağladı.

---

##  Dataset Açıklaması

Kullandığım veri seti, Solana blockchain’ine ait hem **zincir içi (on-chain) verileri** hem de **SOL/USD fiyat geçmişini** içeriyor. Veri oldukça geniş kapsamlı olduğundan, zaman serisi analizi ve korelasyon incelemeleri için zengin bir ortam sunuyor.  

### Veri setinde bulunan başlıca bilgiler:

** On-chain veriler**  
- Blok sayıları  
- İşlemler  
- Cüzdan hareketleri  
- Program/contract çağrıları  

** Piyasa verileri**  
- Açılış, kapanış, en yüksek ve en düşük SOL/USD fiyatları  
- İşlem hacmi  
- Tarih aralığı: 2020 – 2024  

> Bazı metrikleri kullanarak SOL fiyatı lineer olarak tahmin edilebilir mi?

---

##  Problem Tanımı

Proje boyunca şu sorulara cevap aradım:  
- SOL fiyatı zaman içinde nasıl değişiyor?  
- Hangi özellikler (hacim, zaman vb.) fiyatla ilişkili olabilir?  
- Lineer regresyon fiyat tahmini için ne kadar başarılı?  
- Korelasyon analizi bize ne söylüyor?  

---

##  Veri Hazırlık Süreci

Notebook üzerinde sırasıyla şu adımları gerçekleştirdim:  

- Veri setini yükledim.  
- Eksik verileri inceledim ve gerekli temizlikleri yaptım.  
- Tarih kolonlarını uygun zaman formatına dönüştürdüm.  
- Ek özellikler üretmek için `DayOfYear` gibi yeni kolonlar oluşturdum.  
- Hedef değişken olarak **Close (kapanış fiyatı)** seçildi.  
- Zaman, hacim gibi metrikler giriş değişkeni (X) olarak belirlendi.  
- Scatter plot’lar ile değişkenlerin birbirleriyle ilişkilerini görselleştirdim.  

---

##  Lineer Regresyonun Kısa Özeti

Lineer regresyon, iki değişken arasında doğrusal bir ilişki olup olmadığını anlamak için kullanılan en temel yöntemlerden biridir.  

Matematiksel formu:  

Y = a + bX

less
Kodu kopyala

- **X**: açıklayıcı değişken  
- **Y**: tahmin edilen değişken  
- **b**: eğim  
- **a**: kesişim  

Model, tahmin hatalarını minimize etmek için **en küçük kareler yöntemi** kullanılarak eğitilir.

---

##  Korelasyon Analizi
Modeli eğitmeden önce değişkenler arasındaki ilişkinin gücünü inceledim.  

**Genel gözlemler:**  
- Zaman ile SOL fiyatı arasında belirgin bir ilişki yoktu  
- Hacim (Volume) ile fiyat arasındaki ilişki zaman kadar zayıf olmasa da düşük seviyedeydi  
- Kripto para piyasaları yüksek volatiliteye sahip olduğundan doğrusal bir ilişki beklemek çoğu zaman gerçekçi değil  

Bu nedenle **lineer regresyon**, proje için temel bir başlangıç modeli olarak seçildi.

---

##  Modelin Eğitilmesi
Veri seti eğitim ve test olarak ayrıldı ve lineer regresyon modeli bu veriler üzerinde eğitildi.  

**Ardından yapılan işlemler:**  
- Model katsayıları incelendi  
- Tahminler alındı  
- Hata metrikleri (MSE) hesaplandı  
- R² skoru değerlendirildi  

---

##  Sonuçlar ve Değerlendirme
- Lineer regresyon, kripto fiyatlarını tahmin etmede sınırlı başarı gösterdi  
- Zaman → fiyat ilişkisi düşük korelasyona sahipti  
- Hacim gibi değişkenler kullanılsa bile tahmin doğruluğu sınırlı kaldı  
- MSE orta seviyede, R² ise düşük çıktı  

**Genel Sonuç:**  
Lineer regresyon, bu veri seti için güçlü bir model olmasa da, giriş seviyesinde keşif ve anlayış için **ideal bir araçtır**.

---

##  Lineer Regresyonun Diğer Modellerle Karşılaştırılması

| Model                   | Doğruluk        | Eğitim Süresi | Yorumu Kolay mı? | Zaman Serisine Uygunluk |
|-------------------------|----------------|---------------|-----------------|------------------------|
| Lineer Regresyon        |  Düşük       |  Çok Hızlı   |  Evet          |  Zayıf               |
| Random Forest / XGBoost |  Orta–Yüksek  |  Orta        |  Zor           |  Orta                 |
| LSTM (Zaman Serisi)     |  Çok Yüksek  |  Uzun        |  Zor           |  En Uygun             |

---

##  Genel Değerlendirme
Bu proje sayesinde:  
- Veri temizleme  
- Görselleştirme  
- Korelasyon analizi  
- Regresyon modeli eğitme  
- Tahmin ve hata analizi  

konularında tecrübe kazandım.

Solana blockchain verileri oldukça **dalgalı ve karmaşık** olduğundan, lineer regresyon sınırlı bir başarı sağladı.  
Ancak proje, makine öğrenmesi sürecini anlamak ve daha gelişmiş modellere hazırlanmak için **çok iyi bir başlangıç oldu**.
