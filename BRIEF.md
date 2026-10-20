# Buzkıran: etkinlikler için icebreaker platformu

## Ne

Etkinlik düzenleyen biri (meetup, şirket içi onboarding, atölye, konferans yan etkinliği) bir etkinlik oluşturur ve içine sırayla oynatacağı **aktivasyonlar** ekler. Katılımcılar telefonlarından bir kodla ya da QR ile katılır, hesap açmaz. Etkinlik sırasında düzenleyen kişi aktivasyonları sahneden yönetir; büyük ekranda (projeksiyon) sonuçlar canlı görünür.

Amaç: odadaki insanların ilk 15 dakikada birbirleriyle konuşmaya başlaması.

## Kimler

- **Düzenleyen (host):** etkinliği hazırlar, aktivasyonları sıralar, etkinlik günü "sahne" ekranından ilerletir. Dizüstü bilgisayarda kullanır.
- **Katılımcı:** telefondan girer, adını ve isteğe bağlı bir iki bilgi yazar, aktivasyonlara katılır. Uygulama indirmez, hesap açmaz.
- **Ekran:** projeksiyona yansıtılan, yalnızca gösterim yapan sayfa (katılım kodu, QR, canlı sonuçlar).

## Aktivasyonlar (ilk sürüm)

Her aktivasyonun bir hazırlık formu (host), bir katılımcı görünümü (telefon) ve bir ekran görünümü (projeksiyon) vardır.

1. **İki doğru bir yalan:** katılımcı kendisi hakkında üç cümle yazar, biri yalan. Ekran sırayla birini gösterir, herkes yalanı tahmin eder, sonra cevap açılır.
2. **İnsan bingosu:** host 3×3 ya da 4×4 kart için özellikler yazar ("hiç yurt dışında yaşamış", "kedisi var"). Katılımcı odada o özelliğe uyan birini bulup adını kutuya yazar. İlk bingo yapanlar ekranda.
3. **Hızlı tanışma eşleştirmesi:** host tur sayısını ve süresini seçer. Sistem katılımcıları ikişer (tek kalırsa üçlü) eşler, her turda daha önce eşleşmemiş kişileri seçer. Telefonda "şu kişiyi bul" ve geri sayım görünür.
4. **Anlık anket:** tek seçimli soru. Sonuçlar ekranda canlı çubuk grafik.
5. **Kelime bulutu:** "Bu etkinlikten ne bekliyorsun?" gibi açık uçlu soru, en fazla üç kelime. Ekranda canlı kelime bulutu.

## Akışlar

- **Host:** etkinlik oluştur (ad, tarih, kısa açıklama) → aktivasyon ekle, sırala, düzenle → etkinliği aç (katılım kodu oluşur) → sahne ekranında "başlat / sonraki adım / sonuçları göster / kapat".
- **Katılımcı:** kodu gir ya da QR'ı okut → ad yaz → bekleme ekranı → aktif aktivasyon telefona gelir → cevap ver → sonucu gör.
- **Ekran:** `/e/<kod>/ekran` adresi; host ilerlettikçe kendiliğinden değişir.

Host hesabı için ağır bir giriş sistemi gerekmez: e-posta ile sihirli bağlantı ya da en basit haliyle host'a özel gizli bir yönetim bağlantısı (etkinlik oluşturulunca verilir, tarayıcıda saklanır). İlk sürüm için gizli yönetim bağlantısı yeterli.

## Canlılık

Vercel'de sunucusuz çalışacak. WebSocket yerine 1,5–2 saniyelik polling ya da SSE yeterli; 150 katılımcıda sorunsuz çalışmalı. Her istemci yalnızca değişen durumu çeker (etkinlik durumu sürümü / `updatedAt`).

## Teknik

- Next.js (App Router) + TypeScript + Tailwind.
- Veri: Postgres. Vercel takımında (polenist) Neon kurulu; bu proje için **yeni** bir Neon kaynağı aç, var olanlara dokunma. ORM olarak Drizzle.
- Test: Vitest (eşleştirme algoritması, puanlama, durum geçişleri), Playwright (host + iki katılımcı + ekran uçtan uca akışı).
- Dil: arayüz Türkçe. Kod, commit mesajları ve tanımlayıcılar İngilizce.

## Tasarım

- Telefon önce gelir: büyük dokunma alanları, tek elle kullanım, yüksek kontrast.
- Ekran görünümü 3 metreden okunur: büyük tipografi, az metin.
- Sıcak ama klişe olmayan bir görsel dil; rastgele gradyan ve emoji yığını yok.
- Boş durumlar ne yapılacağını söyler ("Henüz aktivasyon yok, ilkini ekle").

## Teslim

- GitHub: `omerfarukdemiral/buzkiran` (public).
- Vercel: proje `buzkiran`, production deploy. Neon bağlantısı Vercel ortam değişkenlerinden gelir.
- Canlıda uçtan uca test: Playwright production URL'ye karşı çalışır (host bir etkinlik açar, iki katılımcı katılır, en az iki aktivasyon oynanır).
- README: ne olduğu, nasıl çalıştırıldığı, ortam değişkenleri.

## Kabul ölçütleri

- Host bir etkinliği beş aktivasyon türünün hepsiyle hazırlayıp sahneden baştan sona oynatabilir.
- Katılımcı telefondan 20 saniyede katılır.
- Ekran görünümü host ilerlettiğinde 3 saniye içinde değişir.
- Hızlı tanışma eşleştirmesi aynı iki kişiyi, kişi sayısı izin verdiği sürece tekrar eşleştirmez (testle kanıtlı).
- Vitest ve Playwright testleri geçer; production'da uçtan uca test geçer.

## Çalışma şekli

- Görev listesi tut; işleri paralel subagent'lara ya da bir workflow'a dağıt.
- Senin yerine verdiğim kararları ve bilmem gereken şeyleri kısa tutarak inbox'a yaz; uzun raporları sohbete değil dosyaya yaz.
