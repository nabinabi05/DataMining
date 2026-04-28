# Öğrenci Akademik Başarı ve Okul Bırakma Tahmini: Problem Tanımı

## 1. Neden Bu Konu Seçildi?
- **Yükseköğretimde Okul Bırakma Oranlarının Önemi:** Üniversite öğrencilerinin programlarını tamamlamadan okulu bırakmaları (dropout), hem eğitim kurumları hem de öğrenciler açısından zaman, emek ve maddi kayıp anlamına gelmektedir. 
- **Erken Müdahalenin Önemi:** Risk altındaki öğrencilerin dönem devam ederken veya dönem başında tespit edilebilmesi, eğitmenlerin ve rehberlerin erken müdahale etmesini sağlar. Zamanında yapılacak bir yönlendirme, öğrencinin akademik başarısını artırıp okulu bırakmasının önüne geçebilir.
- **Veri Odaklı Çözümler:** Geleneksel yöntemlerin aksine, Sanal Öğrenme Ortamı (VLE) verileri ile öğrencilerin sisteme giriş periyotları, ödev yapma alışkanlıkları ve materyal inceleme süreleri analiz edilebilir. Makine öğrenmesi yöntemleri sayesinde bu devasa log verilerinden anlamlı örüntüler çıkarılabilmekte ve proaktif çözümler üretilebilmektedir.

## 2. Araştırma Sorusu
Bu projenin temel araştırma sorusu şudur:
> *"Bir öğrencinin dönem sonu akademik sonucu (Pass / Fail / Distinction / Withdrawn), demografik özellikleri ve öğrenme yönetim sistemi etkileşim verileri kullanılarak makine öğrenmesi modelleri ile tahmin edilebilir mi?"*

## 3. Hedef Değişken (Target Variable)
Projede öğrencinin akademik başarısı **`final_result`** sütunu üzerinden değerlendirilecektir. Bu sütun 4 farklı kategoriden oluşmaktadır:
1. **Pass:** Öğrencinin dersi başarıyla tamamladığını ifade eder.
2. **Fail:** Öğrencinin dersten başarısız olduğunu (kaldığını) gösterir.
3. **Distinction:** Öğrencinin dersi oldukça yüksek bir başarı notu ile (üstün başarı/onur puanı) geçtiğini ifade eder.
4. **Withdrawn:** Öğrencinin dersi yarıda bıraktığını (okuldan veya dersten çekildiğini) ifade eder.

## 4. Kullanılacak OULAD Tabloları ve Gerekçeleri
Veri setinde (OULAD) yer alan çok sayıda tablodan projeye doğrudan katkı sağlayacak olanlar şunlardır:
- **`studentInfo`**: Öğrencilerin cinsiyet, yaş kuşağı, daha önceki eğitim durumu, sosyo-ekonomik arka planı gibi demografik bilgilerini içerir. Öğrencinin geçmiş profili, başarısı üzerinde etkili bir temel faktördür.
- **`studentVle`**: Öğrencinin Sanal Öğrenme Ortamındaki (VLE) materyallere tıklama sayılarını, günlük etkileşimlerini içerir. Katılım (engagement) ölçümü için kritik öneme sahiptir.
- **`studentAssessment`**: Öğrencilerin girdikleri sınavların veya teslim ettikleri ödevlerin puanlarını gösterir. Başarının sayısal kanıtını sunar.
- **`assessments`**: İlgili derste hangi tip değerlendirmelerin (sınav, proje, kısa sınav) olduğunu ve teslim tarihlerini sunar. `studentAssessment` verisi ile birleşerek zaman/performans ilişkisini analiz etmeyi sağlar.
- **`studentRegistration`**: Öğrencilerin derslere kayıt olduğu ve dersten çekildiği gün sayısını (tarihleri) barındırır. Çekilme eyleminin analizinde doğrudan kullanılabilir.
- **`courses`**: Ders modülleri, sunum dönemi (yıl/ay) ve kredi bilgisini tutar. Aynı dersin farklı dönemlerdeki zorluk değişimlerini kontrol etmek için temel tablodur.

## 5. Çıktı Türü (Problem Tipi)
- **Temel Yaklaşım (Çok Sınıflı Sınıflandırma):** Model, öğrencinin dönem sonu sonucunu 4 sınıf (`Pass`, `Fail`, `Distinction`, `Withdrawn`) arasında tahmin edecektir.
- **İkinci / Alternatif Seçenek (İkili Sınıflandırma):** Eğer 4 sınıflı modelde performans düşük kalırsa veya işletme hedefi sadece "okulu bırakacak olanları bulmak" yönünde daraltılırsa, hedef değişken ikili sınıfa dönüştürülerek model `Withdrawn` vs. `Diğerleri` şeklinde yeniden yapılandırılabilir.

## 6. Başarı Metrikleri
Kullanılacak veri setindeki sınıfların (hedef değişken) muhtemelen dengesiz dağılmış olması ihtimaline karşı modellerin değerlendirilmesinde aşağıdaki metrikler dikkate alınacaktır:
- **Accuracy (Doğruluk):** Modelin genel doğru tahmin oranı.
- **F1-score (Macro-Averaged):** Sınıflar arası dengesizlik durumunda algoritmaların azınlık sınıfları ne kadar iyi ayırt ettiğini adil bir şekilde görmek için.
- **ROC-AUC Eğrisi (OVP):** Çoklu sınıflar için Bir-Diğerlerine-Karşı (One-vs-Rest) stratejisi kullanılarak modelin sınıf ayırma gücünü ölçmek için.
- **Karmaşıklık Matrisi (Confusion Matrix):** Hangi sınıfların (ör. Pass ve Fail veya Fail ve Withdrawn) birbiriyle daha çok karıştırıldığını detaylıca analiz etmek için.