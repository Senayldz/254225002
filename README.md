# 🩺 Vision Transformer (ViT) ile Akne Şiddeti Sınıflandırması

Bu proje, yüz görüntülerinden akne şiddetini (**Level 0, 1, 2**) Vision Transformer (ViT) mimarisi kullanarak sınıflandıran, akademik amaçlı bir derin öğrenme çalışmasıdır. Proje kapsamında hem standart denetimli öğrenme hem de **Öz-denetimli Öğrenme (Self-Supervised Learning - SSL)** yaklaşımları test edilmiştir.

---

## 1. Problemin Tanımı
Akne şiddetinin klinik teşhisinde standart bir karar destek sistemi oluşturmak hedeflenmiştir. 
**Temel Zorluk:** Veri setindeki sınıflar arası sayısal dengesizliktir. Özellikle en kritik vaka olan **Level 2 (Ağır Akne)**, veri setinin sadece %13.9'unu oluşturmaktadır. Bu durum modelin nadir vakaları öğrenmesini zorlaştıran bir engeldir.

---

## 2. Kullanılan Veri Seti ve Ön İşleme
* **Veri Seti:** Kaggle Acne Grading Dataset (999 RGB görüntü).
* **Dağılım:** Level_0 (%38.7), Level_1 (%47.3), Level_2 (%13.9).
* **Ön İşleme Adımları:**
    * **Boyutlandırma:** Görüntüler ViT modeline uygun olarak 224x224 piksele getirilmiştir.
    * **Normalizasyon:** [0, 1] aralığına ölçekleme yapılmıştır.
    * **Data Augmentation:** Modelin genelleme kapasitesini artırmak için Mixup, rastgele döndürme ve yakınlaştırma teknikleri uygulanmıştır.

---

## 3. Model Mimarisi ve Yaklaşım Gerekçesi
### A. Vision Transformer (ViT-B16)
Geleneksel CNN yapılarının aksine ViT, **Self-Attention** mekanizması sayesinde görüntüdeki pikseller arasındaki uzun menzilli ilişkileri kurabilir. Akne gibi cilde yayılan dokusal bozukluklarda bu mekanizma daha kapsamlı bir özellik çıkarımı sağlar.
* **Optimizasyon:** Keras Tuner ile **Bayesian Optimizasyonu** kullanılarak en iyi hiperparametreler belirlenmiştir.
* **Kayıp Fonksiyonu:** Dengesiz veri problemi için **Balanced Focal Loss** (Gamma: 2.5) tercih edilmiştir.

### B. Öz-Denetimli Öğrenme (SSL - SimCLR)
Veri setindeki sınırlı etiketli veri problemini aşmak amacıyla **SimCLR** algoritması ile kontrastif öğrenme uygulanmıştır. Bu aşamada modelin, etiketlere ihtiyaç duymadan akne dokularına ait ayırt edici temsilleri öğrenmesi amaçlanmıştır.



---

## 4. Çalıştırma Talimatları
### Bağımlılıklar
Proje, Python 3.11 ve TensorFlow/Keras 3.x ortamında geliştirilmiştir. Gerekli kütüphaneler `requirements.txt` dosyasında mevcuttur.

### Kurulum ve Çalıştırma
1. Bağımlılıkları yükleyin: `pip install -r requirements.txt`
2. Ana Modeli Eğitmek/Test Etmek için: `new-vit-acne.ipynb`
3. SSL Deneylerini İncelemek için: `new-vit-acne-with-ssl.ipynb`
*Not: Notebook'lardaki `BASE_PATH` değişkenini veri setinizin yoluna göre güncellemeyi unutmayın.*

---

## 5. Model Çıktıları ve Metrikler

### Nicel Sonuçlar (Ana Model)
| Metrik | Değer |
| :--- | :--- |
| **Doğruluk (Accuracy)** | %82 |
| **Level_2 (Kritik Sınıf) Recall** | **%96** |
| **F1-Skoru (Makro)** | %83 |

*SSL Deneyi Sonucu:* Sınırlı veri ve yüksek hesaplama maliyeti nedeniyle SSL aşamasında doğrulama başarısı %20 seviyesinde kalmıştır. Detaylar sunum dosyasında tartışılmıştır.

### Görsel Çıktılar
* **Eğitim Grafikleri:** Eğitim sürecindeki Loss ve Accuracy değişimleri `./outputs/` klasöründedir.
* **Inference:** Test verisi üzerindeki örnek tahminler ve Confusion Matrix çıktıları kaynak kodlarda ve `./outputs/` altında sunulmuştur.

