# Sayı İzi — 0–19 Eldesiz İşlemler, Genişletilmiş Sayı Doğrusu, Evre Anahtarı ve Renkli Soru Yönergesi Planı

Bu güncelleme, **Sayı İzi** uygulamasındaki sayı doğrusunu 0'dan 19'a kadar genişletir, öğretmen panelinde 19'a kadar yalnız **eldesiz toplama** ve **onluk bozmasız çıkarma** işlemlerine izin veren akıllı doğrulama kuralları getirir, araştırma evrelerini (Başlama Düzeyi, Uygulama, İzleme, Genelleme) tek bir anahtarla (switch) kapatıp yalnızca geri bildirimli **Uygulama** modunda çalıştırma imkânı sunar ve öğrenci soru yönergesini renkli, ilgi çekici hale getirir.

## User Review & Critical Decisions

> [!IMPORTANT]
> Yanıtlarınız ve ek geri bildiriminiz doğrultusunda aşağıdaki kararlar planımıza dahil edilmiştir. Lütfen inceleyip onaylayın:

- **Onaylanan Karar 1 — 1–19 Arası Eldesiz Toplama ve Onluk Bozmasız Çıkarma**:
  - **Toplama (`+`)**: Sonucu en fazla `19` olan ve birler basamağındaki rakamların toplamı `9`'u aşmayan (`(num1 % 10) + (num2 % 10) <= 9` ve `num1 + num2 <= 19`) işlemler kabul edilir. Öğretmen eldeli bir işlem girdiğinde (ör. `8 + 5 = 13` veya `14 + 7 = 21`) sistem uyarı vererek eklemeyi engeller.
  - **Çıkarma (`−`)**: Eksilenin en fazla `19` olduğu, farkın negatif olmadığı ve onluk bozma gerektirmeyen (`(num1 % 10) >= (num2 % 10)`) işlemler kabul edilir (ör. `17 - 5 = 12`, `9 - 4 = 5`). Onluk bozmalı işlemler (ör. `14 - 6`) engellenir.
- **Onaylanan Karar 2 — Koyulacak Birimlerin 1–9 Rakam Temsili**:
  - Öğrencinin sayı doğrusu üzerinde sürüklediği ve adım adım saydığı birimler 1–9 arası rakam kimliğini (renk ve geometrik şekil kodlamasını) korur. İki basamaklı eldesiz işlemlerde (ör. `12 + 5 = 17` veya `17 - 5 = 12`) toplanan/çıkarılan birim 1–9 arası rakamdır; iki basamaklı sayılar da sayı doğrusunda kendi birler basamağı rakamının (1–9) görsel şekil/renk kodlamasıyla ve net rakam etiketiyle (`0`–`19` sayı doğrusu üzerinde) adım adım ilerler.
- **Onaylanan Karar 3 — Tek Switch ile Evreleri Kapatıp Sadece Uygulama Modunu Çalıştırma**:
  - Öğretmen paneline **"Tüm araştırma evrelerini kullan (Başlama · Uygulama · İzleme · Genelleme)"** anahtarı (switch) eklenir.
  - Bu anahtar **açıkken** (varsayılan): Mevcut 4 evreli yapı (A, B, C, D) ve 4 ayrı oturum kodu üretimi aynen çalışır.
  - Bu anahtar **kapalıyken**: Başlama Düzeyi (A), İzleme (C) ve Genelleme (D) adımları gizlenir; ekran otomatik olarak **Uygulama (B)** evresine kilitlenir. Öğretmen yalnızca Uygulama soruları hazırlar ve kaydettiğinde **sadece 1 adet Uygulama (geri dönütlü öğretim) oturum kodu** üretilir.
- **Onaylanan Karar 4 — Renkli ve Dikkat Çekici Öğrenci Soru Yönergesi**:
  - Öğrenci ekranında (`#prompt`) yer alan `"1. sorudaki işlemi yap"` ifadesi kaldırılarak yerine **`"1. Sorudaki sayıları sayı doğrusuna sürükle ve işlemi yap."`** yazılır.
  - Yönerge içindeki **"sayı doğrusuna sürükle"** (ör. canlı mavi/turuncu vurgu) ve **"işlemi yap"** (ör. canlı yeşil/mor vurgu) ifadeleri öğrencinin dikkatini çekecek farklı renklerle ve belirgin tipografiyle vurgulanır.

---

## 1. Overview & Core Concept

