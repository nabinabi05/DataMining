# Öğrenci Akademik Başarı ve Okul Bırakma Tahmini

## Proje Hakkında
Bu proje, bir veri madenciliği dönem projesidir. Açık Üniversite (Open University) öğrencilerinin demografik durumları, geçmiş akademik bilgileri ve Sanal Öğrenme Ortamı (VLE) üzerindeki etkileşim verilerini kullanarak akademik durumlarını (Pass, Fail, Distinction, Withdrawn) tahmin etmeyi amaçlamaktadır.

## Araştırma Sorusu
**"Öğrencilerin demografik özellikleri ve sanal öğrenme ortamındaki (VLE) günlük etkileşim örüntüleri, onların başarılı olma, kalma veya okulu bırakma durumlarını ne derece yüksek doğrulukla tahmin edebilir?"**

## Veri Seti
Projede kullanılan veri seti **OULAD (Open University Learning Analytics Dataset)**'tir. 
- [Veri Seti Sitesi / Bağlantısı](https://analyse.kmi.open.ac.uk/open_dataset)
- **Problem Türü:** Çok Sınıflı Sınıflandırma (Multi-class Classification)
  - Hedef Sınıflar: `Pass`, `Fail`, `Distinction`, `Withdrawn`
- **Kullanılan Tablolar:** `studentInfo`, `studentAssessment`, `studentVle`, `assessments`, `courses`, `studentRegistration`, `vle`

## Klasör Yapısı
```text
📦 DataMining
 ┣ 📂 data/        # Ham ve işlenmiş veri setleri (.csv)
 ┣ 📂 notebooks/   # Jupyter Notebook dosyaları (Keşifsel veri analizi, modelleme adımları)
 ┣ 📂 visuals/     # Üretilen grafikler ve görselleştirmeler
 ┣ 📂 docs/        # Proje ile ilgili kaynaklar ve dokümanlar
 ┣ 📂 reports/     # Ara / final raporları ve sunum dosyaları
 ┗ 📂 src/         # Yeniden kullanılabilir Python betikleri (.py) (Önişleme, pipeline vb.)
```

## Gereksinimler
Projenin çalışması için aşağıdaki kütüphanelerin ve araçların yüklü olması gerekmektedir:
- Python 3.x
- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn`
- `jupyter` / `ipykernel`

Gereksinimleri yüklemek için terminalde şu komutu çalıştırabilirsiniz:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## Proje Aşamaları
1. **Problem Tanımı:** Proje kapsamının, hedeflerin ve araştırma sorusunun belirlenmesi.
2. **Veri Toplama:** OULAD veri setinin çalışma ortamına indirilmesi ve dahil edilmesi.
3. **Keşifsel Veri Analizi (EDA):** Verideki dağılımların, eksik değerlerin, tablolar arası ilişkilerin ve korelasyonların incelenmesi.
4. **Veri Ön İşleme (Preprocessing):** Tabloların (join işlemleri ile) birleştirilmesi, eksik verilerin doldurulması/silinmesi, kategorik değişkenlerin dönüştürülmesi (encoding) ve özellik mühendisliği (feature engineering).
5. **Modelleme:** Çok sınıflı sınıflandırma için makine öğrenmesi algoritmalarının eğitilmesi.
6. **Değerlendirme:** Modellerin accuracy, precision, recall, f1-score ve karmaşıklık matrisi (confusion matrix) gibi metriklerle değerlendirilmesi.
7. **Raporlama:** Elde edilen bulguların belgelenerek sonuç raporunun oluşturulması.

## Nasıl Çalıştırılır
1. Bu depoyu yerel bilgisayarınıza klonlayın:
   ```bash
   git clone -b feature/problem-definition-data-collection https://github.com/nabinabi05/DataMining.git
   ```
2. Gerekli Python kütüphanelerini kurun.
3. İndirdiğiniz OULAD verilerini `data/` klasörünün altına taşıyın.
4. `notebooks/` dizini altındaki sırasıyla numaralandırılmış Jupyter Notebook dosyalarını açarak projeyi adım adım inceleyin ve çalıştırın.