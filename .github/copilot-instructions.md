# Panduan Project: presentasi-keuangan

Project ini berisi slide presentasi berbasis **MARP** (Markdown Presentation Ecosystem) yang di-deploy ke **GitHub Pages** secara otomatis via **GitHub Actions**. Struktur folder di-mirror langsung ke URL GH Pages yang berjenjang.

## Struktur Folder

```
presentasi-keuangan/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GH Actions: build MARP → HTML + generate index
├── themes/
│   └── keuangan.css            # Custom MARP theme (satu-satunya theme yang digunakan)
└── keuangan/
    ├── index.html              # Auto-generated oleh GH Actions (jangan diedit manual)
    ├── presentasi-xxx.md       # File sumber presentasi
    └── presentasi-yyy.md
```

**Aturan struktur:**
- Seluruh file sumber presentasi berada di dalam `keuangan/` (atau subfolder-nya).
- File `index.html` di setiap folder di-generate otomatis oleh GH Actions; **jangan buat atau edit manual**.
- Nama file presentasi menggunakan kebab-case, deskriptif (contoh: `laporan-keuangan-q1-2026.md`).

## Konvensi File MARP

Setiap file presentasi `.md` **wajib** memiliki frontmatter berikut di baris pertama:

```yaml
---
marp: true
theme: keuangan
paginate: true
lang: id
header: "Yayasan Penyelenggaraan Ilahi Indonesia"
footer: "© YPII — Dokumen Internal"
---
```

**Aturan konten:**
- Bahasa: **Indonesia** (Bahasa Indonesia formal), kecuali istilah teknis/akuntansi.
- Satu slide = satu `---` pemisah. Jangan menaruh terlalu banyak konten per slide.
- Gunakan heading `#` untuk judul slide utama, `##` untuk subjudul.
- Gambar, ikon, atau tabel diperbolehkan; gunakan path relatif dari root repo.

## Custom Theme

File theme berada di `themes/keuangan.css`. Jika perlu menyesuaikan gaya:
- Edit file tersebut; jangan buat file CSS baru.
- Theme ini di-referensikan via direktif MARP `theme: keuangan`.

## GH Actions & Deploy

Pipeline GH Actions (`deploy.yml`) melakukan:
1. Build semua `.md` → `.html` menggunakan `@marp-team/marp-cli`.
2. Generate `index.html` di setiap folder yang berisi daftar link ke file HTML di dalamnya.
3. Push hasil build ke branch `gh-pages`.

**Konvensi CI/CD:**
- Trigger: push ke branch `main`.
- Output build ada di folder `dist/` (tidak di-commit, hanya digunakan oleh pipeline).
- Jangan tambahkan file `.html` ke-commit di branch `main`; HTML adalah artefak build.

## URL GH Pages

Setelah deploy, URL GH Pages mengikuti struktur folder sumber:

| File sumber | URL GH Pages |
|---|---|
| `keuangan/presentasi-xxx.md` | `https://<org>.github.io/presentasi-keuangan/keuangan/presentasi-xxx.html` |
| `keuangan/` (folder) | `https://<org>.github.io/presentasi-keuangan/keuangan/` (index) |

## Yang Tidak Boleh Dilakukan

- Jangan commit file `.html` ke branch `main`.
- Jangan edit `index.html` secara manual di mana pun.
- Jangan buat theme CSS baru selain `themes/keuangan.css`.
- Jangan gunakan theme MARP bawaan (`default`, `gaia`, `uncover`) — selalu gunakan `theme: keuangan`.
