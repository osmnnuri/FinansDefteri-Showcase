<h1 align="center">Finans Defteri</h1>

<p align="center">
  Kişisel gelir-gider, bütçe ve yatırım takibi.<br>
  Sesle, banka ekran görüntüsüyle ya da elle kayıt.
</p>

<p align="center">
  <a href="https://financeapp-997b3.web.app/demo"><b>▶ Canlı demoyu aç</b></a>
  &nbsp;·&nbsp;
  <a href="#english">English</a>
</p>

---

## Demo

**https://financeapp-997b3.web.app/demo**

Kayıt gerekmez. Bağlantı, örnek verilerle dolu ve yalnız size özel geçici bir
hesap açar; istediğinizi ekleyip silebilirsiniz, başka ziyaretçiler bunu görmez.
Telefonda açıp "Ana ekrana ekle" derseniz uygulama gibi çalışır.

## Neden yaptım?

Harcama takibi uygulamalarının çoğu her işlemi elle girmeyi bekliyor; birkaç gün
sonra kayıt tutmak bırakılıyor. Amacım kayıt girmeyi olabildiğince zahmetsiz
hale getirmek ve ay sonunda paranın nereye gittiğini tek bakışta göstermekti.

## Özellikler

| | |
|---|---|
| **Üç yoldan kayıt** | Sesle ("markete 150 lira harcadım"), banka ekran görüntüsünden ya da elle |
| **Aylık rapor** | Gelir, gider, tasarruf oranı, kategori dağılımı; PDF olarak kaydetme |
| **Bütçe** | Kategori bazlı aylık limitler, limit uyarıları, her ay tekrarlayan hatırlatıcılar |
| **Yatırımlar** | Altın, döviz, hisse ve fon alımları; güncel fiyatla değer ve kâr / zarar |
| **Piyasalar** | TCMB kurları, altın ve gümüş fiyatları, BIST hisseleri ve izleme listesi |
| **Çevrimdışı** | İnternet yokken girilen kayıtlar bağlantı gelince kendiliğinden eşitlenir |
| **Güvenlik** | E-posta doğrulamalı hesap, PIN kilidi, kullanıcıya özel veri kuralları |
| **Kişiselleştirme** | Açık / koyu tema, Türkçe / İngilizce, ayarlanabilir yazı boyutu |
| **Yedekleme** | JSON yedek ve geri yükleme, CSV dışa aktarma |

## Nasıl çalışıyor?

```
┌──────────────── Flutter uygulaması (web PWA + Android) ────────────────┐
│   Ekranlar  →  Servisler (iş mantığı)  →  Veri modelleri               │
└──────┬───────────────┬────────────────┬───────────────┬────────────────┘
       │               │                │               │
 Firebase Auth   Cloud Firestore    Cihaz deposu    Dış veri
 (hesaplar)      (işlemler,         (PIN, ayarlar,  (TCMB kurları, altın,
                  yatırımlar)        bütçe, kuyruk)  borsa fiyatları)
```

| Katman | Teknoloji |
|---|---|
| Arayüz | Flutter (tek kod tabanı: web PWA ve Android) |
| Kimlik ve veri | Firebase Authentication, Cloud Firestore |
| Yayın | Firebase Hosting |
| Ekran görüntüsü okuma | Google ML Kit (mobil), Tesseract.js (web); tamamen cihaz üzerinde |
| Sesli giriş | Cihazın konuşma tanıması + Türkçe cümle çözümleyici |
| Piyasa verisi | TCMB bülteni, serbest piyasa altın fiyatları, BIST fiyatları (küçük bir aracı fonksiyon üzerinden) |
| Test | 120 otomatik test |

### Öne çıkan teknik işler

- **Türkçe sesli girişi anlamlandırma.** "84 lira 99 kuruş", "dün", "3 gün önce"
  gibi ifadeleri ve ek almış kategori adlarını ("markete", "faturası")
  ayrıştıran kural tabanlı bir çözümleyici.
- **Ekran görüntüsünden işlem okuma.** OCR'ın konumlarıyla verdiği metin
  parçaları ekrandaki satırlara yeniden dizilir; her satırdan tarih, tutar ve
  gelir / gider bilgisi çıkarılır. Görüntü cihazdan çıkmaz, hiçbir şey onaysız
  eklenmez.
- **Mükerrer kayıt önleme.** Her işleme içeriğinden üretilen sabit bir kimlik
  verilir; aynı ekran görüntüsü yeniden yüklense de kayıt bir kez eklenir.
- **Güvenilir çevrimdışı kayıt.** Kayıtlar kalıcı bir kuyrukta bekler; uygulama
  kapanıp açılsa da kaybolmaz.
- **iPhone'da ana ekrana eklenen web uygulaması.** Çentik boşlukları, kenar
  kaydırma hareketi ve güncelleme önbelleği gibi platforma özgü sorunlar.
- **Kuruş hassasiyeti.** Toplamlar kuruş üzerinden hesaplanır; binlerce
  ondalıklı tutar toplanırken kayan nokta hatası birikmez.
- **Demo modu.** Demo bağlantısı her ziyaretçiye anonim, yalıtılmış bir hesap
  açar ve örnek verileri o hesaba yazar.

## Durum

- Web uygulaması yayında (tarayıcıda ve telefonun ana ekranına eklenerek kullanılır).
- Android sürümü çalışıyor; mağaza yayını planlanıyor.
- Kaynak kod özel bir depoda tutuluyor.

Döviz, altın ve borsa verileri bilgi amaçlıdır; yatırım tavsiyesi değildir.
Borsa fiyatları yaklaşık 15 dakika gecikmelidir.

## İletişim

**osmnnuri** · [GitHub](https://github.com/osmnnuri) · osmannuriyasar57@gmail.com

---

## English

**Finans Defteri** ("Finance Notebook") is a personal income / expense, budget
and investment tracker built with Flutter and Firebase. It runs as a web PWA
and on Android from a single codebase.

**Live demo:** https://financeapp-997b3.web.app/demo — no sign-up; the link
opens a private, temporary account filled with sample data.

- **Three ways to add a transaction:** by voice (Turkish), from a bank-app
  screenshot (on-device OCR), or manually.
- **Monthly reports** with category breakdown and PDF export.
- **Budgets and recurring reminders.**
- **Investments:** gold, currencies, stocks and funds valued at current prices,
  with profit / loss.
- **Markets:** central-bank exchange rates, gold prices, Borsa Istanbul stocks.
- **Offline-first:** entries made offline sync automatically.
- **Light / dark theme, Turkish / English, PIN lock, JSON backup and CSV export.**

The interface language follows your device (Turkish or English) and can be
changed in Settings. Voice input and screenshot parsing currently understand
Turkish only.
