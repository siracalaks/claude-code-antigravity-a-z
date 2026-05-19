# Claude Code + Google Antigravity — A'dan Z'ye Türkçe Rehber

[![Lisans: MIT](https://img.shields.io/badge/Lisans-MIT-yellow.svg)](LICENSE)
![Dil: Türkçe](https://img.shields.io/badge/Dil-T%C3%BCrk%C3%A7e-red.svg)
![Versiyon: 1.1](https://img.shields.io/badge/Versiyon-1.1-blue.svg)

**🌍 Diller:** [English](README.md) · **Türkçe**
**📖 Rehberler:** [GUIDE.md (EN)](GUIDE.md) · [REHBER.md (TR)](REHBER.md)

> **Tek dosyada Claude Code + Antigravity ustalığı.**
> Slash komutlardan MCP server'lara, hooks'tan MVP playbook'una kadar her şey Türkçe — başka yerden araştırma yapmadan iş bitirmek için.

---

## 📖 Bu Rehber Kime Hitap Ediyor?

- **Antigravity IDE** kullanan ya da denemek isteyenler
- **Claude Code** CLI / extension'ı günlük iş akışına oturtmak isteyenler
- Türkçe kaynak bulamayıp İngilizce dokümanlarda kaybolanlar
- MVP çıkarmak isteyen solo geliştiriciler / kurucular

## 🚀 Hızlı Başlangıç

Dosyayı oku: **[REHBER.md](REHBER.md)**

İlk okumada şu sırayı öneririm:
1. **Bölüm 1** — Genel Resim (5 dk)
2. **Bölüm 14** — Acil Durum Cheat Sheet (10 dk)
3. Sonra ihtiyaç duydukça `Ctrl+F` ile arat

## 📑 İçinde Ne Var?

| # | Bölüm | İçerik |
|---|---|---|
| 1 | Genel Resim | Antigravity + Claude Code + MCP mimarisi |
| 2 | Antigravity A-Z | Editor View, Manager View, modes, knowledge base |
| 3 | Claude Code A-Z | Kurulum, model seçimi, konfigürasyon hiyerarşisi |
| 4 | Slash Komutları | 60+ komut, kategorize edilmiş tam referans |
| 5 | Skills | Yetenek sistemi — kendi skill'ini nasıl yazarsın |
| 6 | MCP | Model Context Protocol — dış servis entegrasyonları |
| 7 | Subagents | Paralel akıllar, ne zaman delegasyon yapılır |
| 8 | Hooks | 13 lifecycle olayı, otomasyon örnekleri |
| 9 | Plugins | Paketlenmiş eklentiler |
| 10 | settings.json | Tüm ayarlar tek tablo |
| 11 | Workflow'lar | Profesyonel kullanım pattern'leri |
| 12 | MVP Playbook | Fikirden ürüne adım adım |
| 13 | Self-Updating Agent | Dokümantasyonu kendi güncelleyen agent ⭐ |
| 14 | Acil Durum | Cheat sheet — token bitti, konu değişti, vb. |
| 15 | Kaynaklar | İleri okuma listesi |

## 💡 Öne Çıkan Bölümler

- **MCP ≠ MVP** karışıklığı (evet, farklı şeyler — Bölüm 6.1)
- **Token tasarrufunun 7 kuralı** (Bölüm 14)
- **Model seçim tablosu** — Opus, Sonnet, Haiku ne zaman? (Bölüm 3.9)
- **Hooks ile otomatik test/lint** örnekleri (Bölüm 8)

## 📂 Repo Yapısı

```
.
├── GUIDE.md              # Rehber (English) — tek kaynak doğruluk
├── REHBER.md             # Rehber (Türkçe) — GUIDE.md ile senkron tutulur
├── README.md             # İngilizce README
├── README.tr.md          # Buradasınız
├── CONTRIBUTING.md       # Katkı rehberi (EN)
├── CONTRIBUTING.tr.md    # Katkı rehberi (TR)
├── CHANGELOG.md          # Sürüm geçmişi (Keep a Changelog)
├── CITATION.cff          # Akademik atıf metaverisi
├── LICENSE               # MIT
├── .github/              # Issue + PR şablonları
└── examples/             # Çalıştırılabilir skill / hook / workflow dosyaları
    ├── skills/           # Hazır SKILL.md paketleri
    ├── hooks/            # Açıklamalı settings.example.json
    └── workflows/        # GitHub Actions (günlük doc-updater)
```

Çıkarılmış skill ve hook'ların kurulum talimatları için [`examples/README.md`](examples/README.md) dosyasına bak.

## 🤝 Katkı

Eksik, hatalı veya güncellenmesi gereken kısımlar için **issue açın** veya PR gönderin. [CONTRIBUTING.tr.md](CONTRIBUTING.tr.md) dosyasını oku. EN ↔ TR senkron kuralı gereği `GUIDE.md` ve `REHBER.md` değişiklikleri aynı PR içinde birlikte yapılmalıdır.

## 📄 Lisans

[MIT Lisansı](LICENSE) — Özgürce kullan, değiştir, paylaş. Atıf nazikçe yapılır.

---

**Son güncelleme:** Mayıs 2026 | **Versiyon:** 1.1 | **Yazar:** [@siracalaks](https://github.com/siracalaks)
