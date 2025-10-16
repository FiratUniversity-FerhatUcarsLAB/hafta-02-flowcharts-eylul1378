ATM Para Çekme Sistemi Pseudocode

BAŞLA

// --- Değişken Tanımlamaları ---

SAYISAL pin_hakki = 3

SAYISAL dogru_pin = 1234 // Gerçek sistemde bu bilgi şifreli olarak saklanır

SAYISAL bakiye = 5000 // Kullanıcının mevcut bakiyesi

SAYISAL gunluk_limit = 3000 // Kullanıcının kalan günlük çekim limiti

SAYISAL girilen_pin

SAYISAL cekilecek_tutar

MANTIKSAL pin_dogrulandi = YANLIŞ

KARAKTER devam_etme_secimi = 'E'

// --- PIN Doğrulama Döngüsü ---

DÖNGÜ (pin_hakki > 0 VE pin_dogrulandi == YANLIŞ OLDUĞU SÜRECE)

    YAZ "Lütfen 4 haneli ATM şifrenizi giriniz:"

    OKU girilen_pin

    EĞER girilen_pin == dogru_pin İSE

        pin_dogrulandi = DOĞRU

        YAZ "Giriş başarılı."

    DEĞİLSE

        pin_hakki = pin_hakki - 1

        EĞER pin_hakki > 0 İSE

            YAZ "Hatalı şifre girdiniz. Kalan deneme hakkınız: ", pin_hakki

    DEĞİLSE

            YAZ "3 kez hatalı giriş yaptınız. Güvenlik nedeniyle kartınız bloke edilmiştir."

        SON // Programı sonlandır

  DÖNGÜ SONU
BİTİR

// --- Ana İşlem Döngüsü ---

DÖNGÜ (pin_dogrulandi == DOĞRU VE (devam_etme_secimi == 'E' VEYA devam_etme_secimi == 'e') OLDUĞU SÜRECE)

    YAZ "--------------------------------"

    YAZ "Mevcut Bakiyeniz: ", bakiye, " TL"

    YAZ "Günlük Kalan Çekim Limitiniz: ", gunluk_limit, " TL"

    YAZ "Lütfen çekmek istediğiniz tutarı giriniz (20 TL ve katları olmalıdır):"

    OKU cekilecek_tutar

    // --- Tutar Kontrolleri ---

    EĞER cekilecek_tutar % 20 != 0 İSE

        YAZ "HATA: Girdiğiniz tutar 20 TL ve katları olmalıdır. Lütfen tekrar deneyin."

    DEĞİLSE EĞER cekilecek_tutar > bakiye İSE

        YAZ "HATA: İşlem için bakiyeniz yetersiz."

    DEĞİLSE EĞER cekilecek_tutar > gunluk_limit İSE

        YAZ "HATA: Talep ettiğiniz tutar, günlük çekim limitinizi aşıyor."

    DEĞİLSE // Tüm kontroller başarılı ise

        // --- İşlemi Gerçekleştir ---

        bakiye = bakiye - cekilecek_tutar

        gunluk_limit = gunluk_limit - cekilecek_tutar

        YAZ "İşleminiz başarıyla tamamlandı. Lütfen paranızı alınız."

        YAZ "Yeni Bakiyeniz: ", bakiye, " TL"

        YAZ "Günlük Kalan Çekim Limitiniz: ", gunluk_limit, " TL"

    DÖNGÜ SONU

BİTİR

    // --- Yeni İşlem Sorgusu ---

    YAZ "--------------------------------"

    YAZ "Başka bir işlem yapmak ister misiniz? (Evet için 'E', Hayır için 'H' tuşuna basınız)"

    OKU devam_etme_secimi

DÖNGÜ SONU

// --- Çıkış Mesajı ---

YAZ "Bankamızı tercih ettiğiniz için teşekkür ederiz. İyi günler dileriz."

BİTİR