- **Ne Yapar**: Özel eğitim ve erken matematik öğretiminde öğrencilerin 0–19 sayı doğrusu üzerinde eldesiz toplama ve onluk bozmasız çıkarma işlemlerini somut-görsel adımlarla ve el yazısı (OCR) cevabıyla öğrenmesini sağlar.
- **Hedef Kitle**: Özel eğitim öğretmenleri, araştırmacılar ve erken matematik becerisi çalışan öğrenciler.
- **Temel Katkı**:
  1. Tek denekli araştırmalar dışındaki günlük sınıf içi öğretimlerde 4 evreyi birden doldurma zorunluluğunu tek bir switch ile kaldırarak doğrudan geri dönütlü **Uygulama** oturumu açabilme.
  2. Öğretmenin yanlışlıkla eldeli toplama veya onluk bozmalı çıkarma sorusu eklemesini anında önleyen pedagojik kural denetimi.
  3. 0'dan 19'a kadar kesintisiz, sesli Türkçe sayım ("on bir" … "on dokuz") ve zıplama kemeri destekli etkileşimli sayı doğrusu.
  4. Öğrenciye ne yapacağını adım adım ve renkli vurgularla anlatan (`"sayı doğrusuna sürükle"` ve `"işlemi yap"`) görsel yönerge.

---

## 2. User Experience & Visual Design

- **Temel Kullanıcı Akışları**:
  1. **Öğretmen — Evre Modu Seçimi**: Öğretmen "Öğretim Oluştur" sekmesinde öğrenci adını girdikten hemen sonra yer alan **"Tüm araştırma evrelerini kullan (A–B–C–D)"** switch'ini kapatırsa, 4'lü evre sekme çubuğu (`phaseSeg`) ve "4 Evreyi Birden Rastgele Doldur" butonu gizlenir; yalnızca **Uygulama** evresi aktif kalır.
  2. **Öğretmen — Soru Oluşturma & Eldesiz Denetimi**: Sayı seçiciler (`#tN1`, `#tN2`) seçilen işleme göre eldesiz sınırları denetler. Eldeli bir kombinasyon seçildiğinde sonuç kutusu (`#tAns`) kırmızı uyarı durumuna geçer ve "Soruyu Ekle" tıklandığında anlaşılır bir pedagojik uyarı mesajı (`"Eldeli toplama yapılamaz: Birler basamağı toplamı en fazla 9 olmalıdır."` veya `"Onluk bozmalı çıkarma yapılamaz: İlk sayının birler basamağı ikinciden küçük olamaz."`) gösterilir. "Rastgele Ata" butonu da yalnızca geçerli eldesiz/onluk bozmasız havuzdan soru üretir.
  3. **Öğrenci — Renkli Yönerge ve 0–19 Sayı Doğrusu Deneyimi**:
     - Her soruda üst yönergede: **`"{S.idx}. Sorudaki sayıları "` + `<span class="hl-drag">sayı doğrusuna sürükle</span>` + `" ve "` + `<span class="hl-solve">işlemi yap</span>.`"** gösterilir.
     - Öğrenci 0'dan 19'a kadar 20 noktalı sayı doğrusunu görür. Sayıları sürüklediğinde zıplama kemerleri (`jump-arc`), adım dalgaları (`step-ripple`) ve Türkçe sesli sayım (`"sıfır"` … `"on dokuz"`) 19'a kadar kusursuz çalışır.
- **Görsel Kimlik ve Tema**:
  - Mevcut iOS/iPadOS UIKit semantik renk sistemi (`--bg`, `--cell`, `--blue`, `--green`, `--red`, `--orange`, `--purple`) ve Açık/Koyu görünüm (`data-theme="dark"`) uyumu birebir korunur.
  - `.hl-drag` (canlı mavi/turuncu vurgu) ve `.hl-solve` (canlı yeşil/mor vurgu) sınıfları hem açık hem koyu temada yüksek okunabilirlik ve görsel çekicilik sağlar.
  - 0–19 arası 20 bölmenin (`zone`) tablet ve masaüstü ekranlarda sıkışmadan, okunaklı (`tabular-nums`) ve dokunmatik hedefi rahat olacak şekilde esnek boyutlandırılması (`clamp` ile `--mk` ve `.zlabel` ölçeklemesi) sağlanır.

---

## 3. Key Product Decisions & Trade-Offs

