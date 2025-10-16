FONKSIYON HastaneBilgiSistemi() 
    // ----------------------------------------------------
    // Adım 1: Başlangıç ve Kimlik Doğrulama
    // ----------------------------------------------------
    YAZDIR("Hastane Bilgi Sistemine Hoş Geldiniz.")
    YAZDIR("Lütfen T.C. Kimlik Numaranızı (TC No) giriniz:")
    OKU(tc_no)
    
    EĞER KimlikDogrulama(tc_no) == BAŞARILI İSE
        YAZDIR("Kimlik doğrulama başarılı.")
        
        // Ana İşlem Döngüsü Başlangıcı (Sürekli Çalışma)
        DONGU_DEVAM_ET = DOĞRU
        DONGU SÜRESİNCE (DONGU_DEVAM_ET)
            
            // ----------------------------------------------------
            // Adım 2: Ana İşlem Seçimi (Koşul)
            // ----------------------------------------------------
            YAZDIR("\n--- Ana Menü ---")
            YAZDIR("1: Randevu Al")
            YAZDIR("2: Tahlil Sonucu Gör")
            YAZDIR("3: Çıkış")
            YAZDIR("Lütfen bir işlem seçiniz (1-3):")
            OKU(ana_secim)

            // ----------------------------------------------------
            // Modül A: Randevu Modülü
            // ----------------------------------------------------
            EĞER ana_secim == 1 İSE
                YAZDIR("\n--- Randevu Alma Modülü ---")
                
                // Poliklinik Seçimi (Döngü)
                POLIKLINIK_SECİMİ_BAŞARILI = YANLIŞ
                DONGU SÜRESİNCE (POLIKLINIK_SECİMİ_BAŞARILI == YANLIŞ)
                    YAZDIR("Lütfen randevu almak istediğiniz polikliniği seçiniz:")
                    POLIKLINIK_LISTESI_GETIR()
                    OKU(secilen_poliklinik)
                    EĞER PoliklinikGeçerliMi(secilen_poliklinik) İSE
                        POLIKLINIK_SECİMİ_BAŞARILI = DOĞRU
                    AKSİ HALDE
                        YAZDIR("Geçersiz seçim.")
                    SON_EĞER
                SON_DONGU
                
                // Doktor Listesi Görüntüleme
                YAZDIR("Uygun doktorlar listeleniyor...")
                doktor_listesi = DoktorlariGetir(secilen_poliklinik)
                YAZDIR(doktor_listesi)
                OKU(secilen_doktor_id)

                // Uygun Saatleri Gösterme (Döngü)
                uygun_saatler = UygunSaatleriGetir(secilen_doktor_id)
                EĞER uygun_saatler BOŞ DEĞİL İSE
                    YAZDIR("\nUygun Saatler:")
                    YAZDIR(uygun_saatler)
                    YAZDIR("Lütfen bir tarih ve saat seçiniz:")
                    OKU(secilen_saat_tarih)
                    
                    EĞER RandevuKaydet(tc_no, secilen_doktor_id, secilen_saat_tarih) == BAŞARILI İSE
                        // Randevu Onayı ve SMS Gönderimi
                        YAZDIR("Randevunuz başarıyla oluşturuldu ve onaylandı.")
                        SMS_Gonder(tc_no, "Randevu onay bilgisi: " + secilen_saat_tarih)
                    AKSİ HALDE
                        YAZDIR("Randevu kaydı başarısız oldu.")
                    SON_EĞER
                AKSİ HALDE
                    YAZDIR("Seçilen doktor için uygun saat bulunmamaktadır.")
                SON_EĞER
            
            // ----------------------------------------------------
            // Modül B: Tahlil Sonucu Görüntüleme Modülü
            // ----------------------------------------------------
            EĞER ana_secim == 2 İSE
                YAZDIR("\n--- Tahlil Sonucu Görüntüleme Modülü ---")
                
                // Tahlil Varlığı Kontrolü (Koşul)
                EĞER TahlilVarMi(tc_no) == EVET İSE
                    
                    // Sonuç Hazır Mı Kontrolü (Koşul)
                    EĞER SonuclarHazirMi(tc_no) == EVET İSE
                        // Sonuç Görüntüleme
                        tahlil_sonuclari = TahlilSonuclariniGetir(tc_no)
                        YAZDIR("\n--- Tahlil Sonuçlarınız ---")
                        YAZDIR(tahlil_sonuclari) 
                        
                        // PDF İndir Seçeneği
                        YAZDIR("\nSonuçları PDF olarak indirmek ister misiniz? (E/H)")
                        OKU(pdf_secim)
                        EĞER pdf_secim == "E" VEYA pdf_secim == "e" İSE
                            PDF_Olustur_Ve_Indir(tahlil_sonuclari)
                            YAZDIR("PDF başarıyla oluşturuldu ve indirildi.")
                        SON_EĞER
                    AKSİ HALDE
                        // Bekleme Mesajı
                        YAZDIR("Tahlil sonuçlarınız henüz hazır değildir. Lütfen daha sonra kontrol ediniz.")
                    SON_EĞER
                AKSİ HALDE
                    YAZDIR("Adınıza kayıtlı beklemede veya tamamlanmış tahlil kaydı bulunmamaktadır.")
                SON_EĞER
            
            // ----------------------------------------------------
            // Adım 3: Çıkış ve Genel Döngü Kontrolü
            // ----------------------------------------------------
            EĞER ana_secim == 3 İSE
                YAZDIR("Sistemden çıkılıyor. İyi günler dileriz.")
                DONGU_DEVAM_ET = YANLIŞ // Ana döngüyü sonlandır
            AKSİ HALDE EĞER ana_secim == 1 VEYA ana_secim == 2 İSE
                // Başka işlem yapmak ister misiniz? (Döngü)
                YAZDIR("\nBaşka bir işlem yapmak ister misiniz? (E/H)")
                OKU(tekrar_secim)
                EĞER tekrar_secim == "H" VEYA tekrar_secim == "h" İSE
                    YAZDIR("Sistemden çıkılıyor. İyi günler dileriz.")
                    DONGU_DEVAM_ET = YANLIŞ
                SON_EĞER
            AKSİ HALDE
                YAZDIR("Geçersiz seçim. Lütfen 1, 2 veya 3 giriniz.")
            SON_EĞER

        SON_DONGU // DONGU SÜRESİNCE (DONGU_DEVAM_ET)
        
    AKSİ HALDE
        YAZDIR("Kimlik doğrulama başarısız. Lütfen T.C. Kimlik Numaranızı kontrol ediniz.")
    SON_EĞER

