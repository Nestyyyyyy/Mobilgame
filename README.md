# Sınıfın Kralı 👊

Telefonda oynanan, tek dosyalık (HTML5 canvas) bir **piksel-art** oyunu.
Ekranda kırmızı polo tişörtlü, dağınık siyah saçlı bir arkadaşımızın kocaman kafası duruyor.
Alttaki çubuktan eşya seçip kafasına dokunuyoruz; kafası sallanıyor, yüzü değişiyor,
komik laflar ediyor. Sonunda istersek 🤝 ile barışıyoruz.

## Oynamak

`index.html` dosyasını tarayıcıda açman yeterli; kurulum, paket, build yok.

Telefonda test etmek için bilgisayarda küçük bir sunucu açıp aynı Wi-Fi'den girebilirsin:

```bash
python3 -m http.server 8080
# telefonda: http://<bilgisayar-ip>:8080
```

Ya da repoyu GitHub Pages ile yayınlayıp linki telefonda açabilirsin.

## Eşyalar

| Eşya | Ne olur |
|------|---------|
| ✋ Tokat | Kafa yana savrulur, yanakta el izi kalır, "ŞAK!" |
| 🥊 Eldiven | En sert vuruş; gözler döner, başının etrafında yıldızlar |
| 📕 Kitap | Fotoğraftaki kırmızı kitap; "PAT!" |
| 🩴 Terlik | Klasik; "ŞLAP!" |
| 🍅 Domates | Alttan fırlatılır, yüze yapışıp akar, surat asar |
| 🛏️ Yastık | Yumuşak; tüyler uçuşur, uykusu gelir ("Z Z Z") |
| 🪶 Tüy | Gıdıklar, kahkaha atar |
| ⛄ Kartopu | Fırlatılır, üşür, dudakları morarır, titrer |
| 🤝 Barış | Sırıtır, iki baş parmak kaldırır (fotoğraftaki gibi), kalpler uçar; kombo ve lekeler sıfırlanır |

- Kafa dışına dokunursan **ISKA** olur ve kombo sıfırlanır.
- Kısa sürede art arda vurursan **KOMBO** artar; en yüksek kombo **REKOR** olarak tarayıcıda saklanır.
- Kısa sürede çok vurursan kızar: "Yeter artık ya!"
- Gözleri parmağını / fare imlecini takip eder, arada göz kırpar.
- Klavye: `1`–`9` eşya seçer, `Boşluk` vurur.

## Teknik

- Her şey `index.html` içinde: sahne, kafa, eşyalar, fizik, ses, dokunmatik kontroller.
- Oyun düşük çözünürlüklü bir off-screen canvas'a çizilir, sonra tam sayı katsayıyla
  büyütülür (nearest-neighbor) → gerçek piksel görünümü.
- Kafa 24×26 piksellik bir sprite (`HEAD`; kulaklar, saç tutamları, gölgeler), önce kendi
  tuvaline çizilir, sonra boyun noktasından yay fiziğiyle (`head.a`, `head.ox`, `head.sq`)
  döndürülüp ezilir.
- Vuruşlar birinci şahıs: ekranın alt köşesinden kazak kollu bir kol uzanır, yumruk eşyayı
  tutar (`drawArm`, `drawHeld`), fırlatılan eşyalar önce elde görünüp sonra uçar.
- Eşya çubuğundaki ikonlar emoji değil, aynı sprite'lardan `canvas.toDataURL()` ile üretilir.
- Yüz ifadeleri `drawFace()` içinde durum bazlı çizilir: `happy, ouch, dizzy, grumpy,
  sleepy, laugh, cold, angry, grin`.
- Yeni eşya eklemek için `SPR` içine 12×12 (fırlatılanlar 8×8) bir sprite ve `ITEMS` listesine bir satır eklemek
  yeterli (`kind: 'swing'` elle vurulan, `kind: 'throw'` fırlatılan).
- Sesler WebAudio ile anlık üretilir, ses dosyası yok.
