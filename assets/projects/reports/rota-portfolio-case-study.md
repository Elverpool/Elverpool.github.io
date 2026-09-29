# ROTA — EPS Üretim Operasyon ve İzlenebilirlik Platformu

> Portfolyo sayfasına aktarılmaya hazır proje metni. Gerçek kullanıcı, müşteri,
> üretim ve finansal verileri içermez.

## Proje kartı

**ROTA — Fabrika Operasyon ve İzlenebilirlik Sistemi**

EPS üretiminde siparişten hammadde kabulüne, saha operasyonlarından paket ve
sevkiyata kadar uzanan süreçleri tek bir web uygulamasında birleştirdim. ROTA;
üretim ve stok kayıtlarını varyant düzeyinde izleyerek saha ekipleri, muhasebe
ve yönetim için ortak bir operasyon görünümü sunuyor.

**Etiketler:** Product Design · Full-Stack Development · .NET · PostgreSQL ·
Manufacturing Operations · Traceability

## Vaka çalışması

### 01 — Problem

Bir EPS fabrikasında sipariş, üretim, hammadde, ürün stoğu, paketleme ve
sevkiyat bilgileri farklı operasyon adımlarına ve takip dosyalarına dağılabilir.
Bu durumda toplam stok görünse bile stoğun hangi yoğunluk, renk, ölçü veya
paketlerden oluştuğunu bulmak; bir ürün paketini üretildiği blok ve kullanılan
hammadde lotuna kadar takip etmek zorlaşır.

ROTA’yı bu akışları aynı kayıt zincirinde buluşturmak ve fiziksel sahadaki
işlemleri denetlenebilir kayıtlara dönüştürmek için geliştirdim.

### 02 — Hedef

- Siparişten sevkiyata kadar üretim akışını tek uygulamada görünür kılmak.
- Hammadde, blok, bitmiş ürün ve ambalaj stoğunu ayrı ayrı ve ayrıntılı izlemek.
- Her paket için benzersiz kimlik oluşturarak ürün izlenebilirliği sağlamak.
- Operatör ekranlarını rol ve istasyon sorumluluklarına göre sadeleştirmek.
- Muhasebe ve yöneticilere karar desteği sunarken tahminleri gerçekleşmiş sonuç
  gibi göstermemek.

### 03 — Ürün kapsamı

**Sipariş ve üretim planlama.** Müşteri siparişleri ve terminler takip edilir;
açık talep, mevcut stok ve üretim ihtiyacı iş emri akışına bağlanır.

**Hammadde kabulü ve kalite.** Tedarikçi, hammadde, lot, irsaliye, Big Bag
adedi ve kilogram miktarı birlikte kaydedilir. Gelen e-İrsaliye HTML dosyası
okunarak kabul formu ön doldurulabilir; kullanıcı lot kabulünü onaylamadan stok
değişmez. Bozuk Big Bag’ler gerekçe ile stoktan düşülebilir ve işlem hareket
kaydında izlenir.

**Üretim zinciri.** Şişirme, silo, bloklama, kesim ve paketleme adımları iş
emriyle ilişkilendirilir. Blok ve paketler için saha/iş bilgileri ile kalıcı
kodlar saklanır; kesim ve paketleme sırasında beklenen adet ile sahada sayılan
adet karşılaştırılır.

**Ayrıntılı ürün stoğu.** Blok ve paket miktarları yalnızca toplam adet olarak
değil; ürün, yoğunluk (DNS), renk ve ölçü kırılımında gösterilir. Örneğin
“40 adet beyaz, 16 DNS blok” bilgisinin farklı ölçülerdeki stok dağılımı ayrı
satırlarda görülebilir.

**Paket ve sevkiyat takibi.** Fiziksel paketlere benzersiz QR kimliği verilir.
Sevkiyat sırasında paketler okutularak yükleme listesine eklenir; müşteri,
araç, sürücü, irsaliye, çıkış ve teslim bilgileri kayıt altına alınır.

**Ambalaj ve muhasebe kayıtları.** Poşet/ambalaj girişleri, üretim tüketimi,
stok düzeltmeleri, tedarikçi ve belge bilgileriyle kilogram bazında takip
edilir. Fatura para birimi ve fiyatı ile gerektiğinde TL kuru kaydedilebilir.

