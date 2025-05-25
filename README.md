
#  Amazon Ürün Yorumları ile Duygu Analizi (Sentiment Analysis)

Bu projede Amazon ürün yorumlarını kullanarak bir metin sınıflandırma modeli geliştirdim. Amaç, kullanıcı yorumlarını analiz ederek, bu yorumların **olumlu mu yoksa olumsuz mu** olduğunu tahmin etmektir.

##  Problem Tanımı

Gerçek dünyada şirketler milyonlarca kullanıcı yorumuna sahiptir. Ancak bu yorumların olumlu mu olumsuz mu olduğunu elle incelemek mümkün değildir. Bu nedenle bu projede:

- **Gözetimli öğrenme** ile: etiketli veriler kullanılarak bir sınıflandırma modeli geliştirildi.
- **Gözetimsiz öğrenme** ile: etiket olmadan yorumlar kümelere ayrılarak içerik analizi yapıldı.

##  Kullanılan Veri Seti

- **Kaynak:** [Kaggle - Amazon Fine Food Reviews](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)
- **Boyut:** ~50 MB (yaklaşık 500.000 yorum)
- **Sütunlar:**
  - `Text`: Yorum metni
  - `Score`: 1–5 puan
  - `Sentiment`: (oluşturuldu) → 1 = olumlu, 0 = olumsuz
  
---

## Keşifsel Veri Analizi (EDA) ve Veri Görselleştirmeleri

Bu bölümde, veri setinin yapısını, dağılımlarını ve olası anormallikleri anlamak için keşifsel veri analizi (EDA) adımları uygulanmıştır. Görselleştirmeler aracılığıyla veri setindeki puan ve duygu dağılımları incelenmiştir.

### Puan Dağılımı

Amazon yorumlarındaki 1'den 5'e kadar olan puanların dağılımını gösteren bir grafik oluşturulmuştur. Bu, kullanıcıların ürünleri genel olarak nasıl değerlendirdiğine dair bir fikir verir ve veri setinin genel eğilimini ortaya koyar.

###  Duygu Dağılımı

`Score` sütunundan türetilen ikili `Sentiment` (Duygu) sütununun (1: Pozitif, 0: Negatif) dağılımı görselleştirilmiştir. Bu grafik, veri setindeki sınıf dengesizliğini anlamak açısından kritik öneme sahiptir. Görüldüğü üzere, pozitif yorumlar (1) veri setinde büyük çoğunluğu oluşturmaktadır.

**Not:** Bu grafik, `Score` 3 olan yorumlar çıkarılmadan önceki genel duygu dağılımını gösterir.


## Özellik Mühendisliği

- `Score` değişkeninden 3 puanlı nötr yorumlar çıkarıldı.
- Yorumlar temizlendi (büyük harf, noktalama, boşluklar).
- `Sentiment` sütunu oluşturuldu.
- TF-IDF yöntemi ile metinler vektöre dönüştürüldü.
- `max_features=1000` ile yalnızca anlamlı kelimeler modele dahil edildi.

##  Kullanılan Yöntemler

### 1. Gözetimli Öğrenme (Supervised)
- **Model:** Logistic Regression
- **Veri ayrımı:** train_test_split (%80 eğitim, %20 test)
- **Değerlendirme Metrikleri:** Accuracy, Precision, Recall, F1-score
- **Bonus:** class_weight='balanced' ile dengesiz veri için alternatif model de denendi.

### 2. Gözetimsiz Öğrenme (Unsupervised)
- **Model:** KMeans (n=2)
- **Amaç:** Etiket olmadan yorumları içeriklerine göre kümelere ayırmak
- **Görselleştirme:** WordCloud ile her kümedeki baskın kelimeler görselleştirildi

##  Model Performansı

### 1️ Standart Logistic Regression
- Accuracy: 93%
- Olumlu yorum (sınıf 1): Recall = 98%, F1 = 0.96
- Olumsuz yorum (sınıf 0): Recall = 70%, F1 = 0.77

### 2️ Dengelenmiş Logistic Regression (class_weight='balanced')
- Accuracy: 90%
- Olumlu yorum: Recall = 90%, F1 = 0.94
- Olumsuz yorum: Recall = 91%, F1 = 0.75

> Bu ikinci model, özellikle azınlık sınıf olan olumsuz yorumları daha iyi tanımakta başarılı oldu.

##  WordCloud ile Görselleştirme

KMeans ile gruplandırılan yorumlarda öne çıkan kelimeler WordCloud ile görselleştirilmiştir. Bu sayede her kümenin içerik teması daha kolay yorumlanmıştır.

##  Bonus: Dengesiz Veri İçin Alternatif Model

Veri setinde olumlu yorumlar büyük çoğunluktaydı. Bu nedenle model, olumsuz yorumları tanımakta zorlanıyordu.

Bu durumu iyileştirmek için, `class_weight='balanced'` parametresi ile azınlık sınıfa daha fazla önem veren bir model de denendi. Bu model ile daha dengeli sonuçlar elde edildi.

##  Projenin Yayınlandığı Yerler

- ##  Projenin Yayınlandığı Yerler

-  Kaggle Notebook (Supervised): [Sentiment Analysis on Amazon Reviews (Supervised)](https://www.kaggle.com/code/zlemeker/sentiment-analysis-on-amazon-reviews)
-  Kaggle Notebook (Unsupervised): [Unsupervised Review Clustering](https://www.kaggle.com/code/zlemeker/unsupervised-review-clustering)

-  GitHub Repo: [amazon-sentiment-analysis](https://github.com/ozlemeker/amazon-sentiment-analysis)

##  Geliştirme Fikirleri

- Farklı modeller (SVM, Random Forest, XGBoost) ile karşılaştırma
- Web arayüzü (Streamlit, Gradio) ile yorum tahmin sistemi
- Türkçe yorumlar ile aynı sürecin uygulanması
