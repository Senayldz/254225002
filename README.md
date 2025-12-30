# Vision Transformer (ViT) ile Akne Şiddeti Sınıflandırması

Bu proje, yüz görüntülerinden akne şiddetini (**Level 0, 1, 2**) Vision Transformer (ViT) mimarisi kullanarak sınıflandıran akademik bir derin öğrenme çalışmasıdır. Projede standart denetimli öğrenme ve **Öz-denetimli Öğrenme (Self-Supervised Learning - SSL)** yaklaşımları karşılaştırmalı olarak analiz edilmiştir.

---

## 1. Problemin Tanımı
Klinik akne teşhisinde uzmanlara karar destek sistemi sağlamak amacıyla geliştirilmiştir.  
**Temel Zorluk:** Veri setindeki sınıflar arası sayısal dengesizliktir. Özellikle en kritik ve nadir vaka olan **Level 2 (Ağır Akne)** sınıfının azlığı (%13.9), modelin öğrenme sürecinde odaklanılan temel noktadır.

---

## 2. Kullanılan Veri Seti ve Ön İşleme
* **Veri Seti:** [Kaggle Acne Grading Dataset](https://www.kaggle.com/datasets/rutviklathiyateksun/acne-grading-classificationdataset) (999 RGB görüntü).
* **Ön İşleme:**
    * Görüntüler 224x224 boyutuna getirilmiş ve 1/255 oranında normalize edilmiştir.
    * **Mixup Augmentation:** Sınıf dengesizliğini gidermek için görüntüler belirli oranlarda karıştırılarak veri çeşitliliği artırılmıştır.
    * **Dinamik Veri Artırma:** Rotation, zoom ve flip teknikleri uygulanmıştır.

---

## 3. Model Mimarisi ve Yaklaşım Gerekçesi
### A. Vision Transformer (ViT-B16)
ViT, **Self-Attention** mekanizması sayesinde görüntüdeki pikseller arasındaki uzun menzilli ilişkileri analiz eder.  



* **Neden ViT?:** Geleneksel CNN'lerin aksine ViT, akne lezyonlarının yüz üzerindeki genel dağılımını (global bağlam) daha iyi yakalayabilmektedir.
* **Optimizasyon:** Bayesian Optimizasyonu (Keras Tuner) ile en iyi hiperparametreler seçilmiştir.
* **Kayıp Fonksiyonu:** Level 2 sınıfına **3.60 kat** daha fazla ağırlık veren **Balanced Focal Loss** kullanılarak azınlık sınıfın öğrenilmesi sağlanmıştır.

### B. Öz-Denetimli Öğrenme (SSL - SimCLR)
Etiketli verinin az olduğu senaryolarda modelin genel özelliklerini (renk, doku, biçim) öğrenebilmesi için **SimCLR** algoritması ile kontrastif öğrenme uygulanmıştır.

---

## 4. Çalıştırma Talimatları
### Bağımlılıklar
Proje Python 3.11 ve TensorFlow 2.19+ ortamında çalışmaktadır. Gerekli kütüphaneler `requirements.txt` dosyasındadır.

### Adımlar
1. Bağımlılıkları yükleyin: `pip install -r requirements.txt`
2. **Ana Model Eğitimi:** `new-vit-acne.ipynb` dosyasını çalıştırın.
3. **SSL Deneyleri:** `new-vit-acne-with-ssl.ipynb` dosyasını inceleyin.

---

## 5. Model Çıktıları ve Metrikler

### Nicel Sonuçlar
| Metrik | Ana ViT Modeli | SSL (SimCLR) Modeli |
| :--- | :--- | :--- |
| **Doğruluk (Accuracy)** | %82 | %20 |
| **Level 2 Recall** | **%96** | %100 |
| **F1-Skoru (Makro)** | %83 | %16 |

### Örnek Tahminler (Inference)
Modelin test verisi üzerindeki örnek sınıflandırma sonuçları aşağıda sunulmuştur:
![Örnek Tahminler](./predicts.png)

### Görsel Analizler (Outputs Klasörü)
**Standart ViT Modeli:**
* [Eğitim Grafikleri (Loss/Accuracy)](./outputs/accuracy_loss_curves.png)
* [Karmaşıklık Matrisi (Confusion Matrix)](./outputs/confusion_matrix.png)

**SSL (SimCLR) Deneyleri:**
* [SSL Eğitim Grafikleri](./outputs/accuracy_loss_curves_ssl.png)
* [SSL Karmaşıklık Matrisi](./outputs/confusion_matrix_ssl.png)

---

## 6. Proje Sunumu
Proje metodolojisi ve deney sonuçlarının detaylı analiz edildiği sunum dosyası:
👉 **[Nihai Sunum (PDF)](./sunum.pdf)**

*Not: Sunum içeriği, repoda bulunan kaynak kodlar ve `./outputs/` klasöründeki verilerle tam uyumludur.*
