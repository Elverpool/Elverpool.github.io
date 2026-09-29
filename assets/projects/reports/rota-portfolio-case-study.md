# ROTA — EPS Üretim Operasyon ve İzlenebilirlik Platformu

## İhtiyaç

EPS üretiminde hammadde kabulünden sevkiyata kadar pek çok adım var; ancak bu adımların bilgisi çoğu zaman kâğıt formlara, Excel dosyalarına ve operatör notlarına dağılabiliyor. Böyle olunca bir bloğun hangi hammadde lotundan üretildiğini bulmak, fire artışının hangi vardiyada başladığını anlamak veya stoktaki ürünleri ölçülerine göre saymak zaman alıyor. Toplam stok miktarı tek başına yeterli değil: örneğin 16 DNS beyaz blok stoğunun kaçının 100×114×150, kaçının 110×122×155 ölçüsünde olduğu da bilinmeli. İrsaliye bilgilerinin tekrar girilmesi ve bozuk Big Bag’lerin stoktan nasıl düşüldüğünün izlenmesi de ayrı iş yükü oluşturuyor.

## Yaklaşımım

Bu dağınık kayıtları tek bir üretim akışında birleştirmek için ROTA’yı geliştirdim. Sistem; iş emri ve reçete kontrolünden hammadde girişine, şişirme ve silo takibinden bloklama, kesim, paketleme ve sevkiyata kadar ilerliyor. Her görev için işe uygun ekranlar ve yetkiler bulunuyor; bilgiler işlemin yapıldığı aşamada kaydediliyor. Böylece amaç yalnızca dijital form sunmak değil, bir aşamada oluşan kaydın sonraki aşamalarda da kullanılmasını sağlamak.

## Sorunlardan çözümlere

**İrsaliye ve hammadde kayıtları tekrarlıydı.** Desteklenen e-İrsaliye HTML dosyaları kabul formundaki alanları ön dolduruyor. Kullanıcı bilgileri kontrol edip onaylamadan stok hareketi oluşmuyor. Hammadde; tedarikçi, lot, Big Bag ve kilogram bilgileriyle kaydediliyor. Bozuk olduğu bildirilen Big Bag’ler seçilip gerekçeli bir stok hareketiyle çıkarılabiliyor; böylece düzeltmenin ne olduğu ve neden yapıldığı kayıtlı kalıyor.

**Üretim geçmişini geriye doğru kurmak zordu.** ROTA’da malzeme lotu silo ve üretim kayıtlarıyla ilişkilendiriliyor; bloklara `BLK_...` biçiminde kalıcı kimlik ve sahada görülebilen alan numarası veriliyor. Kesim, fire, kalite ve paketleme kayıtları bu kimliğe bağlanıyor. Paketler için oluşturulan QR kodları da ürün etiketinde ve sevkiyatta kullanılabiliyor. Bu yapı, bir paketten geriye doğru kullanılan hammaddeye; bir bloktan ileriye doğru kesim ve paket bilgilerine ulaşmayı destekliyor.

**Stok toplamları ürünün gerçek dağılımını göstermiyordu.** Bitmiş ürün stoğu yoğunluk, renk ve ölçü boyutlarında; paketler de kendi ürün bilgileri ve adetleriyle görüntüleniyor. Böylece “toplam 40 adet” bilgisinin altındaki ölçü kırılımları da takip edilebiliyor. Ambalaj malzemeleri için giriş, üretimde tüketim ve düzeltme hareketleri; tedarikçi ve belge bilgileriyle birlikte kaydediliyor.

**Yönetim raporları için veriyi tekrar toplamak gerekiyordu.** Dashboard ve raporlar üretim, vardiya, fire, duruş, iş emri, stok ve izlenebilirlik kayıtlarını bir araya getiriyor. Denetim geçmişi, kimin hangi işlemi ne zaman ve hangi gerekçeyle yaptığını saklıyor; hatalı kayıtlar gerekçeli düzeltmelerle ele alınıyor. Böylece sahadaki operasyon kayıtları planlama ve değerlendirme için kullanılabilir bir görünüme kavuşuyor.

## Kapsam ve sınırlar

ROTA’daki 3B Dijital İkiz görünümü, iş emri, silo ve blok gibi operasyon kayıtlarını mekânsal bir arayüzde gösteriyor. Bugünkü hali PLC veya sensörlerden canlı veri alan fiziksel bir fabrika simülasyonu değil; makine telemetrisi ve fiziksel taşıma/konum takibi ileri aşama geliştirme alanları olarak ayrılıyor.

Uygulamayı Ubuntu sunucuda Docker Compose ile çalıştırılabilecek şekilde hazırlıyorum. Teknik temel: ASP.NET Core 10, Blazor Server, C#, Entity Framework Core ve PostgreSQL.

**Veri ve sonuç notu:** Portfolyoda gösterilecek örnek kayıtlar sentetiktir. Gerçek fabrika verisi, doğrulanmış tasarruf, üretim artışı veya gerçekleşmiş ROI sonucu paylaşılmamaktadır; dashboard ve ROI senaryoları operasyon verilerinden değerlendirme yapmaya yöneliktir.

## Daha kısa proje kartı

EPS üretiminde kâğıt, Excel ve birbirinden kopuk operasyon kayıtları; stok dağılımını görmeyi ve bir ürünün hammadde lotundan sevkiyatına kadar izini sürmeyi zorlaştırıyordu. ROTA ile hammadde kabulü, üretim, kesim, paketleme ve sevkiyat kayıtlarını tek akışta birleştirdim. Sistem; ölçü ve ürün özelliklerine göre detaylı stok görünümü, gerekçeli stok hareketleri, blok/paket kimlikleri ve QR destekli izlenebilirlik sunuyor.
