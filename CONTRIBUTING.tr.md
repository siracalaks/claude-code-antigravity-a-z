# Katkı Rehberi

Katkıda bulunmayı düşündüğün için teşekkürler. Bu repo tek dosyalık bir bilgi rehberi — küçük, odaklı katkılar en kolay birleşendir.

**🌍 Dil:** [English](CONTRIBUTING.md) · **Türkçe**

---

## Hangi katkılar memnuniyetle karşılanır?

- **Yazım / dilbilgisi düzeltmeleri** — direkt PR aç, issue açmana gerek yok.
- **Gerçek hatası düzeltmeleri** — rehberdeki bir CLI flag, komut, sürüm numarası veya davranış yanlışsa, düzelt ve PR açıklamasına kaynak ekle (release notes, resmi doküman, commit).
- **Yeni içerik** — bölüm genişletmeleri, ek workflow pattern'leri, eksik slash komutları, yeni MCP örnekleri.
- **Çeviriler** — yeni bir dil mi? [Çeviriler](#çeviriler) bölümüne bak.
- **Örnekler** — [`examples/`](examples/) klasöründe çalıştırılabilir snippet'ler (skill'ler, hook'lar, GitHub Action'lar). Rehberin içindeki kod bloklarını gerçek dosyalara çıkaran PR'lar memnuniyetle karşılanır.

## Hangi katkılar **istenmez**

- Reklam, affiliate link veya okuyucuya hizmet etmeyen kendi tanıtımı eklemek.
- Önceden konuşulmadan büyük bölümleri yeniden yazmak — önce issue aç.
- `REHBER.md`'yi birden fazla dosyaya bölmek. Tek dosya tasarımı bilinçli (README "Hızlı Başlangıç" kısmına bak).

---

## EN ↔ TR senkron kuralı (en önemli kural)

Bu repo iki **kaynak doğruluk** dosyasını senkron tutar:

- [`GUIDE.md`](GUIDE.md) — İngilizce
- [`REHBER.md`](REHBER.md) — Türkçe

**Birini değiştirirsen, aynı PR içinde diğerini de değiştirmen gerekir.**

Sadece bir dil biliyorsan:
1. Bildiğin dilde değişikliği yap.
2. PR'a checkbox notu ekle: `- [ ] TR çeviri gerekli` veya `- [ ] EN çeviri gerekli`.
3. Bir maintainer veya çevirmen diğer tarafı merge öncesi tamamlar.

Diğer dili bayraklamadan sadece bir dili güncelleyen PR'lar ya çeviri yapması ya da eksiği açıkça belirtmesi istenecek.

---

## PR nasıl gönderilir

1. Repo'yu fork'la ve branch aç: `git checkout -b fix/yazim-bolum-4`
2. Değişikliği yap. Diff'i odaklı tut — PR başına tek düzeltme review'u kolaylaştırır.
3. [Conventional Commit](https://www.conventionalcommits.org/) tarzı commit at:
   - `fix: /compact eşiğini cheat sheet'te düzelt`
   - `docs: monorepo için workflow pattern ekle`
   - `feat(examples): conventional-commit skill'ini çıkar`
4. Push'la ve `main`'e PR aç. PR template'i kullan — gerçek hatası düzeltmelerinde kaynak ister.

## Üslup rehberi

- **Ton:** pratik, kısa, yerinde fikrini söyleyen. Rehber bir çalışma referansı, pazarlama sayfası değil.
- **Pazarlama dili yok:** "güçlü", "harika", "oyun değiştirici" kullanma. Ne yaptığını söyle.
- **Kod blokları:** dili her zaman etiketle (` ```bash `, ` ```yaml `, ` ```markdown `).
- **Tablolar:** 3+ öğe aynı boyutlarda kıyaslandığında bullet liste yerine tablo tercih et.
- **Anchor'lar:** yeni bölüm eklersen eşleşen anchor ekle ve içindekiler tablosundan link ver.
- **Tarih formatı:** ISO 8601 (`2026-05-19`) — göreceli tarihler bayatlar.

## Çeviriler

Yeni bir dil eklemek için (örn. Almanca):

1. `GUIDE.md`'yi `GUIDE.de.md` olarak kopyala ve çevir.
2. `README.md`'yi `README.de.md` olarak kopyala ve çevir.
3. Dil seçiciyi her `README.*` üstüne ekle.
4. PR başlığı: `feat(i18n): Almanca çeviri ekle`.
5. İlerideki EN/TR değişiklikleriyle senkron kalmaya hazır ol; ya da çevirinin geç kalabileceğini PR'da belirt.

## Sorun bildirme

- **Bug / yanlış bilgi** → *İçerik düzeltme* issue template'ini kullan.
- **Öneri / yeni bölüm** → *İçerik önerisi* issue template'ini kullan.
- **Çeviri yardımı isteniyor** → *Çeviri* issue template'ini kullan.

## Davranış kuralları

Nazik ol. Teknik içerikte anlaşmazlık serbest; kişisel saldırı değil. Maintainer'lar verimsiz tartışmaları kapatma/kilit hakkını saklı tutar.

---

**Sorun mu var?** [Discussion](https://github.com/siracalaks/claude-code-antigravity-a-z/discussions) aç veya çalıştığın issue'ya yaz.