- **Karar 1 — Eldesiz Toplama ve Onluk Bozmasız Çıkarma Matematiksel Kuralı**:
  - *Seçilen Yaklaşım*:
    - Toplama (`+`): `num1 >= 1`, `num2 >= 1`, `num1 + num2 <= 19`, `(num1 % 10) + (num2 % 10) <= 9` ve koyulacak birimlerin 1–9 arası rakam kimliğini koruması.
    - Çıkarma (`-`): `num1 >= 2`, `num2 >= 1`, `num1 <= 19`, `num1 - num2 >= 0` ve `(num1 % 10) >= (num2 % 10)` (onluk bozmasız).
  - *Neden*: Öğrencinin onluk bozma veya elde aktarma kavramına geçmeden önce 19'a kadar sayı doğrusu üzerinde birim rakamlarla (1–9) ileri/geri sayarak işlem yapmasını garanti altına alır.
- **Karar 2 — Evre Anahtarı (Switch) Veri Uyumluluğu**:
  - *Seçilen Yaklaşım*: Paket verisine `tumEvreler: boolean` alanı eklenir. `tumEvreler: false` olduğunda yalnızca `PHASES.filter(p => p.k === "B")` üzerinden doğrulama ve kod üretimi yapılır.
  - *Neden*: Hem yerel depolama (`localStorage`) hem de Google Sheets senkronizasyonu (`oturum_olusturma`) ile geriye dönük tam uyumlu çalışır; daha önce kaydedilmiş 4 evreli öğrenci paketleri bozulmadan düzenlenebilir.

---

## 4. Technical Architecture & Data Strategy *(Technical Reference)*

- **Mimari ve Bileşen Diyagramı**:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        ÖĞRETMEN MODÜLÜ (Teacher UI)                      │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │ Evre Modu Switch (#tAllPhasesSwitch)                               │  │
│  │  ├─ Açık (true)  ──► 4 Evre Aktif: A (Başlama), B (Uyg), C, D      │  │
│  │  └─ Kapalı (false)─► Sadece B (Uygulama) Aktif · Tek Kod Üretimi   │  │
│  └────────────────────────────────────────────────────────────────────┘  │
│                                    │                                     │
│                                    ▼                                     │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │ Soru Oluşturucu & Doğrulayıcı (calcAnswer / addQuestion / Pool)    │  │
│  │  ├─ Toplama (+): a+b ≤ 19 && (a%10)+(b%10) ≤ 9 (Eldesiz)           │  │
│  │  └─ Çıkarma (−): a≤19 && a-b ≥ 0 && (a%10) ≥ (b%10) (Onluk Bozmasız)│  │
│  └────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────┬─────────────────────────────────────┘
                                     │ Oturum Kaydı (localStorage & Sheets)
                                     ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                        ÖĞRENCİ MODÜLÜ (Student UI)                       │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │ Renkli Soru Yönergesi (#prompt)                                    │  │
│  │  "1. Sorudaki sayıları [sayı doğrusuna sürükle] ve [işlemi yap]."  │  │
│  └─────────────────────────────────┬──────────────────────────────────┘  │
│                                    ▼                                     │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │ Tepsi (buildTray) & 1-9 Rakam Birim Şekilleri (.sh)                │  │
│  └─────────────────────────────────┬──────────────────────────────────┘  │
│                                    ▼                                     │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │ 0–19 Etkileşimli Sayı Doğrusu (buildNumberLine: 0..19 zones)       │  │
│  │  ├─ Zıplama Kemeri (drawJumpArc) & İz Bırakma (dropMarker 0..19)   │  │
│  │  ├─ Türkçe Sesli Sayım (NUM_WORDS: "sıfır" .. "on dokuz")          │  │
│  │  └─ El Yazısı Cevap Tanıma (Tesseract OCR 1 ve 2 basamaklı sayı)   │  │
│  └────────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────┘
```

- **Etkileşimli Bileşen ve Durum Eşlemesi**:
  - `getActivePhases()`: `#tAllPhases` switch durumuna göre ya `[A, B, C, D]` ya da yalnızca `[B]` döndürür.
  - `toggleAllPhases()`: Switch değiştiğinde `phaseSeg` ve `#fillAllBtn` görünürlüğünü günceller; kapalıysa aktif evreyi `"B"` (Uygulama) yapar ve `updatePhaseUI()` çağırır.
  - `validateOperation(n1, n2, op)`: İşlemin 1–19 aralığında ve eldesiz/onluk bozmasız olup olmadığını kontrol eder; geçersizse nedenini belirten Türkçe hata mesajı döndürür.
  - `nextQuestion()` içinde `#prompt` render'ı: `innerHTML` ile `"sayı doğrusuna sürükle"` ve `"işlemi yap"` ifadelerini farklı renkli `<span>` sınıflarıyla yazar.
  - `buildNumberLine()` & `dropMarker()`: `0`–`19` arası 20 düğüm oluşturur, iki basamaklı sayılarda da `1–9` arası şekil/renk paletini güvenli şekilde eşler.