---

## 6. Proje Sunumu
Deneylerin metodolojisini, teorik arka planını ve karşılaştırmalı analizlerini içeren nihai sunum dosyasına aşağıdaki bağlantıdan ulaşabilirsiniz:
👉 **[Sunum Dosyası (PDF)](./sunum.pdf)**

*Bu proje içeriği, sunulan PDF dosyasındaki deneylerle birebir örtüşmektedir.*# Vision Transformer (ViT) ile Akne Şiddeti Sınıflandırması

Bu proje, yüz görüntülerinden akne şiddetini (Level 0, 1, 2) Vision Transformer mimarisi kullanarak sınıflandıran derin öğrenme tabanlı bir sistemdir.

---

## 1. Problemin Tanımı
Akne şiddetinin klinik olarak doğru teşhis edilmesi, tedavi planlaması için kritiktir. Bu projede, manuel teşhis süreçlerine destek olmak amacıyla görüntülerden otomatik seviye tespiti yapılması hedeflenmiştir. 
**Temel Zorluklar:** Sınıflar arasındaki görsel benzerliklerin yüksek olması ve veri setindeki şiddetli akne (Level 2) vakalarının azlığı (Sınıf dengesizliği).

---

## 2. Veri Seti ve Ön İşleme
* **Veri Seti:** [Kaggle Acne Grading Dataset](https://www.kaggle.com/datasets/rutviklathiyateksun/acne-grading-classificationdataset) (999 RGB görüntü).
* **Sınıf Dağılımı:** Level_0 (%38.7), Level_1 (%47.3), Level_2 (%13.9).
* **Ön İşleme:**
    * Görüntüler ViT giriş boyutu olan **224x224** piksele ölçeklendirilmiştir.
    * Normalizasyon (1/255) işlemi uygulanmıştır.
    * **Mixup Augmentation** ve dinamik veri artırma (rotation, zoom, flip) teknikleri ile modelin genelleme yeteneği artırılmıştır.

---

## 3. Model Mimarisi ve Yaklaşım Gerekçesi
* **Mimari:** ImageNet-21k üzerinde ön eğitim almış **Vision Transformer (ViT-B16)**.
* **Gerekçe:** Geleneksel CNN'lerin aksine, ViT'nin **Self-Attention** mekanizması görüntüdeki dokusal bozukluklar arasındaki küresel ilişkileri daha iyi yakalar. Akne gibi cilde yayılan lezyonlarda bu "uzun menzilli" ilişki tespiti teşhis doğruluğunu artırır.
* **Optimizasyon:** Hiperparametreler **Bayesian Optimizasyonu** ile belirlenmiş; sınıf dengesizliği için **Balanced Focal Loss** kullanılmıştır.

---

## 4. Çalıştırma Talimatları
### Bağımlılıklar ve Ortam
Proje Python 3.11+ ve GPU destekli (CUDA) bir ortamda çalıştırılmalıdır. Gerekli kütüphaneler `requirements.txt` dosyasında listelenmiştir.

### Kurulum ve Çalıştırma
1. Depoyu klonlayın: `git clone <repo-url>`
2. Bağımlılıkları yükleyin: `pip install -r requirements.txt`
3. Eğitimi başlatmak ve test etmek için: `jupyter notebook new-vit-acne.ipynb` dosyasındaki tüm hücreleri çalıştırın.

---

## 5. Model Çıktıları ve Analiz

### Nicel Metrikler (Test Sonuçları)
| Metrik | Değer |
| :--- | :--- |
| **Genel Doğruluk (Accuracy)** | %82 |
| **Level_2 Duyarlılığı (Recall)** | %96 |
| **F1-Skoru (Makro Avg)** | %83 |

### Eğitim Süreci Grafikleri

*Modelin eğitim ve doğrulama süreçlerine ait Loss/Accuracy grafikleri ana dizindeki `./outputs/` klasöründe yer almaktadır.*

### Örnek Inference Görselleri

*Modelin test setindeki görüntüler üzerinde yaptığı tahminler notebook çıktılarında ve `./outputs/` klasöründe görselleştirilmiştir.*

---

## 6. Proje Sunumu
Projenin metodolojisi, deney düzenekleri ve detaylı sonuç analizlerini içeren nihai sunum dosyasına buradan erişebilirsiniz:
👉 **[Sunum Dosyası (PDF)](./sunum.pdf)**

*Not: Sunumda anlatılan tüm deneyler ve parametreler bu depodaki kaynak kodlarla birebir örtüşmektedir.*
