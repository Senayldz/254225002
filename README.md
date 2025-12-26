# Vision Transformer (ViT) ile Akne Şiddeti Sınıflandırması

Bu proje, yüz görüntülerinden akne şiddetini otomatik olarak sınıflandırmayı amaçlayan derin öğrenme tabanlı bir çalışmadır. Model olarak güncel **Vision Transformer (ViT)** mimarisi kullanılmış ve sınıf dengesizliği problemleri gelişmiş kayıp fonksiyonları ile aşılmıştır.

---

## Hızlı Erişim
* **[Ana Kod Dosyası (Notebook)](./vit-acne.ipynb)**: Modelin veri yükleme, eğitim ve test süreçlerini içeren Jupyter Notebook dosyasıdır. 
* **[Proje Sunumu (PDF)](./sunum.pdf)**: Çalışmanın teorik altyapısını, yöntemini ve sonuçlarını özetleyen nihai sunum dosyasıdır.
* **Model Performans Grafikleri**:
    * **[Eğitim Eğrileri (Accuracy & Loss)](./accuracy_loss_curves.png)**: Eğitim süresince doğruluk ve kayıp değerlerinin değişim grafikleridir.
    * **[Karmaşıklık Matrisi (Confusion Matrix)](./confusion_matrix.png)**: Sınıf bazlı tahmin başarılarını gösteren hata matrisidir. 
*  **[Gereksinimler](./requirements.txt)**: Projenin yerel ortamda çalıştırılması için gereken kütüphane listesidir.
---

##  1. Problemin Tanımı
Akne şiddetinin belirlenmesi klinik teşhis süreçlerinde önemlidir. Bu proje, akne seviyelerini üç ana sınıfta (Level_0, Level_1, Level_2) sınıflandırarak uzmanlara karar destek sistemi sağlamayı hedeflemektedir. Veri setindeki sınıflar arası sayısal dengesizlik (yani Level_2 sınıfının azlığı), modelin öğrenme sürecinde karşılaşılan temel zorluktur.

##  2. Veri Seti ve Ön İşleme
* **Veri Seti:** Kaggle Acne Grading Dataset.
* **Sınıf Dağılımı:**
    * **Level_0:** Hafif (%38.7) 
    * **Level_1:** Orta (%47.3) 
    * **Level_2:** Şiddetli (%13.9) 
* **Ön İşleme Adımları:**
    * Görüntüler **224x224** boyutuna ölçeklendirilmiştir.
    * Piksel değerleri **1/255** oranında normalize edilmiştir.
    * **Mixup Augmentation** ve kapsamlı veri artırma teknikleri kullanılarak veri çeşitliliği artırılmıştır.

##  3. Model Mimarisi ve Yaklaşım
* **Mimari:** ImageNet-21k üzerinde ön eğitim almış **ViT-Base (Vision Transformer)**.
* **Gerekçe:** Geleneksel CNN yapılarının aksine, ViT mimarisi **Self-Attention (Öz-dikkat)** mekanizması sayesinde görüntüdeki dokusal özellikler arasındaki uzun menzilli ilişkileri daha iyi analiz edebilmektedir.
* **Kayıp Fonksiyonu:** Nadir görülen ağır vakaların (Level_2) kaçırılmaması için **Balanced Focal Loss** kullanılmış ve bu sınıfa **3.60 kat** daha fazla ağırlık verilmiştir.
.

##  4. Model Performansı (Nicel Metrikler)
Eğitim sonucunda elde edilen temel başarı metrikleri şöyledir:
* **Doğruluk (Accuracy):** %81
* **Ağır Akne (Level_2) Duyarlılığı (Recall):** %93
* **F1-Skoru (Makro Ortalaması):** %82

Eğitim sürecine ait grafiklere ve hata analizine **[buradan](./outputs/)** ulaşabilirsiniz.

---

##  5. Çalıştırma Talimatları
1.  Depoyu klonlayın.
2.  Bağımlılıkları yükleyin:
    ```bash
    pip install -r requirements.txt
    ```
3.  Jupyter Notebook üzerinden `vit-acne.ipynb` dosyasını açarak tüm hücreleri çalıştırın.
