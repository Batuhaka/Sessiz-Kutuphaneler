
# Sessiz Kütüphaneler: Gürültü İzleme ve IoT Entegrasyonu

## 1. Proje Konusu

Bu projede, kütüphane gibi sessizliğin önemli olduğu ortamlarda anlık ses seviyesini izleyen ve belirlenen eşik değerleri aşıldığında kullanıcıları LED'ler ve Telegram bildirimi yoluyla uyaran bir IoT tabanlı izleme sistemi geliştirilmiştir. Sistem, ESP32 mikrodenetleyicisi ve ses sensörü kullanılarak düşük maliyetli ve taşınabilir bir çözüm sunar.

## 2. Özet

Kütüphaneler gibi sessizliğin korunmasının kritik olduğu ortamlarda, ses kirliliğini izlemek ve anında müdahale sağlamak büyük önem taşır. Bu projede, ESP32 kullanılarak geliştirilen sistem, ortam sesini sürekli olarak takip eder ve belirlenen dB (desibel) eşiğinin aşıldığı durumlarda hem görsel (LED) hem de uzaktan (Telegram mesajı) bildirimlerle uyarı verir.

Başlangıçta kullanılan voltaj tabanlı ölçüm yöntemi, anlık değişimlere duyarlı olduğu için kararsız sonuçlar vermekteydi. Bu nedenle, daha tutarlı sonuçlar üreten, ardışık ölçümler üzerinden karar verilen bir algoritma geliştirilmiştir. Yeni yöntemde, 10 ölçümden en az 3'ü eşik değeri aştığında sistem durumu "gürültü var" olarak değerlendirir.

İlerleyen aşamalarda, farklı renk LED'ler aracılığıyla ses seviyesi sınıflandırması yapılacak ve kullanıcıya hem yerel hem de uzaktan uyarılar sağlanacaktır. Bu sayede kütüphane çalışanları ortam sessizliğini etkili bir şekilde denetleyebilecektir.

## 3. Kullanılan Yöntemler

- **ESP32 Geliştirme Kartı:** Ses verilerinin işlenmesi ve haberleşme görevlerini üstlenir.
- **Analog Ses Sensörü:** MIC_PIN (GPIO34) üzerinden analog ses verisi alınır.
- **Desibel Ölçüm Yöntemleri:**  
  - *Eski Yöntem:* ADC değeri → Voltaj → logaritmik işlemle desibel  
  - *Yeni Yöntem:* Ortamın taban (baseline) ses seviyesine göre fark hesaplanarak `map()` fonksiyonu ile dB'ye dönüştürme
- **Eşik Tabanlı Karar Algoritması:** 10 ölçümden en az 3 tanesi eşik değeri aşarsa sistem gürültü var olarak değerlendirir.
- **Telegram API:** Aşırı gürültü durumunda kullanıcıya internet üzerinden bildirim gönderilir.
- **LED ve Buzzer:** Görsel ve işitsel uyarı için kullanılır.

## 4. Yapılan Çalışmalar ve Görselleri

Projenin bu aşamasında, ses seviyesini çevreden algılayıp dB (desibel) cinsinden ölçebilen bir sistemin temelini oluşturacak olan sensör okuma ve işleme mekanizmaları geliştirilmiştir.

İlk olarak mikrofon sensörü ESP32'nin ADC (Analog-Dijital Çevirici) pinlerinden birine bağlanarak analog ses verileri okunmuştur. Başlangıçta kullanılan eski yöntemle, mikrofonun ürettiği voltaj değeri doğrudan ölçülüp şu formülle desibel değeri elde edilmeye çalışılmıştır:

```
dB = 20 * log10(voltage / 0.006) + 15
```

Bu formülde voltage, 0–3.3V aralığındaki analog değerin hesaplanmasıyla bulunuyordu. Ancak bu yöntem, ses seviyesi değişimlerine karşı çok hassas olup kararsız sonuçlar üretmekteydi. 

Daha sonra yapılan iyileştirmelerde, analog değerin doğrudan ADC sayısal değeri kullanılmış, voltaj dönüşümü kaldırılmış ve sabit baseline (taban) değerine göre fark alınarak dB benzeri bir "sound level" elde edilmiştir. Bu değer daha sonra `map()` fonksiyonu ile dB skalasına yaklaşık olarak çevrilmiştir:

