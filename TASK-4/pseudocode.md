BAŞLA

  // --- Değişken Tanımlamaları ---
  
  DEĞİŞKEN ogrenciNo, sifre : METİN
  DEĞİŞKEN girisBasarili : MANTIKSAL = YANLIŞ
  DEĞİŞKEN kayitDevamEdiyor : MANTIKSAL = DOĞRU
  DEĞİŞKEN islemSecimi, dersKodu : METİN
  
  DEĞİŞKEN acilanDerslerListesi : DİZİ // Tüm açılan derslerin tutulduğu liste
  DEĞİŞKEN secilenDerslerListesi : DİZİ // Öğrencinin seçtiği derslerin tutulduğu liste
  DEĞİŞKEN ogrencininTranskripti : DİZİ // Öğrencinin geçmiş ders ve not bilgileri
  
  DEĞİŞKEN toplamKredi : TAMSAYI = 0
  DEĞİŞKEN KREDI_LIMITI : TAMSAYI = 35
  DEĞİŞKEN GPA_LIMITI : ONDALIKLI = 2.5
  DEĞİŞKEN ogrenciGPA : ONDALIKLI
  
  // --- 1. Adım: Öğrenci Girişi ---

  GÖSTER "Öğrenci Numaranızı Giriniz:"
  OKU ogrenciNo
  GÖSTER "Şifrenizi Giriniz:"
  OKU sifre
  
  // Veritabanından öğrenci bilgileri doğrulanır ve bilgileri çekilir
  EĞER (Veritabanı_KullanıcıDogrula(ogrenciNo, sifre)) İSE
    girisBasarili = DOĞRU
    ogrenciGPA = Veritabanı_GPA_Getir(ogrenciNo)
    ogrencininTranskripti = Veritabanı_TranskriptGetir(ogrenciNo)
    GÖSTER "Giriş başarılı. Ders kayıt sistemine hoş geldiniz."
  DEĞİLSE
    GÖSTER "Hatalı öğrenci numarası veya şifre. Program sonlandırılıyor."
    SONLANDIR
  EĞER BİTTİ

  // --- 2. Adım: Ders Ekleme/Çıkarma Ana Döngüsü ---

  EĞER (girisBasarili) İSE
    SÜRECE (kayitDevamEdiyor) DÖNGÜSÜNE GİR
      GÖSTER "--- Ders Kayıt Ekranı ---"
      GÖSTER "Açılan Dersler:"
      YAZDIR(acilanDerslerListesi) // Açılan tüm dersleri listele
      
      GÖSTER "Seçtiğiniz Dersler:"
      YAZDIR(secilenDerslerListesi) // Seçili dersleri listele
      GÖSTER "Toplam Kredi: " + toplamKredi
      
      GÖSTER "Yapmak istediğiniz işlem: (1: Ders Ekle, 2: Ders Çıkar, 3: Kaydı Bitir)"
      OKU islemSecimi
      
      // --- Ders Ekleme Bloğu ---
      EĞER (islemSecimi == "1") İSE
        GÖSTER "Eklemek istediğiniz dersin kodunu giriniz:"
        OKU dersKodu
        hedefDers = acilanDerslerListesi.Bul(dersKodu)
        
        // --- İÇ İÇE KONTROLLER BAŞLIYOR ---
        
        // KONTROL 1: Kredi Limiti
        EĞER (toplamKredi + hedefDers.kredi <= KREDI_LIMITI) İSE
          
          // KONTROL 2: Zaman Çakışması
          EĞER (ZamanCakismasiVarMi(hedefDers, secilenDerslerListesi) == YANLIŞ) İSE
            
            // KONTROL 3: Ön Koşul
            EĞER (OnkosulSaglandiMi(hedefDers, ogrencininTranskripti) == DOĞRU) İSE
              
              // KONTROL 4: Kontenjan
              EĞER (hedefDers.kayitliOgrenci < hedefDers.kontenjan) İSE
                // TÜM KONTROLLER BAŞARILI
                secilenDerslerListesi.Ekle(hedefDers)
                toplamKredi = toplamKredi + hedefDers.kredi
                hedefDers.kayitliOgrenci = hedefDers.kayitliOgrenci + 1
                GÖSTER dersKodu + " kodlu ders başarıyla eklendi."
              DEĞİLSE // Kontenjan doluysa
                GÖSTER "HATA: Ders kontenjanı dolu."
              EĞER BİTTİ // Kontenjan kontrolü sonu
              
            DEĞİLSE // Ön koşul sağlanmadıysa
              GÖSTER "HATA: Bu ders için gerekli ön koşul dersini başarmamışsınız."
            EĞER BİTTİ // Ön koşul kontrolü sonu
            
          DEĞİLSE // Zaman çakışması varsa
            GÖSTER "HATA: Seçtiğiniz ders, programınızdaki başka bir dersle çakışıyor."
          EĞER BİTTİ // Zaman çakışması kontrolü sonu
          
        DEĞİLSE // Kredi limiti aşıldıysa
          GÖSTER "HATA: Bu dersi eklerseniz kredi limitini (" + KREDI_LIMITI + ") aşıyorsunuz."
        EĞER BİTTİ // Kredi limiti kontrolü sonu
        
      // --- Ders Çıkarma Bloğu ---
      DEĞİLSE EĞER (islemSecimi == "2") İSE
        GÖSTER "Çıkarmak istediğiniz dersin kodunu giriniz:"
        OKU dersKodu
        
        EĞER (secilenDerslerListesi.IceriyorMu(dersKodu)) İSE
          cikarilacakDers = secilenDerslerListesi.Bul(dersKodu)
          toplamKredi = toplamKredi - cikarilacakDers.kredi
          cikarilacakDers.kayitliOgrenci = cikarilacakDers.kayitliOgrenci - 1
          secilenDerslerListesi.Cikar(dersKodu)
          GÖSTER dersKodu + " kodlu ders listenizden çıkarıldı."
        DEĞİLSE
          GÖSTER "HATA: Bu ders zaten seçili değil."
        EĞER BİTTİ
        
      // --- Kaydı Bitirme Bloğu ---
      DEĞİLSE EĞER (islemSecimi == "3") İSE
        kayitDevamEdiyor = YANLIŞ // Döngüden çıkmak için
      
      DEĞİLSE // Geçersiz işlem seçimi
        GÖSTER "Geçersiz bir işlem seçtiniz. Lütfen tekrar deneyin."
      EĞER BİTTİ
      
    DÖNGÜ BİTTİ
    
  EĞER BİTTİ
  
  // --- 3. Adım: Kayıt Özeti ve Danışman Onayı Kontrolü ---

  GÖSTER "\n--- Kayıt Özeti ---"
  GÖSTER "Seçilen Dersler:"
  YAZDIR(secilenDerslerListesi)
  GÖSTER "Toplam Alınan Kredi: " + toplamKredi
  
  // KONTROL 5: Danışman Onayı Gerekli mi?
  EĞER (ogrenciGPA < GPA_LIMITI) İSE
    GÖSTER "UYARI: Not ortalamanız " + GPA_LIMITI + " altında olduğu için ders kaydınız danışman onayına gönderilmiştir."
    GÖSTER "Danışmanınız onayladıktan sonra kaydınız kesinleşecektir."
  DEĞİLSE
    GÖSTER "Tebrikler! Ders kaydınız başarıyla ve kesin olarak tamamlanmıştır."
  EĞER BİTTİ

SONLANDIR
