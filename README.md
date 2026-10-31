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

<p align="center">
  <a href="https://osmnnuri.github.io"><b>Güncel görseller için siteme göz at</b></a>
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
| **Gelecek / Gidecek** | Alınacak ve verilecek paraları tarihiyle planlama; "Alındı / Verildi" denince gelir / gider olarak ekleme |
| **Çoklu para birimi** | Her işlem kendi para biriminde; toplamlar seçilen ana birime çevrilerek gösterilir |
| **Yatırımlar** | Altın, döviz, hisse ve fon alımları; güncel fiyatla değer ve kâr / zarar |
| **Piyasalar** | TCMB kurları, altın ve gümüş fiyatları, BIST hisseleri ve izleme listesi (altın ve BIST Türkiye bölgesinde) |
| **Bildirimler** | Hatırlatıcı ve limit uyarıları sistem bildirimi olarak; duyurular push bildirimiyle gelir ve üst bardaki bildirim panelinde okundu işaretlenip silinebilir |
| **Çevrimdışı** | İnternet yokken girilen kayıtlar bağlantı gelince kendiliğinden eşitlenir |
| **Güvenlik** | E-posta doğrulamalı hesap, PIN kilidi, kullanıcıya özel veri kuralları |
| **Kişiselleştirme** | Açık / koyu tema, 11 dil (Arapçada sağdan sola düzen), ayarlanabilir yazı boyutu |
| **Yedekleme** | JSON yedek ve geri yükleme, CSV dışa aktarma, 30 gün saklayan geri dönüşüm kutusu |
| **Geri bildirim** | Uygulama içinden şikâyet / öneri gönderme ve yanıtı görme; gizlilik sayfasından hesap silme |

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
| Bildirim | Firebase Cloud Messaging (push), cihazdaki zamanlanmış bildirimler |
| Yönetim | Firebase'e bağlanan statik yönetim paneli: kullanıcılar, şikâyetler, hata kayıtları, duyurular |
| Ekran görüntüsü okuma | Google ML Kit (mobil), Tesseract.js (web); tamamen cihaz üzerinde |
| Sesli giriş | Cihazın konuşma tanıması + Türkçe cümle çözümleyici |
| Piyasa verisi | TCMB bülteni, serbest piyasa altın fiyatları, BIST fiyatları (Netlify üzerindeki küçük bir aracı fonksiyon üzerinden) |
| Test | 198 otomatik test |

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
- **Çoklu dil ve para birimi.** Arayüz metinleri tek kaynaktan 11 dile çevrilir;
  tarih ve tutarlar dile ve bölgeye göre biçimlenir. Her işlem kendi para
  biriminde saklanır, raporlar o günün kuruyla ana birime çevrilir.
- **Aynı gün işlem birleştirme.** Aynı gün aynı açıklamayla girilen işlemler tek
  kayıtta toplanır; her giriş ayrıca saklanır ve raporda ayrıntısıyla gösterilir.
- **Duyuru ve bildirim paneli.** Yönetim panelinden yayımlanan duyuru, açık
  uygulamada banner, kapalıyken push bildirimi olarak gelir; bir kez gelen
  duyuru bildirim panelinde kalır ve uygulama her açıldığında yeniden
  gösterilmez.
- **Sıkı veri kuralları.** Firestore kuralları her kullanıcıyı kendi verisiyle
  sınırlar ve yazılan alanları, türlerini ve uzunluklarını denetler.

## Durum

- Web uygulaması yayında (tarayıcıda ve telefonun ana ekranına eklenerek kullanılır;
  iPhone'da ana ekrandan açıldığında tam ekran çalışır).
- Android sürümü çalışıyor; mağaza yayını planlanıyor.
- Kaynak kod özel bir depoda tutuluyor.

Döviz, altın ve borsa verileri bilgi amaçlıdır; yatırım tavsiyesi değildir.
Borsa fiyatları yaklaşık 15 dakika gecikmelidir.

## İletişim

**osmnnuri** ·  [Web sitesi](https://osmnnuri.github.io) · [GitHub](https://github.com/osmnnuri) · osmannuriyasar57@gmail.com

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
- **Budgets and recurring reminders,** delivered as system notifications.
- **Planned money in / out** with due dates, turned into income or expense once
  settled.
- **Multiple currencies:** each transaction keeps its own currency; totals are
  converted to the base currency you choose.
- **Investments:** gold, currencies, stocks and funds valued at current prices,
  with profit / loss.
- **Markets:** central-bank exchange rates, gold prices, Borsa Istanbul stocks.
- **Offline-first:** entries made offline sync automatically.
- **Notifications panel:** announcements arrive as push notifications and stay
  in an in-app panel where they can be marked as read or deleted.
- **In-app feedback** and an admin panel for users, feedback, error reports and
  announcements.
- **Light / dark theme, 11 languages (right-to-left for Arabic), PIN lock, JSON
  backup and CSV export.**

The interface language follows your device and can be changed in Settings. Voice input and screenshot parsing currently understand
Turkish only.