**Yönetim ve raporlama.** Dashboard üretim çıktısı, duruş, kalite kaybı,
hammadde ve paket stoğu, termin riski ve izlenebilirlik göstergelerini bir
arada sunar. Raporlar Excel veya yazdırılabilir çıktı olarak alınabilir; stok
karar ekranı satın alma önerileri, tahmini stok günleri, sipariş rezervasyonu
ve belge kontrollerini öne çıkarır.

### 04 — Ürün kararları

- **Fiziksel kayıt önceliği:** e-İrsaliye veri girişi hızlandırır; tek başına
  stok hareketi yaratmaz. Operatör veya muhasebe kullanıcısı doğrulayıp kabul
  ettikten sonra kayıt oluşur.
- **Toplam yerine varyant:** stok, karar vermeye yetecek özelliklerle
  gruplanır; blok ve paket sayıları ölçü, renk ve yoğunlukla birlikte görünür.
- **Silme yerine iz bırakma:** operasyon kayıtlarında yanlışlık olduğunda
  gerekçeli düzeltme veya ters hareket yaklaşımı kullanılır.
- **Tahmini sonuçları açık etiketleme:** dashboard'daki ROI ve tasarruf
  hesapları maliyet varsayımlarına dayalı tahminlerdir. Karşılaştırılabilir
  saha verisi olmadan gerçekleşmiş tasarruf olarak sunulmaz.

### 05 — Teknik uygulama

Uygulamayı ASP.NET Core 10 ve Blazor Server ile geliştirdim. Veriler Entity
Framework Core ve PostgreSQL üzerinde tutuluyor; QR kodları paket ve blok
izlenebilirliğinde kullanılıyor. Üretim, muhasebe, sevkiyat, stok ve yönetim
ekranları rol tabanlı yetkilendirme ile ayrılıyor.

Dağıtım hedefi Ubuntu üzerinde Docker Compose. CI akışı uygulama imajını
oluşturup özel container registry'ye yayımlayacak şekilde tasarlandı; Ubuntu
sunucusu kaynak koddan derleme yapmak yerine digest ile sabitlenmiş imajı
indirip çalıştırıyor. HTTPS, veritabanı ağının uygulama ağıyla sınırlandırılması
ve şifreli yedekleme üretim kurulumunun güvenlik yaklaşımının parçalarıdır.

### 06 — Sonuç ve kapsam

ROTA; üretim adımlarını, varyant bazlı stokları, belge girişlerini ve sevkiyat
izini ortak bir veri modelinde bir araya getiriyor. Böylece operasyon ekranları
günlük işleme, yönetim ekranları ise risk ve planlama kararlarına odaklanabiliyor.

Portfolyoda kullanılacak örnek kayıtlar sentetiktir. Bu çalışma için
doğrulanmış üretim verimliliği, maliyet tasarrufu veya ROI yüzdesi
bulunmadığından nicel başarı iddiası eklenmemiştir.

## Benim katkım

İş akışlarını ve kullanıcı rollerini modelledim; üretim, stok ve sevkiyat
süreçlerini ürün gereksinimlerine dönüştürerek web uygulamasını, veri
yapılarını, raporlama ekranlarını ve Ubuntu dağıtım akışını geliştirdim.

## Teknolojiler

`ASP.NET Core 10` · `Blazor Server` · `C#` · `Entity Framework Core` ·
`PostgreSQL` · `Docker Compose` · `GitHub Actions` · `QR` · `Excel`

---

### Site kartı için daha kısa alternatif

EPS üretiminin sipariş, hammadde kabulü, üretim, varyant bazlı stok, paketleme
ve sevkiyat süreçlerini tek bir operasyon platformunda birleştirdim. ROTA,
Big Bag/lot hareketlerini ve her QR kodlu ürünü üretimden teslimata kadar
izleyerek saha ve yönetim ekiplerine ortak bir görünüm sunuyor.

`ASP.NET Core` `Blazor Server` `PostgreSQL` `Manufacturing` `Traceability`
