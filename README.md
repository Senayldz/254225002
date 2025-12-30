# Vision Transformer (ViT) ile Akne Şiddeti Sınıflandırması

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
