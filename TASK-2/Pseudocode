BAŞLA ADIM 1: KULLANICI GİRİŞ KONTROLÜ

KONTROL NOKTASI: Kullanıcı oturumu açık mı?

EĞER kullanıcı giriş yapmış İSE 
Sonraki adıma geç.
DEĞİLSE
Kullanıcıyı "Giriş Yap / Kayıt Ol" sayfasına yönlendir.
İşlemi durdur.

BİTİR ADIM 1

BAŞLA ADIM 2: ÜRÜN SEÇİMİ VE SEPETE EKLEME

DÖNGÜ (Sitedeki her bir ürün kategorisi için)

Kategoriyi ekranda göster.
BİTİR DÖNGÜ

Kullanıcı bir ürün seçer ve "Sepete Ekle" butonuna tıklar.

Seçilen Ürün ID'sini ve adedini al.

BİTİR ADIM 2

BAŞLA ADIM 3: STOK KONTROLÜ

KONTROL NOKTASI: "Sepete Ekle" butonuna tıklanan ürünün stok adedi > 0 mı?

EĞER stok mevcut İSE

 Ürünü, kullanıcının sepetine ekle.
 Kullanıcıya "Ürün sepete eklendi" mesajı göster.
 DEĞİLSE
 Kullanıcıya "Ürün stokta bulunmamaktadır" uyarısı göster.
 İşlemi durdur.

BİTİR ADIM 3

BAŞLA ADIM 4: SEPETİ GÖRÜNTÜLEME VE DÜZENLEME

Kullanıcı sepet sayfasına gider.

AraToplam = 0 olarak ayarla.

DÖNGÜ (Kullanıcının sepetindeki her bir ürün için)

 Ürünün bilgilerini (isim, fiyat, adet) ekranda göster.
 `AraToplam` = `AraToplam` + (Ürün Fiyatı * Ürün Adedi).
  BİTİR DÖNGÜ

Hesaplanan AraToplam değerini ekranda göster.

BİTİR ADIM 4

BAŞLA ADIM 5: İNDİRİM VE MİNİMUM TUTAR KONTROLÜ

Kullanıcı indirim kodu girebilir.

KONTROL NOKTASI: Kullanıcı "İndirim Kodunu Uygula" butonuna tıkladı mı?

EĞER butona tıklandı İSE

 **KONTROL NOKTASI:** Girilen kod veritabanında geçerli mi?
 **EĞER** kod geçerli **İSE**
     İndirim miktarını `AraToplam`'dan düş.
     Yeni toplamı `GenelToplam` olarak ata.
     "İndirim uygulandı" mesajı göster.
 **DEĞİLSE**
    "Geçersiz indirim kodu" uyarısı göster.
    `GenelToplam` = `AraToplam`.
  DEĞİLSE
`GenelToplam` = `AraToplam`.

KONTROL NOKTASI: GenelToplam >= 50 TL mi?
EĞER tutar 50 TL veya üzeri İSE
Sonraki adıma geç.
DEĞİLSE
Kullanıcıya "Minimum sepet tutarı 50 TL olmalıdır" uyarısı göster.
İşlemi durdur.

BİTİR ADIM 5

BAŞLA ADIM 6: KARGO HESAPLAMA

KONTROL NOKTASI: GenelToplam >= 200 TL mi?

EĞER tutar 200 TL veya üzeri İSE

 `KargoÜcreti` = 0.
 Ekranda "Ücretsiz Kargo" ibaresi göster.
 DEĞİLSE
 `KargoÜcreti` = Standart Kargo Fiyatı (Örn: 25 TL).
 SiparişToplamı = GenelToplam + KargoÜcreti.

SiparişToplamı'nı ekranda göster.,

BİTİR ADIM 6

BAŞLA ADIM 7: ÖDEME YÖNTEMİ SEÇİMİ

Kullanıcı bir ödeme yöntemi seçer (Kredi Kartı / Havale).

KONTROL NOKTASI: Seçilen yöntem hangisi?

EĞER seçilen yöntem "Kredi Kartı" İSE

 Kredi kartı bilgi giriş formunu göster.
 Bilgiler girildikten sonra bankadan ödeme onayı iste.
 **KONTROL NOKTASI:** Bankadan gelen yanıt "Başarılı" mı?
 **EĞER** yanıt "Başarılı" **İSE**
     `ÖdemeDurumu` = "Başarılı".
 **DEĞİLSE**
    `ÖdemeDurumu` = "Başarısız".
    Kullanıcıya hata mesajı göster ve işlemi durdur.
EĞER seçilen yöntem "Havale/EFT" İSE

Banka hesap bilgilerini ve sipariş numarasını ekranda göster.
`ÖdemeDurumu` = "Onay Bekliyor".

BİTİR ADIM 7

BAŞLA ADIM 8: SİPARİŞ ONAYI VE TAMAMLAMA

KONTROL NOKTASI: ÖdemeDurumu = "Başarılı" mı?

EĞER ödeme başarılı İSE

 Siparişi tüm detaylarıyla (kullanıcı, ürünler, tutar, adres) veritabanına kaydet.
 **DÖNGÜ** (Siparişteki her bir ürün için)
     Veritabanındaki ürün stoğunu, satılan adet kadar düşür.
 **BİTİR DÖNGÜ**
 Kullanıcıya "Siparişiniz başarıyla alınmıştır" mesajını ve sipariş numarasını göster.
 Kullanıcıya sipariş onay e-postası gönder.
DEĞİLSE

İşlemi bir hata mesajı ile sonlandır.

BİTİR ADIM 8
