# IoT Dönem Projesi Öneri Raporu: Akıllı Kütüphane Gürültü Takip ve Bildirim Sistemi


## Grup 10
**Batuhan Genç, Habib Salim, Yücel Aytaç Akgün**

## Proje Konusu:
Kütüphanelerde sessiz çalışma ortamını korumak için belirlenen desibel seviyesini aşan gürültüler tespit edilerek uyarı sistemi devreye girecektir. İlk aşıldığında bir LED yanarak kullanıcıya uyarı verilecek, ikinci kez aşıldığında ise başka bir LED yanacak ve bildirim sistemi devreye girerek yetkililere haber gönderilecek. Bu sayede sessizlik sağlanarak verimli bir çalışma ortamı oluşturulması hedeflenmektedir.

## Proje Hedefleri:
 - **Sessiz Çalışma Ortamı Sağlamak:** Kütüphanelerde belirlenen desibel seviyesini koruyarak kullanıcıların verimli bir şekilde çalışmasını desteklemek.
 - **Gerçek Zamanlı Gürültü Takibi:** Ortamdaki ses seviyesini sürekli izleyerek anlık olarak değişiklikleri tespit etmek. 
 - **LED ile Kullanıcı Uyarısı:** Gürültü belirlenen sınırı aştığında ilk LED’in yanarak kullanıcıları uyarmasını sağlamak.
 - **Bildirim Sistemi ile Yetkili Uyarısı:** Gürültü seviyesi ikinci kez aşıldığında ikinci LED’in yanmasıyla birlikte yetkililere otomatik bildirim göndermek.
 - **IoT Entegrasyonu:** Sensörler ve kablosuz iletişim teknolojileri kullanarak sistemin uzaktan yönetilebilir olmasını sağlamak.

## Tahmini Zaman Çizelgesi  

| **Aşama** | **Süre** | **Açıklama** |
|-----------|---------|-------------|
| **Donanım Temini** | 1 Hafta | Ses sensörü, mikrodenetleyici (Arduino, ESP32 vb.), LED ve diğer bileşenlerin temin edilmesi. |
| **Sensör Kalibrasyonu ve Testleri** | 2 Hafta | Ses sensörünün belirlenen desibel seviyelerine uygun şekilde çalıştığının test edilmesi. |
| **LED ve Bildirim Sistemi Geliştirme** | 1-2 Hafta | İlk LED uyarısı ve ikinci LED ile bildirim gönderme mekanizmasının geliştirilmesi. |
| **IoT Entegrasyonu** | 2 Hafta | Wi-Fi veya Bluetooth kullanarak bildirimlerin uzaktan iletilmesini sağlamak. |
| **Yazılım ve Algoritma Geliştirme** | 2 Hafta | Ses verilerini işleyen algoritmaların ve bildirim mekanizmalarının yazılımının tamamlanması. |
| **Sistem Testleri ve Hata Giderme** | 2 Hafta | Tüm sistemin bir araya getirilerek gerçek ortamda test edilmesi ve olası hataların düzeltilmesi. |
| **Son Dokunuşlar ve Raporlama** | 1 Hafta | Kullanıcı dostu hale getirme, dökümantasyon hazırlama ve sunuma hazır hale getirme. |

## Kaynak Planlaması

### Ekip Üyeleri ve Görevleri:
   - **Batuhan Genç:** Donanım Tasarımı ve Entegrasyonu
   - **Habib Salim:** Api Entegrasyonu 
   - **Yücel Aytaç Akgün:** Mikrodenetleyici Programlanması
### Kullanılacak Ekipmanlar  

| **Ekipman** | **Görsel** |
|------------|-----------|
| ESP32 (İletişim ve kontrol) | <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/blob/main/%C3%96neri%20Raporu/Figure/ESP32.jpg" width="200"> |
| MAX9814 (Ses sensörü) | <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/blob/main/%C3%96neri%20Raporu/Figure/Max9814.jpg" width="200"> |
| LED (Uyarı sistemi) | <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/blob/main/%C3%96neri%20Raporu/Figure/Led.jpg" width="200"> |
| Dirençler(Temsili Resim) | <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/blob/main/%C3%96neri%20Raporu/Figure/Direnc.jpg" width="200"> |
| Breadboard | <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/blob/main/%C3%96neri%20Raporu/Figure/Breadboard.jpg" width="200"> |
| Jumper Kablolar | <img src="https://github.com/Aytacus/Sessiz-Kutuphaneler/blob/main/%C3%96neri%20Raporu/Figure/jumper.jpg" width="200"> |

### Kullanılacak Yazılımlar
- **Telegram Api**: API işlemleri ve bildirim göndermek için
- **Arduino IDE**: Mikrodenetleyici programlama için


### Risk Analizi  

| **Risk** | **Önlemler** |
|----------|--------------|
| **Ses Sensörünün Hassasiyeti Yetersiz Olabilir** | Sensör doğru konumlandırılmalı ve kalibrasyon yapılmalı.|
| **LED ve Bildirim Sistemi Senkronizasyon Problemleri** | Kod düzenli olarak test edilmeli ve hata ayıklama (debugging) süreci uygulanmalı. Zamanlama algoritmaları optimize edilmeli. |
| **Donanım Aşırı Isınma ve Arızalar** | Uygun soğutma yöntemleri (ısı dağıtıcılar, fanlar) düşünülmeli. Düzenli bakım ve testler yapılmalı. |
| **ESP32'nin Yetersiz Kalması** | Başka bir yol bularak yazılım ile Donanım optimizasyonu arttırmak. |
| **Veri Kaybı veya Geç Bildirimler** | Bildirim sisteminde hata yakalama mekanizmaları eklenmeli. Veriler yedeklenmeli ve mesajlar tekrar gönderme algoritmalarıyla güçlendirilmeli. |
| **Kod Hataları ve Yazılım Çökmesi** | Kod düzenli olarak test edilmeli, hata ayıklama süreçleri uygulanmalı. Bellek sızıntılarını önlemek için yazılım optimizasyonu yapılmalı. |

## Ticari Planlaması
 Bu sistem sayesinde artık Kütüphaneler daha sessiz olabilecektir. Ayrıca bu sistem daha da geliştirilerek akıllı Kütüphaneler oluşturulabilir.


## Katkıda Bulunanlar
* **Batuhan Genç** - [BatuhanGenç](https://github.com/Batuhaka)
* **Habib Salim** - [HabibSalim](https://github.com/habibsalimov)
* **Yücel Aytaç Akgün** - [YücelAytaçAkgün](https://github.com/Aytacus)

