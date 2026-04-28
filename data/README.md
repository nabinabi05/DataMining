# OULAD Veri Seti (Open University Learning Analytics Dataset)

## 1. Veri Seti Hakkında
- **Açıklama:** Bu veri seti, İngiltere'deki The Open University tarafından yayınlanmış gerçek öğrencilerin demografik durumlarını ve ögrenme yönetim sistemi etkileşimlerini (VLE - Virtual Learning Environment) içeren gerçek bir veridir.
- **Lisans:** Creative Commons Gerekçelendirme 4.0 Uluslararası (CC BY 4.0) Lisansı altındadır.
- **İndirme Bağlantısı:** [OULAD Resmi Sitesi](https://analyse.kmi.open.ac.uk/open_dataset)

## 2. Klasör Yapısı
Bu klasör (`data/`) projede kullanılan veri setlerini barındırmaktadır:
- **`data/raw/`**: Kaynak siteden doğrudan indirilen ham (orijinal) CSV dosyalarının bulunduğu klasör.
- **`data/processed/`**: Gerekli veri temizleme, ön işleme ve özellik mühendisliği işlemlerinden (EDA) geçmiş, doğrudan modelleme için kullanıma hazır olan verilerin tutulduğu klasör.

## 3. CSV Dosyalarının Açıklaması

* **`studentInfo.csv`**
  - **İçerik:** Öğrencilerin temel demografik bilgileri, daha önceki eğitim geçmişleri ve dönem sonu başarı durumları.
  - **Temel Sütunlar:** `id_student`, `gender`, `region`, `highest_education`, `imd_band`, `age_band`, `disability`, `final_result`.
  - **İlişkisi:** Veri analizindeki en merkezi dosyalardan biridir. `id_student` ile diğer tüm öğrenci hareket verisi dosyalarına bağlanır.

* **`studentAssessment.csv`**
  - **İçerik:** Öğrencilerin yıl içinde teslim ettikleri değerlendirmelerden (sınav, ödev vb.) aldığı notlar.
  - **Temel Sütunlar:** `id_assessment`, `id_student`, `date_submitted`, `is_banked`, `score`.
  - **İlişkisi:** Hangi öğrenciye ait olduğu `id_student` ile belirlenirken; sınav/ödev özellikleri `id_assessment` anahtarı üzerinden tespit edilir.

* **`studentVle.csv`**
  - **İçerik:** Öğrencilerin VLE (Sanal Öğrenme Ortamı) üzerindeki ders materyalleriyle günlük etkileşim ve sayfa tıklama frekansları.
  - **Temel Sütunlar:** `code_module`, `code_presentation`, `id_student`, `id_site`, `date`, `sum_click`.
  - **İlişkisi:** Zaman serisi formatındaki bu log verisi; materyal detayına `id_site` ile, öğrenci demografik verisine ise `id_student` ile katılır (join edilir).

* **`assessments.csv`**
  - **İçerik:** Modüllerde (derslerde) yer alan değerlendirmelerin tipleri (TMA, CMA, Exam), sınav ağırlıkları ve teslim tarihlerini barındırır.
  - **Temel Sütunlar:** `code_module`, `code_presentation`, `id_assessment`, `assessment_type`, `date`, `weight`.
  - **İlişkisi:** `id_assessment` kullanılarak `studentAssessment.csv` tablosuyla ilişkilendirilir.

* **`courses.csv`**
  - **İçerik:** Üniversitenin sunduğu ders modüllerini, modülün sunulduğu yarıyıl dönemini (presentation) ve yarıyılın süresini içerir.
  - **Temel Sütunlar:** `code_module`, `code_presentation`, `module_presentation_length`.
  - **İlişkisi:** `code_module` ve `code_presentation` birlikte kullanılarak ilgili dersin diğer veri dosyalarındaki kayıtlarına ulaşılır.

* **`studentRegistration.csv`**
  - **İçerik:** Öğrencilerin modül sunumları için derse kayıt oldukları ve (eğer ayrıldılarsa) dersi bıraktıkları gün bilgilerini (tarihleri) barındırır.
  - **Temel Sütunlar:** `code_module`, `code_presentation`, `id_student`, `date_registration`, `date_unregistration`.
  - **İlişkisi:** Hem bölüm terkini (Withdrawn) destekleyici özellikler içerir hem de `id_student` ile `studentInfo` tablosuna direkt bağlanır.

* **`vle.csv`**
  - **İçerik:** Sanal Öğrenme Ortamında yer alan PDF, video, format, URL gibi ders içi materyallerin detaylarını verir.
  - **Temel Sütunlar:** `id_site`, `code_module`, `code_presentation`, `activity_type`, `week_from`, `week_to`.
  - **İlişkisi:** `studentVle` günlük tıklama loglarındaki faaliyet tiplerini ve haftalık sınırları öğrenmek için `id_site` üzerinden join yapılır.

## 4. Tablolar Arası İlişkiler (Keys & Joins)
OULAD veritabanı birbirine foreign key yapılarıyla bağlıdır. Modellemede veri setlerini tek bir büyük analitik matrise dönüştürmek için join kurmak hayati öneme sahiptir.
- **`id_student`, `code_module`, `code_presentation` üçlüsünün önemi:** Bir öğrenci geçmişte kaldığı bir derse `(code_module)` farklı bir dönemde `(code_presentation)` tekrar kaydolabilir. Bu durumlardan dolayı öğrenci performansı sadece `id_student` bazında değil, *Öğrenci + Ders + Dönem* üçlüsünün kombinasyonuna göre benzersiz bir şekilde ayrıştırılıp incelenmelidir.
- Temel eşleştirme anahtarları:
  - `id_student` -> Çoğu öğrenci logunu `studentInfo` ile bağlar.
  - `id_assessment` -> Sonuçları (`studentAssessment`) içerik yapısıyla (`assessments`) birleştirir.
  - `id_site` -> Tıklama olaylarını (`studentVle`), spesifik materyalle (`vle`) eşlemenizi sağlar.

## 5. Hedef Değişken (Target Variable)
Problemde modelin sınıflandırıp öğrenmeye çalışacağı hedef değişken **`final_result`** sütunudur. Bu kolon **`studentInfo.csv`** tablosu içerisinde konumlanmaktadır. Aldığı sınıflar (kategoriler) şunlardır:
1. **Pass:** Ders başarıyla geçilmiş
2. **Fail:** Dersten başarısız olunmuş
3. **Distinction:** Üstün başarı / onur puanıyla ders geçilmiş
4. **Withdrawn:** Öğrenci dersi resmi olarak yarıda bırakmış ve ayrılmış