# 🎯 Claude Code + Google Antigravity — A'dan Z'ye Ustalık Referansı

**Versiyon:** 1.0 — 13 Mayıs 2026
**Hedef:** Her şeyi tek dosyada bulmak; başka yerden araştırmadan iş yapmak
**Kapsam:** Antigravity IDE + Claude Code eklentisi içinde doğal dille kod yazarak profesyonel proje üretmek

> **NOT — MCP vs MVP kafa karışıklığı:**
> - **MCP** = Model Context Protocol — Claude'un dış servislere (GitHub, veritabanı, browser) bağlanmasını sağlayan açık protokol
> - **MVP** = Minimum Viable Product — yazılım terminolojisi, "satılabilir ürünün en küçük çalışan hali"
> Bu doküman MCP'yi detaylı anlatır; MVP konusunda da "ilk MVP'ni Claude ile nasıl çıkarırsın" başlığı vardır.

---

## 📑 İçindekiler

1. [Genel Resim — Mimari ve Felsefe](#1-genel-resim)
2. [Google Antigravity A'dan Z'ye](#2-antigravity-a-z)
3. [Claude Code A'dan Z'ye](#3-claude-code-a-z)
4. [Slash Komutları — Tam Referans](#4-slash-komutlari)
5. [Skills — Yetenek Sistemi](#5-skills)
6. [MCP — Model Context Protocol](#6-mcp)
7. [Subagents — Paralel Akıllar](#7-subagents)
8. [Hooks — Otomatik Tetikleyiciler](#8-hooks)
9. [Plugins — Paketlenmiş Eklentiler](#9-plugins)
10. [settings.json — Tüm Ayarlar](#10-settings)
11. [Workflow Pattern'leri — Profesyonel Kullanım](#11-workflows)
12. [MVP Üretme Playbook'u](#12-mvp)
13. [Self-Updating Documentation Agent](#13-self-updating-agent) ⭐
14. [Acil Durum Cheat Sheet](#14-cheat-sheet)
15. [Kaynak Listesi](#15-kaynaklar)

---

<a name="1-genel-resim"></a>
## 1. 🌍 Genel Resim — Mimari ve Felsefe

### Antigravity ve Claude Code'un İlişkisi

Antigravity bir **agent-first IDE** (VS Code fork'u). Claude Code ise terminal/extension olarak çalışan **agentic CLI**. İkisini birlikte kullandığında üç katmanlı bir sistem oluşturur:

```
┌─────────────────────────────────────────────────────────┐
│  ANTIGRAVITY (IDE Katmanı)                              │
│  - Editor View: kod yazma, inline edit, agent chat      │
│  - Manager View: çoklu agent orkestrasyon, artifacts    │
│  - Built-in browser, terminal, Knowledge Base           │
│  - Modeller: Gemini 3.1 Pro/Flash, Claude, GPT-OSS      │
└──────────────────────┬──────────────────────────────────┘
                       │ extension olarak
┌──────────────────────▼──────────────────────────────────┐
│  CLAUDE CODE (Agent Katmanı)                            │
│  - CLAUDE.md, skills, subagents, hooks, plugins         │
│  - 13 lifecycle hook olayı                              │
│  - MCP server bağlantıları                              │
│  - Slash komutları (60+ built-in)                       │
└──────────────────────┬──────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────┐
│  MCP SERVERS (Dış Dünya Katmanı)                        │
│  GitHub, Postgres, Playwright, Context7, Linear, ...    │
└─────────────────────────────────────────────────────────┘
```

### Felsefe: Kim Ne Yapar?

| Katman | Güçlü olduğu iş | Zayıf olduğu iş |
|---|---|---|
| **Antigravity Gemini** | Planlama, mimari, geniş context (2M token), browser test, paralel agent | Multi-file refactor derinliği |
| **Claude Code** | Çok dosyalı reasoning, refactor, debug, terminal işleri | Tarayıcı entegrasyonu, görsel iş |
| **MCP Servers** | Spesifik servis erişimi (DB, API) | Genel akıl yürütme |
| **Sen (insan)** | Yargı, mimari karar, son onay | Tekrar eden boilerplate |

**Profesyonel kullanımın altın kuralı:** Gemini düşünür (plan), Claude yapar (build), MCP bağlar (data), sen onaylarsın (judgment).

---

<a name="2-antigravity-a-z"></a>
## 2. 🚀 Google Antigravity A'dan Z'ye

### 2.1 İki Ana Görünüm

#### Editor View
Geleneksel VS Code deneyimi. `Cmd+E` / `Ctrl+E` ile **Manager View**'a geç.
- **Sidebar chat** (`Cmd+L`): hızlı sohbet, açık dosyaya context'li
- **Inline edit** (`Cmd+I`): seçili kod üzerinde direkt değişiklik
- **Tab completion**: yazarken otomatik tamamlama
- **Terminal**: built-in, agent burada komut çalıştırabilir
- **Built-in browser**: agent web app'i açıp test edebilir

#### Manager View (Mission Control)
Asıl ayrıcalık burada. **Birden fazla agent paralel** çalıştırırsın.
- **Workspace**: her workspace = bir proje klasörü
- **Conversation**: her sohbet = bir agent instance
- **Worktree toggle**: her conversation için ayrı git worktree oluşturur (paralel agent'lar birbirine girmesin diye)
- **Status göstergesi**: hangi agent ne yapıyor, hangi onayı bekliyor
- **Maks 5 paralel agent** (preview limiti)

### 2.2 Modes (Çalışma Modları)

| Mode | Ne yapar | Ne zaman |
|---|---|---|
| **Planning Mode** | Önce plan artifact'i üretir, onay bekler | Karmaşık feature, riskli iş |
| **Execution Mode** | Direkt kodlamaya geçer (vibe coding) | UI tweak, küçük bugfix |
| **Plan-Review-Execute** | Plan → yorum → güncelle → kodla | Default, sağlam yaklaşım |

### 2.3 Artifacts — Güven Boşluğunun Çözümü

Agent her iş için **artifact** üretir. Artifact = doğrulanabilir teslimat. Türleri:

1. **Task Lists** — yapılacaklar listesi, hangi dosya değişecek
2. **Implementation Plans** — mimari plan, hangi fonksiyon nasıl
3. **Code Diffs** — değişikliklerin önizlemesi
4. **Screenshots** — UI değişikliklerinin görsel kanıtı
5. **Browser Recordings** — E2E test akışlarının videosu
6. **Walkthroughs** — agent'in adım adım ne yaptığı

**Kullanım kuralı:** Artifact üzerinde Google Docs gibi **yorum** yap, agent yorumları okuyup düzeltir (akışı bozmadan).

### 2.4 Knowledge Base (Brain)

`.gemini/antigravity/brain/` klasörü = projenin **kalıcı hafızası**. Agent yeni şeyler öğrendikçe (örn: "biz Tailwind kullanırız, Bootstrap değil") buraya yazar. Sonraki agent'lar bu dosyayı okur.

**Manuel ekleme:** Settings → Knowledge → Add Knowledge Item. Örnek girdiler:
- "Tüm API endpoint'leri `/api/v1/` ile başlar"
- "Veritabanı erişimi sadece server actions üzerinden"
- "Müşteri X için renk paleti: #1a1a1a, #FF6B35"

### 2.5 Agent Ayarları (Settings → Agent)

| Ayar | Önerilen değer | Neden |
|---|---|---|
| **Artifact Review Policy** | Asks for Review | Her artifact'i gör, yanlış yöne gitmesin |
| **Terminal Command Auto Execution** | Request Review | `rm -rf` gibi tehlikeli komut için onay |
| **Terminal Sandbox** | Enabled | Workspace dışına çıkmasın |
| **Non-Workspace File Access** | Disabled | Hassas dosyaları (.env, ssh keys) koruma |
| **Browser Agent** | Enabled | E2E test için kritik |

### 2.6 MCP Server Yönetimi (Antigravity Tarafı)

**Settings → Customizations → MCP** üzerinden tek tıkla kurulum. Antigravity'nin **one-click MCP store**'unda:
- Google ekosistemi (Drive, Sheets, Docs, Calendar)
- GitHub, Figma, Slack, Notion
- "Open MCP Config" ile custom MCP server da eklenir

**Önemli:** Antigravity'de kurulan MCP'ler Claude Code extension'ı tarafından **görülmez**; Claude Code kendi `~/.claude/settings.json` dosyasını okur. İkisini ayrı ayrı yapılandıracaksın.

### 2.7 Antigravity Kısayolları

| Kısayol | İşlev |
|---|---|
| `Cmd+E` / `Ctrl+E` | Editor ↔ Manager geçişi |
| `Cmd+L` / `Ctrl+L` | Sidebar chat aç/kapa |
| `Cmd+I` / `Ctrl+I` | Inline edit (seçili kod) |
| `Cmd+,` / `Ctrl+,` | Settings |
| `Cmd+P` / `Ctrl+P` | Dosya arama |
| `Cmd+Shift+P` / `Ctrl+Shift+P` | Komut paleti |

### 2.8 Antigravity Tuzakları

- **Rate limit kafa karışıklığı**: Public preview'da 5 saatlik quota sandı; gerçekte haftalık. Yoğun Claude/GPT kullanan kullanıcılar erken takılıyor.
- **Workspace dışı dosya erişimi**: Default olarak kapalı; bilerek açana kadar agent `.env` göremez (güvenlik).
- **VS Code Marketplace yerine Open VSX**: Bazı Microsoft-only extension'lar çalışmaz; çoğu çalışır.
- **Gmail-only**: Workspace hesabı henüz desteklenmiyor (Mayıs 2026 itibariyle).
- **Lag sorunu**: Context büyüdükçe RAM yer; ara sıra window restart yardımcı olur.

---

<a name="3-claude-code-a-z"></a>
## 3. 💻 Claude Code A'dan Z'ye

### 3.1 Kurulum (3 Yöntem)

```bash
# 1. Native binary (önerilen, en hızlı)
curl -fsSL https://claude.ai/install.sh | bash

# 2. Homebrew (macOS)
brew install --cask claude-code

# 3. NPM (deprecated - migrate edilecek)
npm install -g @anthropic-ai/claude-code
```

Doğrula:
```bash
claude --version
claude --help
```

### 3.2 Authentication

```bash
claude auth login      # Login/account değiştir
claude auth status     # Mevcut auth durumu
claude auth logout     # Credentials sil
```

**Antigravity içinde Claude Code extension:**
Antigravity'de Extensions → Claude Code → Spark eklendiğinde, **iki yol** var:
1. **Anthropic API key** (Console'dan al, credit ekle): `/login` ile gir → Anthropic faturalandırır
2. **Antigravity proxy** (Google quota'sıyla): `antigravity-claude-proxy` kullan → Google faturalandırır (preview'da bedava)

### 3.3 Oturum Başlatma

```bash
cd ~/projects/my-project
claude                              # Yeni oturum aç
claude -c                           # Son oturumu devam ettir
claude -r <session-id>              # Belirli oturumu devam ettir
claude --from-pr <pr-url>           # PR'a bağlı oturum
claude "fix the date bug in dates.ts"  # Tek prompt, oturum aç
claude -p "review my changes"       # Non-interactive (print mode), CI için
```

### 3.4 CLI Flag'leri (En Kritikler)

| Flag | İşlev |
|---|---|
| `--model <name>` | Model belirt (sonnet, opus, haiku, opusplan) |
| `--print` / `-p` | Tek prompt çalıştır, çık (script için) |
| `--output-format json` | Yapısal çıktı (CI/pipeline için) |
| `--system-prompt-file <path>` | Sistem prompt'unu dosyadan oku |
| `--append-system-prompt <text>` | Default'a ek prompt yapıştır |
| `--add-dir <path>` | Ek klasörü context'e ekle |
| `--agents '<json>'` | CLI'dan dinamik subagent tanımla |
| `--debug` | Hook ve MCP çağrılarını izle |
| `--continue` / `-c` | Son oturumu sürdür |
| `--resume` / `-r <id>` | Spesifik oturuma dön |

### 3.5 @-Mentions (Dosya Referansı)

Yapıştırmak yerine **referans ver** — daha az token, daha doğru kayıt:

```
@README.md                          # Dosya ekle
@src/components/                    # Klasör ekle (özyinelemeli)
@https://example.com/docs           # URL fetch et
@!`git diff HEAD`                   # Shell çıktısı ekle (dinamik)
```

### 3.6 Shell Komutlarını Çalıştırma

Oturumda doğrudan shell çalıştırmak için `!` ile başla:

```
!ls -la src/
!npm test
!git log --oneline -10
```

Bu Claude'a şu komutu çalıştır demek; çıktıyı görüp üzerine sohbet edersin.

### 3.7 Klavye Kısayolları (Interactive Mode)

| Kısayol | İşlev |
|---|---|
| `Shift+Tab` | Mod döngüsü: normal → auto-accept → plan |
| `Ctrl+O` | Verbose transcript toggle |
| `Cmd+Enter` (Mac) / `Ctrl+Enter` (Linux/Win) | Submit (extension'da) |
| `Ctrl+C` (2 kez) | Oturumdan çık |
| `Esc` | Mevcut işlemi iptal et |
| `↑` / `↓` | Önceki prompt'ları gez |

### 3.8 Klavye Bağlamalarını Özelleştirme

`~/.claude/keybindings.json` dosyasını düzenle. Değişiklik anında etkili.

### 3.9 Model Seçimi (Mayıs 2026 itibariyle)

| Model | Alias | Ne için | Ne zaman değil |
|---|---|---|---|
| **Opus 4.7** | `opus` | Mimari karar, çok dosyalı zor refactor, ciddi debug | Basit formatla, lint fix |
| **Sonnet 4.6** | `sonnet` | Günlük iş, görevlerin %80'i | Token bütçesi sıkıysa Haiku |
| **Haiku 4.5** | `haiku` | Formatla, küçük edit, lint hatası | Çoklu dosya reasoning |
| **opusplan** | `opusplan` | Plan modunda Opus, build modunda Sonnet (token tasarrufu) | — |

```bash
/model opus           # Switch to Opus
/model sonnet         # Switch to Sonnet
/model haiku          # Switch to Haiku
/effort xhigh         # Opus 4.7 only, kodlama için önerilen
```

**Uyarı:** Opus 4.7'nin yeni tokenizer'ı aynı metin için **%35'e kadar fazla token** üretir. "Her şeye Opus" en pahalı junior hatasıdır.

### 3.10 Konfigürasyon Hiyerarşisi

Claude Code, ayarları şu sırayla okur (sonraki üsttekini override eder):

1. `~/.claude/settings.json` — kullanıcı geneli (her projede)
2. `.claude/settings.json` — proje geneli (git'e commit'li, takım paylaşımı)
3. `.claude/settings.local.json` — sadece bana (gitignore'da olmalı)
4. Enterprise managed settings — kurumsal politikalar

**Aynı şekilde** skills, agents, commands için:
- `~/.claude/skills/` — kullanıcı
- `.claude/skills/` — proje
- `~/.claude/agents/` ve `.claude/agents/`
- `~/.claude/commands/` ve `.claude/commands/` (legacy; skills tercih)

### 3.11 CLAUDE.md Hiyerarşisi

Claude Code, her oturumda şu sırayla CLAUDE.md dosyalarını okur:

1. `~/.claude/CLAUDE.md` — kişisel global kurallar
2. Project root'tan başlayıp aşağı doğru: `./CLAUDE.md`, `./src/CLAUDE.md`, vs.
3. `--add-dir` ile eklenen klasörlerdekiler (eğer `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` set'liyse)

**Hepsi context'e yüklenir.** Bu yüzden kısa tut.

---

<a name="4-slash-komutlari"></a>
## 4. ⚡ Slash Komutları — Tam Referans

Slash komutları oturumun **kontrol panelidir**. 60+ built-in komut var. Aşağıda kategorize ettim.

### 4.1 Session & Context Yönetimi

| Komut | Açıklama | Profesyonel Kullanım |
|---|---|---|
| `/init` | CLAUDE.md oluştur (interactive flow için `CLAUDE_CODE_NEW_INIT=1`) | Her yeni proje, ilk komut |
| `/clear` | Tüm context'i sıfırla, fresh start | Konu değişince refleks |
| `/compact [retain X]` | Context'i özetle, anahtar bilgiyi koru | %70'te (varsayılan %95'ten önce) |
| `/branch` (alias `/fork`) | Mevcut oturumu yeni bir branch'e ayır | "Aynı problemi başka türlü deneyeceğim" |
| `/ask` | Ephemeral yan soru (ana context kirletmez) | "Bu sırada arada şunu sorayım" |
| `/continue` | Compact sonrası akışa devam | — |
| `/resume <id>` | Spesifik oturuma dön | — |

### 4.2 Memory & Bilgi

| Komut | Açıklama |
|---|---|
| `/memory` | CLAUDE.md ve diğer memory dosyalarını yönet |
| `/release-notes` | Son sürüm notlarını göster |
| `/doctor` | Sistem sağlık kontrolü (config sorunları için) |

### 4.3 Model & Permission

| Komut | Açıklama |
|---|---|
| `/model [opus\|sonnet\|haiku\|opusplan]` | Model değiştir |
| `/effort [low\|medium\|high\|xhigh]` | Reasoning effort ayarla (Opus 4.7) |
| `/fast` | Fast Mode aç/kapa (aynı model, hız-optimize API) |
| `/plan` | Plan mode toggle (her tool çağrısı onay ister) |
| `/permissions` | Allow/deny kurallarını düzenle |
| `/less-permission-prompts` | Geçmiş çağrıları analiz et, allowlist oluştur (v2.1.111+) |
| `/keybindings` | Klavye bağlamalarını düzenle |
| `/auto-mode` | Auto mode toggle (Max sub için varsayılan açık, v2.1.112+) |

### 4.4 Extension Yönetimi

| Komut | Açıklama |
|---|---|
| `/mcp` | MCP server'ları yönet (list/add/remove/test) |
| `/agents` | Subagent'ları yönet ve test et |
| `/plugin` | Plugin marketplace + yükleme |
| `/skills` | Skill'leri listele |
| `/hooks` | Hook konfigürasyonunu düzenle |

### 4.5 Bilgi & İstatistik

| Komut | Açıklama |
|---|---|
| `/usage` | Canonical usage dashboard (plan limit + rate limit + maliyet + günlük session, v2.1.118+) |
| `/cost` | Maliyet sekmesi (alias) |
| `/stats` | Günlük usage, sessions, streaks (alias) |
| `/help` | Tüm komutları listele (kendi custom'ların dahil) |
| `/context` | Mevcut context kullanımını göster |
| `/sessions` | Oturum geçmişi |

### 4.6 Tema & Görünüm

| Komut | Açıklama |
|---|---|
| `/theme` | Tema seç / custom tema ekle (`~/.claude/themes/<name>.json`, v2.1.118+) |
| `/output-style` | Output stylesheet değiştir |

### 4.7 Workflow Yardımcıları

| Komut | Açıklama |
|---|---|
| `/review` | Mevcut değişikliklerin code review'unu yap |
| `/install-github-app` | GitHub Actions entegrasyonunu kur |
| `/team-onboarding` | Takım üyesi için onboarding rehberi üret (built-in) |
| `/buddy` | 🐣 Easter egg: terminal pet (April Fools sürprizi, v2.1.89+) |

### 4.8 MCP Prompts (otomatik gelir)

Bağladığın her MCP server kendi prompt'larını ekler. Format:
```
/mcp__<server-name>__<prompt-name>
```

Örnek (GitHub MCP):
```
/mcp__github__list_prs
/mcp__github__create_issue
/mcp__github__review_pr
```

### 4.9 Plugin Commands

Plugin'lerden gelir, namespace'lidir:
```
/frontend-design:frontend-design
/connect-apps:send-email
/claude-code-builder:create-skill
```

### 4.10 Custom Skills (eski custom commands)

Senin yazdığın skill'ler. `.claude/skills/<name>/SKILL.md` veya `~/.claude/skills/<name>/SKILL.md`:

```
/my-skill
/conventional-commit
/api-endpoint
```

> **Önemli:** v2.1.101 (Nisan 2026) ile **custom slash commands ve skills birleşti**. Eski `.claude/commands/*.md` dosyaları hâlâ çalışır, ama yeni dünyada `.claude/skills/<name>/SKILL.md` formatı tercih edilir. Aynı isimli skill ve command varsa **skill** kazanır.

### 4.11 Profesyonel Komut Yazma Pattern'leri

**Pattern 1: Argument templating**
```markdown
---
name: fix-issue
description: GitHub issue'sunu çöz
---
Fix issue #$ARGUMENTS following our coding standards.
```
Kullanım: `/fix-issue 123` → `$ARGUMENTS` = `"123"`

**Pattern 2: Dynamic context injection**
```markdown
---
name: commit
description: Bağlamlı git commit
allowed-tools: Bash(git *)
---
## Context
- Status: !`git status`
- Diff: !`git diff HEAD`

Generate a conventional commit message and commit.
```

**Pattern 3: Model pinning (kritik komutlar için)**
```markdown
---
name: security-audit
description: Tam güvenlik denetimi
model: claude-opus-4-7
---
Audit for OWASP Top 10...
```

**Pattern 4: Tool restriction**
```markdown
---
name: readonly-explore
allowed-tools: Read, Grep, Glob
---
Sadece okumaya izin var, hiçbir şey yazma.
```

**Pattern 5: Namespacing**
```
/refactor/rename-pattern
/test/add-edge-cases
/db/migration-draft
```

---

<a name="5-skills"></a>
## 5. 🧠 Skills — Yetenek Sistemi

### 5.1 Nedir?

**Skill = Claude'a bir görev türünü nasıl yapacağını öğreten markdown paketi.**

- Bir **klasör** + içinde `SKILL.md` (zorunlu) + opsiyonel destek dosyaları
- **Progressive disclosure**: ilk başta sadece name+description okunur (~100 token/skill), gerektiğinde tam içerik (<5K token) yüklenir
- **Açık standart**: Claude Code, Codex, Cursor, Gemini CLI, Antigravity, Windsurf hepsi destekler

### 5.2 Skill vs Slash Command vs Subagent — Mental Model

| | Slash Command (legacy) | Skill | Subagent |
|---|---|---|---|
| **Tetikleyici** | `/komut` yazınca | `/komut` veya Claude otomatik karar verir | Claude otomatik veya `/agents` ile |
| **Context** | Ana oturumda | Ana oturumda | **Ayrı context window** |
| **Amaç** | Hızlı prompt şablonu | Yeniden kullanılabilir bilgi paketi | İzole, uzmanlaşmış iş |
| **Token maliyeti** | Yüksek (tam yüklü) | Düşük (progressive) | Orta (izole ama paralel) |

**Şöyle düşün:** Skills = bilgi. Subagents = işçi. Plugins = paketlenmiş ekip.

### 5.3 SKILL.md Yapısı

```markdown
---
name: conventional-commit
description: Use this skill any time the user asks for a git commit. Writes messages in Conventional Commit format.
---

# Conventional Commit

When you create a git commit, follow these rules:

1. Start the subject line with one of: feat, fix, chore, docs, refactor, test, perf
2. Add a colon and a space, then a short imperative summary, no period
3. Maximum 72 characters
4. If the change is breaking, add ! before the colon

## Examples

Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication

Input: Fixed null pointer in date parser
Output: fix(parser): handle null input in formatDate

## YAPMA

- Asla "WIP" veya "tmp" gibi belirsiz mesaj kullanma
- Body'de "this commit" deme; bunun bir commit olduğu zaten belli
- Type kısmını büyük harfle yazma
```

### 5.4 Frontmatter Alanları

| Alan | Zorunlu | Ne işe yarar |
|---|---|---|
| `name` | ✅ | Skill ismi (64 char max) ve `/komut` adı |
| `description` | ✅ | Claude bunu okuyup ne zaman kullanacağına karar verir (200 char max). **En kritik alan.** |
| `allowed-tools` | ❌ | Sadece izin verilen tool'lar |
| `argument-hint` | ❌ | `/komut [arg]` için ipucu |
| `model` | ❌ | Hangi modelle çalışsın |
| `disable-model-invocation` | ❌ | `true` ise sadece manuel `/komut` çalışır, Claude otomatik tetikleyemez |
| `user-invocable` | ❌ | `false` ise sadece Claude otomatik çağırır |
| `dependencies` | ❌ | Skill'in ihtiyaç duyduğu paketler |

### 5.5 Skill Yerleri (Scope)

```
~/.claude/skills/<name>/SKILL.md     # Kullanıcı geneli (her projede)
.claude/skills/<name>/SKILL.md       # Proje geneli (git'e commit, ekip paylaşımı)
plugins/<plugin>/skills/<name>/      # Plugin içinden gelir
```

### 5.6 Built-in Skills (Bundled)

Claude Code 5+ bundled skill ile gelir:
- **task-orchestration** — karmaşık iş kalabalığını alt görevlere böl
- **troubleshooting** — debug loops, root cause narrowing
- **monitoring** — repeated check (oturum açıkken)
- **anthropic-api-helper** — API spesifik iş
- **plan-management** — plan oluşturma ve takip

### 5.7 Profesyonel Skill Örnekleri

#### Örnek 1: Test runner skill
```markdown
---
name: run-tests
description: Run tests matching a pattern. Use when user says "test", "run tests", "kontrol et", or asks to verify changes.
allowed-tools: Bash(npm *), Bash(npx *), Read, Edit
argument-hint: [pattern]
---

Run tests matching: $ARGUMENTS

1. Detect framework (jest, vitest, pytest)
2. Run with given pattern, or all if empty
3. If failures: analyze, propose fix, re-run
4. Report: X passed, Y failed
```

#### Örnek 2: Clean imports skill
```markdown
---
name: clean-imports
description: Remove unused imports and sort the rest. Use any time the user asks to clean imports, sort imports, or tidy imports.
---

For each file the user asks to clean:

1. Remove imports not referenced in file
2. Sort remaining by: standard library, third-party, local
3. Group sections with blank line between

Do not touch side-effect imports (imports without name binding).
```

#### Örnek 3: PR description generator
```markdown
---
name: pr-description
description: Generate a PR description from git diff. Use when the user asks for a PR description, summary of changes, or "what changed".
---

## Diff
!`git diff main...HEAD`

## Commits
!`git log main..HEAD --oneline`

## Görev
Above context'ten:

1. **Summary** (1-2 cümle): bu PR ne yapıyor
2. **Changes** (madde madde): hangi dosya neden değişti
3. **Testing**: nasıl test edilebilir
4. **Breaking changes**: varsa belirt
5. **Screenshots**: UI değişikliği varsa hatırlat (görüntü ekle)
```

### 5.8 Skill Yazma Best Practice

1. **Description, description, description** — en kritik alan. Claude bunu okuyup karar verir. "Use this skill when..." ile başla. Tetikleyici kelimeleri ekle.
2. **500 satırın altında kal**. Daha uzunsa REFERENCE.md ekle, oradan referans ver.
3. **Imperative form kullan**: "Do X" değil "Write Y".
4. **Negative instructions ekle**: "Do not do X" — Claude'u default davranışından alıkoyar.
5. **Examples kritik**: Input → Output paterni göster.
6. **Tek sorumluluk**: mega-skill yazma. Bir skill bir iş.

### 5.9 Skill Test Etme

Anthropic'in **Skill Creator** skill'i interaktif Q&A ile yazar:
```bash
# Anthropic'in resmi skill creator'ını yükle
git clone https://github.com/anthropics/skills ~/.claude/skills-temp
cp -r ~/.claude/skills-temp/skills/skill-creator ~/.claude/skills/
```

Sonra Claude Code'da:
```
/skill-creator
```

---

<a name="6-mcp"></a>
## 6. 🔌 MCP — Model Context Protocol

### 6.1 MVP ≠ MCP Açıklaması

- **MVP** (Minimum Viable Product): Yazılım terminolojisi. "Satılabilir/test edilebilir en küçük ürün". Eric Ries'in *Lean Startup* kitabından.
- **MCP** (Model Context Protocol): Anthropic'in geliştirdiği açık protokol. Claude (veya başka LLM) ile dış servisleri konuşturmak için.

Bu doküman MCP üzerine. MVP üretme playbook'u için → [Bölüm 12](#12-mvp).

### 6.2 MCP Nedir?

**Tek cümle:** MCP, Claude'a "şu API'yi şöyle konuş" diyen evrensel adaptör.

Bir MCP server şunları açar:
- **Tools** (yapma): "GitHub'da PR aç", "Postgres'te query çalıştır"
- **Resources** (okuma): dosya, doküman, veri
- **Prompts** (hazır şablon): `/mcp__github__create_pr` gibi

### 6.3 Transport Türleri

| Transport | Ne için | Örnek |
|---|---|---|
| **stdio** | Lokal subprocess (npm/pip paketi) | `@modelcontextprotocol/server-filesystem` |
| **http** | Cloud-tabanlı servis (modern, önerilen) | Notion MCP, GitHub MCP |
| **sse** | Server-sent events (legacy, http'ye geçişte) | Eski cloud server'lar |

### 6.4 Kurulum Komutları

```bash
# Stdio (yerel paket)
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /Users/me/projects

# Stdio with env vars
claude mcp add airtable \
  --env AIRTABLE_API_KEY=$AIRTABLE_KEY \
  -- npx -y airtable-mcp-server

# HTTP (önerilen modern cloud)
claude mcp add --transport http notion https://mcp.notion.com/mcp

# HTTP with auth header
claude mcp add --transport http github-api https://api.github.com/mcp \
  --header "Authorization: Bearer $GH_TOKEN"

# SSE (legacy, mümkünse http kullan)
claude mcp add --transport sse my-service https://my-service.internal/mcp

# JSON config ile
claude mcp add-json myserver '{"command": "npx", "args": ["-y", "@pkg/mcp"]}'

# Listele
claude mcp list

# Detay
claude mcp get <name>

# Sil
claude mcp remove <name>

# Test (kurmadan)
claude mcp run <name>

# Claude Desktop config'inden import
claude mcp add-from-claude-desktop
```

### 6.5 settings.json İçinde MCP

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://readonly@localhost/mydb"
      ]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@executeautomation/playwright-mcp-server"]
    }
  }
}
```

### 6.6 Önerilen MCP Stack (Tier'lara Göre)

#### Tier 1 — Minimum (her geliştirici)
| MCP | Ne yapar | Komut |
|---|---|---|
| **GitHub** | PR, issue, code search | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` (env: GH token) |
| **Context7** | Güncel dokümantasyon (kütüphane sürümüne göre) | `claude mcp add context7 -- npx -y @upstash/context7-mcp` |
| **Playwright** | Browser automation, E2E test | `claude mcp add playwright -- npx -y @playwright/mcp` |

#### Tier 2 — Full-stack
Üst tier + şunlar:
| MCP | Ne için |
|---|---|
| **Postgres** | Read-only DSN ile DB schema + query |
| **Supabase** | Tek MCP'de DB + auth + storage |
| **Sentry** | Production hatalarını çekme |
| **Linear** veya **Jira** | Ticket entegrasyonu |

#### Tier 3 — Power user
Üst tier + şunlar:
| MCP | Ne için |
|---|---|
| **Memory** | Knowledge graph, oturumlar arası hafıza |
| **Slack** | Takım iletişimi |
| **Brave Search** / **Exa** | Web search |
| **Figma** | Design-to-code |
| **Vercel** / **Cloudflare** | Deployment |

### 6.7 MCP Güvenlik Uyarıları

- **Token'ları hardcode etme** — env var kullan
- **Postgres MCP'ye write erişimi verme** — read-only DSN
- **Untrusted MCP server kullanma** — SKILL.md gibi MCP de keyfi kod çalıştırabilir
- **Her MCP context'i şişirir** — tool definition olarak yüklenir. 20 MCP = ~5000 token sabit vergi
- **Tool Search özelliği** (yeni) — gerekli olmayan tool'ları context'e yüklemez, %85'e kadar tasarruf

### 6.8 MCP Tuzakları

- **Cursor config'ini Claude Code'a kopyalama**: Cursor `"mcpServers"` kullanır, VS Code `"servers"`. Format kontrolü şart.
- **`npx -y` her zaman latest fetch eder**: production agent için sürüm pinle (`@upstash/context7-mcp@1.4.2`)
- **OAuth-only fallback unutma**: headless agent'lar redirect tamamlayamaz. API-key fallback'i kullan
- **6'dan fazla MCP yükleme**: kararsızlığa yol açar, agent karar veremez

### 6.9 Antigravity'deki MCP vs Claude Code MCP

| | Antigravity MCP | Claude Code MCP |
|---|---|---|
| Config dosyası | Antigravity Settings UI | `~/.claude/settings.json` |
| One-click install | ✅ | ❌ (manuel) |
| Görünür | Antigravity agent'larına | Claude Code session'ına |
| Aynı server, iki kez kurma | ✅ | — |

**Pratik kural:** İkisinde de aynı MCP'leri kurarsan en az sürtünme yaşarsın.

---

<a name="7-subagents"></a>
## 7. 🤖 Subagents — Paralel Akıllar

### 7.1 Nedir?

**Subagent = Ana Claude'un içine gömülü ayrı bir Claude.**

- Kendi context window'u var (ana sohbeti kirletmez)
- Kendi sistem prompt'u (uzmanlaşmış davranış)
- Kendi tool izinleri (kısıtlı yetki)
- Sonucu özet olarak ana sohbete döner

**Faydası:** Codebase keşfi gibi geniş context yiyen işleri izole edersin. 10K+ token tasarruf.

**Maliyeti:** Subagent ana context'ten habersizdir. Holistik akıl yürütme gerektiren işlerde çalışmaz.

### 7.2 Subagent Yerleri

```
.claude/agents/<name>.md       # Proje agent'ı
~/.claude/agents/<name>.md     # Kullanıcı agent'ı
plugins/<plugin>/agents/       # Plugin agent'ı
```

Çakışma kuralı: project > user > plugin

### 7.3 Subagent Dosya Formatı

```markdown
---
name: security-reviewer
description: Expert security code reviewer. Use PROACTIVELY after code changes to auth or data handling.
tools: Read, Grep, Glob, Bash
model: opus
permissionMode: plan
isolation: worktree
---

You are a senior security engineer specializing in OWASP Top 10.

When invoked:
1. Run `git diff HEAD` to see recent changes
2. Focus on files touching auth, data handling, input validation
3. Check for:
   - SQL injection risks
   - XSS vulnerabilities
   - Hardcoded credentials
   - Insecure deserialization
   - Missing rate limiting

Report findings as:
- **Critical**: must fix before merge
- **Warning**: should fix soon
- **Suggestion**: nice to have

For each finding: file:line, issue description, suggested fix.
```

### 7.4 Tüm Frontmatter Alanları

| Alan | Açıklama |
|---|---|
| `name` ⭐ | Agent ismi (zorunlu) |
| `description` ⭐ | Ne zaman kullanılır (zorunlu) |
| `tools` | Allow-list: izin verilen tool'lar |
| `disallowedTools` | Deny-list: yasak tool'lar |
| `model` | `opus`, `sonnet`, `haiku`, veya tam ID (`claude-opus-4-7`) |
| `permissionMode` | `plan`, `default`, `bypassPermissions` |
| `mcpServers` | Sadece bu MCP'lere erişim |
| `hooks` | Agent'a özel hook'lar |
| `maxTurns` | Max kaç tur (sonsuz döngü önler) |
| `skills` | Otomatik yüklenecek skill listesi |
| `initialPrompt` | Başlangıç mesajı |
| `memory` | Hafıza dosyası path'i |
| `effort` | Reasoning effort seviyesi |
| `background` | `true` ise asenkron çalışır |
| `isolation` | `worktree` = ayrı git worktree |
| `color` | UI'de renk |

### 7.5 Skills + Subagents = Süper Güç

Bir subagent'a skill'leri otomatik yükletebilirsin:

```markdown
---
name: api-developer
description: Implement API endpoints following team conventions
skills:
  - api-conventions
  - error-handling-patterns
  - validation-rules
---

Implement API endpoints following the preloaded skills' conventions.
```

Bu agent başlarken 3 skill içeriği context'inde olur — sen tekrar açıklamak zorunda kalmazsın.

### 7.6 Profesyonel Subagent Setleri

#### Set 1: Code Quality
```
code-reviewer.md       # Genel review, OWASP odaklı
type-checker.md        # TypeScript strict, no any
performance-auditor.md # Bottleneck detection
```

#### Set 2: Documentation
```
docs-writer.md         # JSDoc/Python docstring
readme-updater.md      # README sync
changelog-generator.md # Conventional commits → changelog
```

#### Set 3: Investigation
```
codebase-explorer.md   # "Auth nasıl çalışıyor?" tipi keşif
bug-reproducer.md      # Hatayı izole et
log-analyzer.md        # Production log'u tara
```

### 7.7 CLI'dan Dinamik Subagent

```bash
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer",
    "prompt": "You are a senior code reviewer...",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  }
}'
```

Bu agent oturumla beraber yaşar, dosyaya kaydedilmez.

### 7.8 Agent Teams (Experimental)

`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` env var'ı set'liyse çalışır. Bir lead agent + birden fazla teammate task list üzerinden koordine olur. Çoğu kullanıcı için aşırı; subagent'lar yeter.

---

<a name="8-hooks"></a>
## 8. 🪝 Hooks — Otomatik Tetikleyiciler

### 8.1 Nedir? (Skills'ten Farkı)

| | Skill | Hook |
|---|---|---|
| Tetikleyici | Claude karar verir | Lifecycle event'i |
| Çalışan | LLM | Shell script |
| Güvenilirlik | Model unutabilir | %100 deterministik |
| Amaç | Akıl yürütme | Garanti edilmiş aksiyon |

**Şöyle düşün:** Prompt'lar önerir, hook'lar **garantiler**. Format, lint, security gate hook'a aittir.

### 8.2 13 Lifecycle Olayı

```
Session Lifecycle:
├── Setup (init/maintenance)
├── SessionStart (startup/resume/clear)
└── SessionEnd (exit/sigint/error)

Main Loop:
├── UserPromptSubmit
├── UserPromptExpansion (slash command genişlerken)
└── Tool Execution
    ├── PreToolUse 🔒
    ├── PermissionRequest ❓
    ├── PostToolUse ✅
    └── PostToolUseFailure ❌

Subagent Lifecycle:
├── SubagentStart 🚀
└── SubagentStop 🏁

Maintenance:
├── PreCompact 📦
└── Notification 🔔

Termination:
└── Stop / StopFailure 🛑
```

### 8.3 Hook Konfigürasyonu

`.claude/settings.json` içinde:

```json
{
  "hooks": {
    "<EventName>": [
      {
        "matcher": "<RegexPattern>",
        "hooks": [
          {
            "type": "command",
            "command": "<shell command>",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### 8.4 Exit Code Anlamları

| Exit Code | Anlam |
|---|---|
| `0` | Başarılı, devam et |
| `2` | **Bloklama** — tool çağrısını engelle, stderr Claude'a hata olarak verilir |
| Diğer | Non-blocking hata, log'a yazılır |

### 8.5 Hook Tip'leri

```json
// 1. Shell command
{ "type": "command", "command": "npm run lint" }

// 2. HTTP webhook
{ "type": "http", "url": "http://localhost:8080/hook", "timeout": 30 }

// 3. Prompt (lightweight LLM çağrısı)
{ "type": "prompt", "prompt": "Evaluate if this is safe: $TOOL_INPUT" }

// 4. MCP tool
{ "type": "mcp", "tool": "myserver.evaluate" }
```

### 8.6 Ortam Değişkenleri

Hook'ların erişebildiği değişkenler:

| Değişken | Ne |
|---|---|
| `$CLAUDE_PROJECT_DIR` | Proje root path |
| `$CLAUDE_PLUGIN_ROOT` | Plugin klasörü (portable path için) |
| `$CLAUDE_FILE_PATH` | İlgili dosya path |
| `$CLAUDE_TOOL_NAME` | Tool ismi |
| `$CLAUDE_TOOL_INPUT` | Tool input JSON |
| `$CLAUDE_TOOL_RESULT` | (PostToolUse) tool sonucu |
| `$CLAUDE_SESSION_ID` | Oturum ID |
| `$CLAUDE_ENV_FILE` | (SessionStart) env var kalıcı yazma |
| `$CLAUDE_CODE_REMOTE` | Remote context'te mi |
| `$USER_PROMPT` | (UserPromptSubmit) prompt metni |

### 8.7 Profesyonel Hook Örnekleri

#### Hook 1: Auto-format on file edit
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
          }
        ]
      }
    ]
  }
}
```

#### Hook 2: Block dangerous commands
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -qE '(rm -rf|sudo rm|chmod 777|:(){:|:&};:)'; then echo 'Dangerous command blocked!' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

#### Hook 3: Block secret file access
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read|Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$CLAUDE_FILE_PATH\" | grep -qE '\\.(env|pem|key)$'; then echo 'Secret file access blocked' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

#### Hook 4: SessionStart context loader
```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo '## Git Status'; git status --short; echo '## TODO Comments'; grep -rn 'TODO:' src/ 2>/dev/null | head -5"
          }
        ]
      }
    ]
  }
}
```

#### Hook 5: Test-on-commit enforcement
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -q 'git commit'; then npm test --silent || (echo 'Tests must pass before commit' >&2 && exit 2); fi"
          }
        ]
      }
    ]
  }
}
```

#### Hook 6: Notification on completion
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude finished\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

### 8.8 Hook Performans Kuralları

- **Her hook senkron** çalışır; total süre tool call'a eklenir
- 500ms üzeri PostToolUse hook → oturum yavaşlamaya başlar
- Çok sayıda hızlı hook > az sayıda yavaş hook
- `time` ile profile et: `time bash my-hook.sh`

### 8.9 `/hooks` Komutu — Interactive UI

```
/hooks                # Hook yönetim ekranını aç
```

Yeni event seç → matcher gir → command yapıştır → kaydet. JSON elle yazma zahmetinden kurtulursun.

---

<a name="9-plugins"></a>
## 9. 📦 Plugins — Paketlenmiş Eklentiler

### 9.1 Nedir?

**Plugin = Skills + Subagents + Hooks + MCP servers + Commands paketi.**

Bir plugin = tek isim altında tüm uzantı türlerini bir araya getirir. Ekibin için tek kurulum, tek güncelleme.

### 9.2 Plugin Kurulum

```bash
# Marketplace ekle
/plugin marketplace add <github-org/repo>

# Plugin yükle
/plugin install <plugin-name>@<marketplace>

# Listele
/plugin list

# Detay
/plugin details <plugin-name>

# Kaldır
/plugin uninstall <plugin-name>
```

### 9.3 Marketplace Yapısı

Plugin repo'sunda:
```
my-plugin-marketplace/
├── .claude-plugin/
│   └── marketplace.json     # Plugin listesi
└── plugins/
    └── my-plugin/
        ├── .claude-plugin/
        │   └── plugin.json  # Manifest
        ├── agents/
        ├── skills/
        ├── commands/
        ├── hooks/
        └── README.md
```

### 9.4 Önerilen Plugin Repository'leri

| Repo | Ne içerir | Kim için |
|---|---|---|
| `wshobson/agents` | 81 plugin, 200+ skill | Genel kullanım |
| `anthropics/skills` | Resmi skill örnekleri | Öğrenme |
| `ccplugins/awesome-claude-code-plugins` | Curated plugin listesi | Keşif |
| `alirezarezvani/claude-skills` | 245+ skill (engineering, marketing, compliance) | Geniş yelpaze |
| `hyperskill/claude-code-marketplace` | Hyperskill takım plugin'leri | Production-ready |
| `alexanderop/claude-code-builder` | Plugin/skill/agent oluşturucu | Yeni başlayan plugin geliştiriciler |

### 9.5 Plugin Yazma Mini-Tutorial

`.claude-plugin/plugin.json`:
```json
{
  "name": "my-awesome-plugin",
  "version": "1.0.0",
  "description": "Does awesome things",
  "author": "Senin İsmin",
  "repository": "github.com/sen/my-plugin"
}
```

`.claude-plugin/marketplace.json` (eğer kendi marketplace'in olsun istiyorsan):
```json
{
  "name": "my-marketplace",
  "plugins": [
    {
      "name": "my-awesome-plugin",
      "path": "./plugins/my-awesome-plugin"
    }
  ]
}
```

Lokal test:
```bash
cd my-plugin-repo
claude
/plugin marketplace add .
/plugin install my-awesome-plugin@my-marketplace
```

---

<a name="10-settings"></a>
## 10. ⚙️ settings.json — Tüm Ayarlar

### 10.1 Konum Hiyerarşisi (yeniden hatırlatma)

1. `~/.claude/settings.json` — user
2. `.claude/settings.json` — project (git'e commit)
3. `.claude/settings.local.json` — local override (gitignore)
4. Enterprise managed settings — top priority

### 10.2 Tam Şema (Komple Örnek)

```json
{
  "model": "claude-sonnet-4-6",
  "maxTokens": 4096,
  "autoCompactThreshold": 0.7,
  "effort": "high",
  "fastMode": false,

  "permissions": {
    "allowedTools": [
      "Read",
      "Write",
      "Edit",
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(npx *)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Write(./production.config.*)",
      "Bash(rm -rf *)",
      "Bash(sudo *)"
    ],
    "ask": [
      "Bash(git push *)",
      "Write(./public/*)"
    ]
  },

  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "python -m black \"$CLAUDE_FILE_PATH\""
          }
        ]
      },
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "git status --short && echo '---' && git log --oneline -5"
          }
        ]
      }
    ]
  },

  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    }
  },

  "subagents": {
    "autoDelegate": true,
    "maxParallel": 5
  },

  "skills": {
    "autoInvoke": true
  },

  "ui": {
    "theme": "tokyo-night",
    "statusLine": "tokens-and-cost"
  },

  "enabledPlugins": [
    "frontend-design",
    "connect-apps"
  ]
}
```

### 10.3 Permissions — Granüler Kontrol

3 seviye:
- **`allowedTools`**: izin verilen tool'lar (allow-list)
- **`deny`**: kesinlikle yasak (her zaman bloklar)
- **`ask`**: onay sorar (interactive)

**Pattern matching:** `Bash(git *)` = `git` başlayan tüm komutlar. `Read(./.env)` = sadece `.env` dosyası.

### 10.4 Status Line Özelleştirme

Token sayacını sürekli görmek için custom status line:

```bash
# ~/.claude/statusline.sh
#!/bin/bash
cat <<EOF
${CLAUDE_MODEL} | ${CLAUDE_TOKENS_USED}/${CLAUDE_TOKENS_TOTAL} tokens | \$${CLAUDE_COST}
EOF
```

```json
{
  "ui": {
    "statusLine": "/Users/me/.claude/statusline.sh"
  }
}
```

### 10.5 Environment Variables

| Var | Açıklama |
|---|---|
| `CLAUDE_CODE_NEW_INIT=1` | Interactive `/init` flow |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | `--add-dir` klasörlerindeki CLAUDE.md'leri de yükle |
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | Agent Teams (deneysel) |
| `CLAUDE_CODE_REMOTE` | Remote context'te çalışıyorum (hook'lara bilgi) |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | Bedrock/Vertex için Opus modeli pinle |

---

<a name="11-workflows"></a>
## 11. 🎯 Workflow Pattern'leri — Profesyonel Kullanım

### 11.1 Pattern A: Plan-Build-Verify Loop

**Senaryosu:** Yeni feature, riskli/karmaşık.

```
1. Antigravity Manager → Yeni conversation aç (Gemini Pro)
2. Prompt: "Plan only, don't code: <feature description>"
3. Gemini Implementation Plan artifact'i üretir
4. Plan üzerinde Google Docs comment'leri ile düzelt
5. Plan onaylandıktan sonra → Editor View'a geç
6. Terminal'de Claude Code aç
7. Plan'i Claude'a ver: "Implement the plan in /artifacts/plan-001.md"
8. Claude implement eder
9. Playwright MCP ile E2E test
10. Claude'a PR description yazdır
```

**Kazanım:** Gemini token = ucuz/bedava (Antigravity quota). Claude token = sadece implementation. %40-60 token tasarrufu.

### 11.2 Pattern B: Paralel Multi-Agent (Worktree)

**Senaryosu:** Bağımsız 3 görev (frontend + backend + test).

```
1. Antigravity Manager View
2. 3 conversation aç, her birinde "Worktree toggle ON"
3. Agent 1 (frontend): "Build UserCard component, components/UserCard.tsx"
4. Agent 2 (backend): "Add /api/users endpoint with validation"
5. Agent 3 (test): "Write E2E test for user registration flow"
6. Manager View'da 3 agent paralel çalışır
7. Her biri artifact üretir
8. Onay → her worktree main'e merge
```

**Kazanım:** Tek developer 3x verimlilik.

### 11.3 Pattern C: Iterative Refinement (Comment-Driven)

**Senaryosu:** UI tweak'leri, müşteri feedback'i.

```
1. Antigravity'de Editor View
2. Sidebar chat ile component'i üret (Gemini Flash, hızlı)
3. Browser preview'da gör
4. Bir kısmı seç → Cmd+I → "Make this card more rounded, add shadow"
5. Tekrar gör
6. Müşteri "renk daha sıcak olsun" derse → comment yaz → agent uygular
```

**Kazanım:** Konuşma akışı bozulmaz, hızlı iterasyon.

### 11.4 Pattern D: Bug Investigation (Subagent-Heavy)

**Senaryosu:** Production'da garip bug.

```
1. Claude Code aç
2. /agents → "Use codebase-explorer to find all auth-related files"
3. Sonuç döner (subagent context izole)
4. /agents → "Use log-analyzer to scan /var/log/app.log for errors today"
5. Sonuç döner
6. Ana sohbette ikisini birleştir: "Combine findings, propose root cause"
7. Sonra implement
```

**Kazanım:** Ana context kirlenmedi, 20K+ token tasarruf.

### 11.5 Pattern E: Spec-Driven Development

**Senaryosu:** Müşteri brief'i → çalışan ürün.

```
1. Müşteri brief'i markdown olarak hazırla: brief.md
2. Gemini'ye ver: "Convert this to PRD with user stories and acceptance criteria"
3. PRD onaylan → "Generate architecture diagram (mermaid)"
4. Architecture onaylan → "Generate task breakdown (list of issues)"
5. Her task → ayrı Claude Code conversation
6. Her conversation: implement + test + PR
7. Sonunda demo browser recording (Antigravity)
```

**Kazanım:** Brief → MVP, 2-3 günde.

### 11.6 Antigravity'de Claude Code Extension Spesifik Workflow

Claude Code extension Antigravity içinde nasıl yaşar:

1. **Aç**: Sidebar'da Claude Code panel açılır
2. **Auth**: İlk açılışta `/log` ile login ol (Anthropic veya Antigravity proxy)
3. **MCP**: Antigravity'de kurduğun MCP'ler Claude Code'a **otomatik gelmez**. `~/.claude/settings.json`'da ayrıca kur
4. **`/init`**: Mevcut proje varsa, terminal'de `claude` çağır → `/init`
5. **`/mcp`**: MCP'leri Antigravity Extensions → Claude Code → Spark üzerinden de yönetebilirsin
6. **İş bölümü**: Gemini'yi sidebar'da, Claude Code'u terminal/panel'de tut

---

<a name="12-mvp"></a>
## 12. 🚀 MVP Üretme Playbook'u (yazılım terminolojisi)

**MVP = Minimum Viable Product.** Müşterinin gerçekten ödeyeceği en küçük ürün. Antigravity + Claude Code ile 1-3 günde çıkarılabilir.

### 12.1 7 Adımlı MVP Çıkarma

```
Gün 1 sabah  → BRIEF
Gün 1 öğle   → PRD + ARCHITECTURE
Gün 1 akşam  → SCAFFOLD
Gün 2 sabah  → AUTH + DATABASE
Gün 2 öğle   → CORE FEATURES
Gün 2 akşam  → UI POLISH + TEST
Gün 3        → DEPLOY + DEMO
```

### 12.2 Adım 1: Brief → PRD (Antigravity / Gemini)

```
"You are a product manager. Convert this rough idea into a PRD:

[müşteri brief'i]

Output:
1. User personas (3 max)
2. User stories (in format: As a X, I want Y, so that Z)
3. Acceptance criteria per story (Given/When/Then)
4. Non-functional requirements
5. Out of scope (what we WON'T build)
6. Success metrics

Keep ruthless. MVP is 'minimum viable'."
```

### 12.3 Adım 2: PRD → Architecture (Antigravity / Gemini)

```
"From the PRD above, propose:

1. Tech stack (justify each choice)
2. Folder structure
3. Database schema (mermaid ER diagram)
4. API endpoints (RESTful)
5. Auth strategy
6. Deployment target
7. Estimated cost ($)

Constraints:
- Stack: Next.js + Supabase + Vercel (unless strong reason not to)
- Time budget: 3 days
- Must be deployable to production"
```

### 12.4 Adım 3: Scaffold (Claude Code)

```bash
cd ~/projects
mkdir my-mvp && cd my-mvp
claude
```

```
"Following the architecture in /Users/me/Documents/architecture.md, scaffold a Next.js 15 project:

1. Initialize with create-next-app (TypeScript, Tailwind, App Router)
2. Set up Supabase client
3. Create folder structure per architecture
4. Add CLAUDE.md with stack info and conventions
5. Initialize git, first commit

After scaffold, run npm run dev and confirm it works."
```

### 12.5 Adım 4: Auth + DB (Claude Code, Supabase MCP)

```
"Implement authentication:

1. Supabase Auth with email/password
2. Login, signup, logout pages
3. Protected route middleware
4. User profile page
5. Migration for users table extension (if needed)

Use Supabase MCP to:
- Create the migration
- Apply it
- Test the auth flow

Test with Playwright MCP: signup → email confirm (mock) → login → profile → logout."
```

### 12.6 Adım 5: Core Features

Her feature için ayrı Claude conversation:

```
"Feature: <name from PRD>

Acceptance criteria:
<paste from PRD>

Constraints:
- Match existing component patterns (see /components)
- Use server actions, not API routes
- Add Vitest tests for business logic
- Add Playwright test for happy path

When done:
- Show me the diff
- Show me how to test manually
- Suggest commit message"
```

### 12.7 Adım 6: UI Polish (Antigravity inline)

Antigravity Editor View'da bileşeni seç → `Cmd+I`:
- "Make this more polished with subtle animations"
- "Add loading states"
- "Improve accessibility (ARIA labels, keyboard nav)"
- "Make mobile-responsive"

### 12.8 Adım 7: Deploy (Claude Code + Vercel MCP)

```
"Deploy to Vercel:

1. Check there are no console.log()s left
2. Check no hardcoded secrets
3. Set environment variables in Vercel (use Vercel MCP)
4. Configure custom domain if provided
5. Deploy preview first
6. Run smoke tests against preview URL (Playwright MCP)
7. If green, promote to production
8. Create release notes from git log

Output: production URL + smoke test results."
```

### 12.9 MVP Anti-Pattern'leri

❌ **Tüm feature'ları aynı conversation'da yapmak** → context şişer, kalite düşer
✅ Her feature ayrı conversation, worktree'de paralel

❌ **Test'i sona bırakmak** → bir feature bittiğinde geri dönüp test eklemek pahalı
✅ Her feature: implement + test aynı conversation'da

❌ **"Sonra ekleriz" diyerek auth/security'i atlamak**
✅ Auth ilk adımlardan biri, kıvama gelen ürün üzerine çakılmaz

❌ **MVP'ye 10 feature sokmak** → MVP değil, V1 olur
✅ Maks 3 user story, gerisi V2

---

<a name="13-self-updating-agent"></a>
## 13. ⭐ Self-Updating Documentation Agent

**İstediğin şey:** Bir agent günlük olarak internetten yeni Claude Code/Antigravity gelişmelerini taransın, bu dokümanı güncellesin. Mantıklı, hatta zekice. İşte implementation.

### 13.1 Mimari

```
┌──────────────────────────────────────────────────────────┐
│  Cron job (günlük 09:00)                                 │
│    │                                                      │
│    ▼                                                      │
│  Claude Code CLI çağır (-p print mode, JSON output)      │
│    │                                                      │
│    ▼                                                      │
│  Skill: doc-updater + Subagent: research-scout           │
│    │                                                      │
│    ├─→ WebFetch tool ile son blog yazılarını tara        │
│    ├─→ GitHub MCP ile son release notes'ları çek         │
│    ├─→ Mevcut dokümanı oku, farkları bul                 │
│    └─→ Yeni bölümler ekle, eski bölümleri güncelle       │
│         (CHANGELOG.md'ye de yaz)                          │
│    │                                                      │
│    ▼                                                      │
│  Git commit + push (varsa otomatik PR aç)                │
│    │                                                      │
│    ▼                                                      │
│  Notification (Slack/Discord/email)                      │
└──────────────────────────────────────────────────────────┘
```

### 13.2 Implementation Adımları

#### Adım 1: Subagent oluştur — `~/.claude/agents/research-scout.md`

```markdown
---
name: research-scout
description: Daily reconnaissance for new Claude Code and Antigravity features, releases, and best practices. Use ONLY when invoked by the doc-updater skill.
tools: WebFetch, WebSearch, Read, Grep
model: sonnet
maxTurns: 20
isolation: worktree
---

You are a research scout for Claude Code and Google Antigravity ecosystem updates.

Your mission, when invoked:

1. Check these sources for updates in the last 7 days:
   - https://code.claude.com/docs/en/release-notes
   - https://developers.googleblog.com (Antigravity tag)
   - https://github.com/anthropics/claude-code/releases
   - https://github.com/hesreallyhim/awesome-claude-code (recent commits)
   - https://github.com/anthropics/skills (recent commits)

2. For each new piece of info, classify it as:
   - **NEW FEATURE** (new slash command, hook event, etc.)
   - **DEPRECATION** (something removed/changed)
   - **BEST PRACTICE** (community pattern)
   - **TOOL** (new MCP server, skill, plugin)

3. Return a structured report:

```yaml
date: YYYY-MM-DD
items:
  - type: NEW_FEATURE
    title: "..."
    description: "..."
    source_url: "..."
    affected_section: "..."  # Bu dokümandaki hangi bölüm güncellenmeli?
    suggested_change: "..."
```

4. Do NOT modify any files. You only research.
5. If nothing significant found, return: `{ date: "...", items: [] }`
```

#### Adım 2: Skill oluştur — `~/.claude/skills/doc-updater/SKILL.md`

```markdown
---
name: doc-updater
description: Update the Claude Code + Antigravity reference document with new findings. Use when invoked by the daily cron or when user says "update docs".
allowed-tools: Read, Edit, Write, Bash(git *)
---

# Documentation Updater

When invoked:

1. **Invoke the research-scout subagent** with: "Run daily reconnaissance"

2. Wait for structured report.

3. If `items` is empty: log "No updates today" and exit.

4. For each item in the report:
   - Read the affected section of `~/Documents/claude-code-antigravity-reference.md`
   - Make a minimal, surgical edit (don't rewrite, just add/update)
   - Add an entry to `CHANGELOG.md` at the top:
     ```
     ## YYYY-MM-DD
     - [type] Brief description (source)
     ```

5. After all updates:
   - Commit changes: `git add -A && git commit -m "docs: daily auto-update $(date +%Y-%m-%d)"`
   - If on a tracked branch, push: `git push`

6. Output a summary:
   - Number of changes
   - Sections affected
   - Link to commit

## RULES

- Do NOT rewrite working sections
- Do NOT remove content unless source explicitly deprecates it
- DO preserve the document's structure (table of contents, anchors)
- DO add "Updated: YYYY-MM-DD" to changed sections
- If unsure about a change, create an entry in `pending-review.md` instead of editing
```

#### Adım 3: Hook ile günlük tetikle — `~/.claude/settings.json`

Aslında Claude Code hook'ları kullanıcı oturumuna bağlı; cron için **dış scheduler** lazım.

#### Adım 4: Cron script — `~/scripts/daily-doc-update.sh`

```bash
#!/bin/bash
set -e

LOG_FILE=~/logs/claude-doc-update-$(date +%Y%m%d).log
mkdir -p ~/logs

cd ~/Documents/claude-docs

# Headless çalıştır
claude -p "/doc-updater run daily update" \
  --output-format json \
  --model sonnet \
  > "$LOG_FILE" 2>&1

# Hata kontrolü
if [ $? -ne 0 ]; then
  echo "Doc update failed, see $LOG_FILE"
  # Notification gönder
  osascript -e 'display notification "Doc update failed" with title "Claude Auto-Doc"'
  exit 1
fi

# Başarılı: özet çıkar
SUMMARY=$(jq -r '.result' "$LOG_FILE")
echo "$SUMMARY"

# macOS notification
osascript -e "display notification \"Updated docs. See $LOG_FILE\" with title \"Claude Auto-Doc\""
```

Cron entry (macOS launchd kullanıyorsan `launchd` plist'i gerekir; Linux için):
```bash
crontab -e
# Her gün 09:00'da çalış
0 9 * * * /Users/me/scripts/daily-doc-update.sh
```

#### Adım 5: macOS launchd (cron yerine modern yol)

`~/Library/LaunchAgents/com.me.claudedocupdate.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.me.claudedocupdate</string>

  <key>ProgramArguments</key>
  <array>
    <string>/Users/me/scripts/daily-doc-update.sh</string>
  </array>

  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>9</integer>
    <key>Minute</key>
    <integer>0</integer>
  </dict>

  <key>StandardOutPath</key>
  <string>/Users/me/logs/launchd-doc-update.log</string>

  <key>StandardErrorPath</key>
  <string>/Users/me/logs/launchd-doc-update.err</string>
</dict>
</plist>
```

Yükle:
```bash
launchctl load ~/Library/LaunchAgents/com.me.claudedocupdate.plist
launchctl start com.me.claudedocupdate
```

### 13.3 İyileştirmeler

**Versiyon 2 fikirleri:**

1. **Web search MCP ekle** (Brave veya Exa) — Claude'un kendi WebSearch'ünden daha geniş tarama
2. **Notification webhook** — Slack/Discord'a değişikliği yaz
3. **PR mode** — direkt commit yerine PR aç (review için)
4. **Embeddings tabanlı dedup** — aynı haberi iki kez ekleme
5. **Source priority skoru** — anthropics/ org'undan gelen değişiklik = ALTIN; rastgele blog = SİLVER
6. **Weekly digest** — günlük noise yerine haftalık özet (Pazar sabahı)

### 13.4 Self-Updating Doc Agent Alternatifi: GitHub Actions

CI/CD kullanıyorsan daha temiz:

`.github/workflows/doc-update.yml`:

```yaml
name: Daily Doc Update
on:
  schedule:
    - cron: '0 9 * * *'  # UTC
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: curl -fsSL https://claude.ai/install.sh | bash

      - name: Run doc updater
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "/doc-updater run daily update" \
            --output-format json \
            --model sonnet

      - name: Create PR if changes
        uses: peter-evans/create-pull-request@v6
        with:
          commit-message: "docs: daily auto-update"
          title: "Auto-update docs $(date +%Y-%m-%d)"
          branch: auto-doc-update-$(date +%Y%m%d)
```

Bu yaklaşım hem cloud'da çalışır, hem GitHub history'sinde kayıt tutar.

---

<a name="14-cheat-sheet"></a>
## 14. 🆘 Acil Durum Cheat Sheet

```
═══════════════════════════════════════════════════════════
TOKEN ACİL DURUM
═══════════════════════════════════════════════════════════
Token bitiyor             /compact (retain X)
Konu değişti              /clear
Yan soru sor              /ask
Alternatif dene           /branch (eski: /fork)
Maliyet gör               /cost
Plan vs gerçek            /usage

═══════════════════════════════════════════════════════════
MODEL HIZLI GEÇİŞ
═══════════════════════════════════════════════════════════
Hafif iş                  /model haiku
Günlük iş                 /model sonnet
Ağır iş                   /model opus
Plan'da Opus, build'de Sonnet  /model opusplan
Effort yüksek (Opus 4.7)  /effort xhigh

═══════════════════════════════════════════════════════════
EKLENTİ YÖNETİM
═══════════════════════════════════════════════════════════
MCP yönet                 /mcp
Subagent yönet            /agents
Skill listele             /skills
Plugin yükle              /plugin install <name>@<market>
Hook düzenle              /hooks

═══════════════════════════════════════════════════════════
DOSYA REFERANSI
═══════════════════════════════════════════════════════════
Dosya ekle                @path/to/file
Klasör ekle               @path/to/dir/
URL fetch                 @https://...
Komut çıktısı             !`git diff HEAD`
Inline shell              !ls -la

═══════════════════════════════════════════════════════════
KLAVYE
═══════════════════════════════════════════════════════════
Mod döngüsü               Shift+Tab
Submit (extension)        Cmd/Ctrl+Enter
İptal                     Esc
Çıkış                     Ctrl+C, Ctrl+C
Editor↔Manager (Antigrav) Cmd/Ctrl+E
Inline edit (Antigrav)    Cmd/Ctrl+I

═══════════════════════════════════════════════════════════
PROMPT FORMÜLÜ
═══════════════════════════════════════════════════════════
   Bağlam     (hangi dosya, hangi fonksiyon)
+  Reproducer (input → expected → actual)
+  Constraint (ne YAPMA)
+  Done       (ne zaman bitti)
+  Output     (diff mi, full file mi?)
═══════════════════════════════════════════════════════════

TOKEN TASARRUFUNUN 7 KURALI:
1. CLAUDE.md kısa tut (<1200 token)
2. /compact eşiği %70 (vars %95 değil)
3. Doğru model (Sonnet default, Opus istisna)
4. Uzun log yapıştırma → dosyadan okut
5. /clear refleksi (konu değişince)
6. Subagent ile keşif (context isolation)
7. "Teşekkür ederim" mesajı atma

ANTIGRAVITY + CLAUDE CODE ROL PAYLAŞIMI:
- Gemini (Antigravity sidebar) → planlama, mimari, hızlı sorular
- Claude Code (terminal/panel)  → implementation, refactor, debug
- MCP servers                   → DB, GitHub, Playwright, vb.
- Sen                           → yargı, mimari karar, onay
```

---

<a name="15-kaynaklar"></a>
## 15. 📚 Kaynaklar (Sırayla İncele)

### Resmi Dokümantasyon
- https://code.claude.com/docs — Claude Code ana docs
- https://code.claude.com/docs/en/commands — Slash commands
- https://code.claude.com/docs/en/skills — Skills
- https://code.claude.com/docs/en/sub-agents — Subagents
- https://code.claude.com/docs/en/hooks — Hooks
- https://platform.claude.com/docs — Claude API & Agent SDK
- https://developers.googleblog.com/build-with-google-antigravity/ — Antigravity intro
- https://codelabs.developers.google.com/getting-started-google-antigravity — Antigravity codelab

### GitHub Repo'ları
- https://github.com/anthropics/claude-code — Resmi Claude Code repo
- https://github.com/anthropics/skills — Resmi skill örnekleri
- https://github.com/hesreallyhim/awesome-claude-code — Ana awesome listesi
- https://github.com/jqueryscript/awesome-claude-code — Tool entegrasyonları
- https://github.com/ccplugins/awesome-claude-code-plugins — Plugin listesi
- https://github.com/ComposioHQ/awesome-claude-skills — 1000+ skill
- https://github.com/wshobson/agents — 81 plugin marketplace
- https://github.com/alirezarezvani/claude-skills — 245+ cross-platform skill
- https://github.com/luongnv89/claude-howto — Görsel rehber

### Marketplace & Directories
- https://buildwithclaude.com/ — Resmi plugin marketplace
- https://awesomeclaude.ai/ — Visual directory
- https://mcp-marketplace.io/ — MCP server marketplace
- https://sub-agents.directory — Subagent kataloğu

### Topluluk
- r/ClaudeAI (Reddit)
- r/ChatGPTCoding (Reddit, çapraz topluluk)
- Anthropic Discord
- Claude Code GitHub issues

### Anthropic Resmi Eğitim
- https://anthropic.skilljar.com — Anthropic Academy
- https://anthropic.skilljar.com/introduction-to-model-context-protocol — MCP kursu

---

## 📝 Son Söz

Bu doküman 13 Mayıs 2026'da hazırlandı. Ekosistem haftada bir güncelleniyor — özellikle:
- Claude Code versiyonları (v2.1.x)
- Yeni slash komutları
- Yeni hook event'leri
- Skill standardı genişlemesi

**Bölüm 13'teki Self-Updating Agent'ı bugün kur**, bir hafta içinde bu dokümanın daima güncel kalmasını sağlar. Manuel araştırmadan kurtulursun — söz verdiğim gibi.

Soruların olursa bu dokümanı Claude'a yapıştır, üzerinde sohbet et. Doküman canlı bir varlıktır, donmuş bir kitap değil.

**İyi yolculuklar.** 🚀
