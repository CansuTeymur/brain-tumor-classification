# **Brain Tumor Classification**

Bu proje, MRI görüntülerini kullanarak **beyin tümörlerini sınıflandırmak** amacıyla geliştirilmiştir.  
Model, derin öğrenme teknikleri kullanılarak dört farklı sınıfı ayırt edebilecek şekilde tasarlanmıştır:

- **Glioma**
- **Meningioma**
- **Pituitary**
- **No Tumor (Tümör Yok)**

---

## **1. Projenin Amacı**
Bu projenin temel amacı, MRI beyin görüntülerinden **otomatik tümör tespiti ve sınıflandırma** yapmaktır.  

---

## **2. Veri Seti Hakkında**
Projede kullanılan veri seti Kaggle üzerinden alınmıştır.  
Veri seti, MRI beyin görüntülerinden oluşmakta ve **4 farklı sınıf** içermektedir.


**Training Set Görsel Sayıları:**
- **Pituitary:** 1457  
- **No Tumor:** 1595  
- **Meningioma:** 1339  
- **Glioma:** 1321  
- **Toplam:** **5712**

**Testing Set Görsel Sayıları:**
- **Pituitary:** 300  
- **No Tumor:** 405  
- **Meningioma:** 306  
- **Glioma:** 300  
- **Toplam:** **1311**

**Kaynak:**  
[Kaggle Brain Tumor MRI Dataset](https://www.kaggle.com/datasets)

---

## **3. Kullanılan Yöntemler**

Bu projede beyin tümörü sınıflandırması için aşağıdaki yöntemler kullanılmıştır:

**1. Temel CNN Modeli**
Giriş verisi: 150×150×3 boyutlu görüntüler.
Üç adet Conv2D + MaxPooling2D bloğu (filtre sayıları: 32 → 64 → 128, kernel: 3×3).
Flatten → Dense (128, ReLU) → Dropout (0.5) → Dense (4, Softmax) yapısı ile dört sınıf tahmini.

**2. Kayıp Fonksiyonu ve Optimizasyon**
- Kayıp fonksiyonu: categorical_crossentropy
- Optimizasyon: Adam
- ReduceLROnPlateau ile öğrenme oranı dinamik olarak azaltılmıştır.
- EarlyStopping ile en iyi ağırlıklar korunmuştur.

**3. Sınıf Ağırlıkları (Class Weights)**
- compute_class_weight ile sınıf dengesizliği giderilmiştir.
- Az örneğe sahip sınıflara daha fazla ağırlık verilmiştir.

**4. Transfer Learning (VGG16)**
- VGG16 önceden eğitilmiş model kullanılmıştır (include_top=False).
- Üzerine GlobalAveragePooling, Dense ve Dropout katmanları eklenerek dört sınıf tahmini yapılmıştır.
- Daha iyi genelleme için sınırlı veriyle yüksek performans sağlanmıştır.

**5. Veri Artırımı (Data Augmentation)**
- ImageDataGenerator ile dönüşümler uygulanmıştır:
- Döndürme, kaydırma, zoom, yatay çevirme.
- Overfitting’i azaltıp modelin genelleme yeteneğini artırmıştır.

---

## **4. Elde Edilen Sonuçlar**

Modelin performans karşılaştırması:

| Metrik            | CNN Model | Transfer Learning |
| ----------------- | --------- | ----------------- |
| **Test Accuracy** | 0.7391    | **0.8337**        |

Genel Accuracy (CNN): %74

> **Yorum:**  
> Transfer Learning ile model doğruluğu önemli ölçüde artmıştır.  
> Özellikle `Meningioma` ve `Glioma` sınıflarında yüksek performans elde edilmiştir.  

---

## **5. Kaggle Notebook Linki**
Projeye ait Kaggle notebook’unu aşağıdaki bağlantıdan inceleyebilirsiniz:  

🔗 [Brain Tumor Classification - Kaggle Notebook](https://www.kaggle.com/code/cansuteymur/brain-tumor-classification)

---

## **6. Dosya Yapısı**
GitHub reposu şu şekilde düzenlenmiştir:

brain-tumor-classification/
│
├── brain_tumor_classification.ipynb     # Projenin ana notebook dosyası
└── README.md                            # Proje açıklamaları ve kullanım bilgileri


## **7. Sonuç**   

Bu proje, MRI görüntülerinden beyin tümörlerini sınıflandırmada derin öğrenme modellerinin ne kadar güçlü olduğunu göstermektedir.
Transfer Learning sayesinde doğruluk oranı artırılmış ve daha genel bir model elde edilmiştir.