SON_FONKSIYON

// --- Yardımcı Fonksiyonlar (Açıklama Amaçlı) ---
// Not: Bu fonksiyonların gerçek kodları sistem gereksinimlerine göre yazılmalıdır.

FONKSIYON KimlikDogrulama(tc_no) // GERİ_DÖN BAŞARILI/BAŞARISIZ
FONKSIYON POLIKLINIK_LISTESI_GETIR() 
FONKSIYON PoliklinikGeçerliMi(isim) // GERİ_DÖN EVET/HAYIR
FONKSIYON DoktorlariGetir(poliklinik) // GERİ_DÖN doktor_listesi
FONKSIYON UygunSaatleriGetir(doktor_id) // GERİ_DÖN saat_tarih_listesi
FONKSIYON RandevuKaydet(tc_no, doktor_id, saat_tarih) // GERİ_DÖN BAŞARILI/BAŞARISIZ
FONKSIYON SMS_Gonder(tc_no, mesaj)
FONKSIYON TahlilVarMi(tc_no) // GERİ_DÖN EVET/HAYIR
FONKSIYON SonuclarHazirMi(tc_no) // GERİ_DÖN EVET/HAYIR
FONKSIYON TahlilSonuclariniGetir(tc_no) // GERİ_DÖN sonuç_detayları
FONKSIYON PDF_Olustur_Ve_Indir(sonuclar)