```cpp
int soundLevel = abs(micValue - baseline);
int soundLevelDB = map(soundLevel, 0, 1200, 30, 100);
```

### Sessiz Ortam Testi ve Kalibrasyon

Doğru eşik değerlerinin belirlenmesi için sessiz bir ortamda yapılan gözlemler sonucunda:

- Mikrofon pininden okunan ADC değeri genellikle 1390 – 1420 aralığında kalmıştır.
- Bu değerler baz alınarak baseline değeri 1430 olarak ayarlanmıştır.

Bu fark alma yöntemi sayesinde anlık ses değişimlerine karşı daha istikrarlı sonuçlar elde edilmiştir.

### Gürültü Tespiti için 10 Ölçümden 3'ü Eşik Üzerindeyse Mantığı

Yeni sistemde, arka arkaya alınan 10 ses seviyesi ölçümünden en az 3 tanesi belirlenen dB eşiğini aşıyorsa bu durum gerçek bir gürültü olarak değerlendirilmekte ve ilgili çıktı (LED veya mesaj) tetiklenmektedir.

İlerleyen aşamalarda, bu ses seviyesi sınıflandırmasına göre 2 yeşil, 1 sarı, 1 turuncu ve 1 kırmızı LED ile görsel bir uyarı sistemi kurulacak ve kritik eşikler aşıldığında Telegram üzerinden mesaj gönderilecek şekilde sistem tamamlanacaktır.

<p align="center">
  <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/raw/main/Ara%20Rapor/Figures/TemsiliDevre.jpg" alt="Şekil 1: Temsili Devre" width="500"/>
</p>
<p align="center"><strong>Şekil 1: Temsili Devre</strong></p>

<p align="center">
  <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/raw/main/Ara%20Rapor/Figures/GercekDevre.jpg" alt="Şekil 2: Gerçek Devre" width="500" height="400"/>
</p>
<p align="center"><strong>Şekil 2: Gerçek Devre</strong></p>

## 5. Elde Edilen Sonuçlar

- Yeni algoritma sayesinde ses ölçümünde anlık sapmaların etkisi azaltıldı.
- Ortam sesi daha kararlı şekilde değerlendirildi.
- Telegram üzerinden gönderilen uyarılar başarıyla ulaştırıldı.
- İlk testlerde sistemin belirlenen eşiklerde doğru tepkiler verdiği gözlemlendi.
- Ses seviyesi ölçüm aralığı yaklaşık 30 dB – 100 dB arasında ölçeklendi.

## 6. Karşılaşılan Sorunlar ve Çözümler

| Sorun Başlığı            | Sorun Açıklaması                                                                 | Uygulanan Çözüm                                                                 |
|--------------------------|----------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Hatalı dB Ölçümü         | Logaritmik voltaj-dB dönüşüm yöntemi hatalı ve kararsız sonuçlar verdi.         | Ortam taban ses seviyesi baz alınarak fark üzerinden ölçüm yapılması sağlandı. |
| Değişken Ortam Gürültüsü | Anlık gürültüler ölçüm sonuçlarını bozuyordu.                                   | Ortalama yerine 10 ölçümden 3'ünün eşik aşması mantığı geliştirildi.           |


## 7. Projenin Devamında Yapılacaklar

- Farklı ses seviyelerini belirten 5 LED (2 yeşil, 1 sarı, 1 turuncu, 1 kırmızı) ve bir buzzer devreye entegre edilecek.
- Kırmızı LED yandığında buzzer uyarıcı bir ses çıkaracak.
- Ses seviyesine göre bu LED'ler anlık yanacak.
- Ayrıca sistem, yüksek gürültü devam ederse belirli aralıklarla kullanıcıyı tekrar uyaracak.

## Katkıda Bulunanlar:
| İsim                 | GitHub Profili                             |
|----------------------|---------------------------------------------|
| Batuhan Genç         | [Batuhaka](https://github.com/Batuhaka)         |
| Habib Salim          | [habibsalimov](https://github.com/habibsalimov)      |
| Yücel Aytaç Akgün    | [Aytacus](https://github.com/Aytacus)     |
