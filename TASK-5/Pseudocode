// --- Değişkenlerin ve Durumların Başlatılması ---

BAŞLA

  // Sistem Durumları
  DEĞİŞKEN sistem_durumu = AKTİF // (AKTİF veya PASİF olabilir)
  DEĞİŞKEN alarm_durumu = PASİF // (AKTİF veya PASİF olabilir)
  DEĞİŞKEN ev_sahibi_evde = YANLIŞ // (DOĞRU veya YANLIŞ olabilir, coğrafi konum veya manuel olarak belirlenir)

  // Sensör Değerleri
  DEĞİŞKEN hareket_algilandi
  DEĞİŞKEN kapi_pencere_acik
  
// --- ANA SİSTEM DÖNGÜSÜ (7/24 Çalışır) ---
SÜREKLİ DÖNGÜ

  // 1. ADIM: Sistemin kurulu olup olmadığını kontrol et
  EĞER sistem_durumu == AKTİF İSE
  
    // 2. ADIM: Sensörleri Sürekli Olarak Oku
    hareket_algilandi = oku_hareket_sensoru()
    kapi_pencere_acik = oku_kapi_pencere_sensoru()
    
    // 3. ADIM: Tehdit Algılama (Sensörlerden biri tetiklendi mi?)
    EĞER hareket_algilandi == DOĞRU VEYA kapi_pencere_acik == DOĞRU İSE
    
      // 4. ADIM: Yanlış Alarm Kontrolü (Ev sahibi evde mi?)
      ev_sahibi_evde = kontrol_et_ev_sahibi_konumu() // Konum servisi veya manuel ayarı kontrol et
      
      EĞER ev_sahibi_evde == YANLIŞ İSE
      
        // TEHDİT DOĞRULANDI, ALARM SÜRECİNİ BAŞLAT
        EĞER alarm_durumu == PASİF İSE // Alarm zaten çalmıyorsa başlat
        
          YAZ "Tehdit Algılandı! Alarm prosedürü başlatılıyor."
          alarm_durumu = AKTİF
          
          // 5. ADIM: Alarm Seviyesini Belirle
          DEĞİŞKEN alarm_seviyesi
          EĞER kapi_pencere_acik == DOĞRU İSE
            alarm_seviyesi = 3 // Yüksek Öncelik: Kapı/Pencere ihlali
          DEĞİLSE // Sadece hareket algılandıysa
            alarm_seviyesi = 2 // Orta Öncelik: Hareket algılandı
          BİTİR EĞER
          
          // 6. ADIM: Alarm ve Bildirim Eylemlerini Gerçekleştir
          siren_cal(alarm_seviyesi)
          kamera_kaydi_baslat()
          
          // 7. ADIM: Bildirimleri Gönder
          bildirim_gonder("SMS", "ALARM: Evde hareket veya kapı/pencere ihlali algılandı! Seviye: " + alarm_seviyesi)
          bildirim_gonder("Uygulama", "GÜVENLİK UYARISI: Tehdit algılandı. Kameraları kontrol edin.")
          bildirim_gonder("Email", "Acil Durum: Güvenlik sisteminiz tetiklendi.")
          
        BİTİR EĞER
        
      DEĞİLSE // Ev sahibi evdeyse yanlış alarmdır
        YAZ "Sensör tetiklendi ancak ev sahibi evde. Yanlış alarm olarak yoksayıldı."
      BİTİR EĞER
      
    DEĞİLSE // Sensörlerde bir hareketlilik yoksa
    
      // 8. ADIM: Alarm Sıfırlama (Eğer tehdit ortadan kalktıysa ve sistem manuel sıfırlanmadıysa)
      EĞER alarm_durumu == AKTİF İSE
        // Bu senaryoda alarm, kullanıcı müdahalesi (örneğin uygulamadan kapatma)
        // ile sıfırlanana kadar çalmaya devam eder.
        YAZ "Tehdit durumu devam ediyor. Alarm aktif."
      DEĞİLSE
        YAZ "Sistem normal, herhangi bir tehdit yok."
      BİTİR EĞER
      
    BİTİR EĞER
    
  DEĞİLSE // Sistem kurulu (aktif) değilse
    YAZ "Sistem pasif modda. Bekleniyor..."
    // Sistem pasifken çalan bir alarm varsa durdur
    EĞER alarm_durumu == AKTİF İSE
      alarm_durdur()
      kamera_kaydi_durdur()
      alarm_durumu = PASİF
      YAZ "Sistem kullanıcı tarafından devre dışı bırakıldı. Alarm susturuldu."
    BİTİR EĞER
    
  BİTİR EĞER
  
  //
