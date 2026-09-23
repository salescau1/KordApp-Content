# kordapp-content

Konten remote untuk aplikasi **KordApp** (by kekalclip). Satu repo ini menyimpan:

1. **Popup promosi band** — `promo.json` + gambar di `images/`
2. **Konten layar About** — `about.txt`

App menarik file-file ini dari GitHub. Ubah isinya lalu `git commit` + `git push`;
aplikasi memuat versi baru saat berikutnya online (tanpa update APK).

> **Repo ini WAJIB publik.** Raw dari repo privat butuh token dan akan gagal di app.

---

## Struktur

```
kordapp-content/
├── promo.json        # daftar promosi (manifest)
├── about.txt         # teks layar About (plain text)
├── images/           # gambar band untuk promosi
│   └── contoh.jpg
└── README.md
```

---

## 1. Popup promosi — `promo.json`

Popup muncul di Home **tiap buka app**. App mengambil promo pertama yang
`"active": true`. Kalau file gagal dimuat / offline / tidak ada yang aktif →
**popup tidak muncul** (tidak pernah menampilkan kotak kosong).

### Skema tiap promo

| Field | Wajib | Keterangan |
|---|---|---|
| `id` | ya | Identitas unik promo (bebas, mis. `2026-09-noah`). |
| `active` | ya | `true` = tampil. Set `false` untuk menyimpan tanpa menampilkan. |
| `image` | ya | Path gambar relatif dari repo, mis. `images/noah.jpg`. |
| `title` | ya | Judul di popup. |
| `subtitle` | tidak | Baris teks kecil di bawah judul. |
| `action.type` | ya | `external` (buka link luar) atau `catalog` (buka di app). |
| `action.url` | untuk `external` | URL http/https (Spotify/YouTube/dll). |
| `action.artistSlug` + `action.titleSlug` | untuk `catalog` | Membuka lagu/artis di katalog app. |

### Contoh isi

```json
{
  "version": 1,
  "promos": [
    {
      "id": "2026-09-noah",
      "active": true,
      "image": "images/noah.jpg",
      "title": "NOAH - Single Terbaru",
      "subtitle": "Dengar sekarang",
      "action": { "type": "external", "url": "https://open.spotify.com/artist/xxxx" }
    }
  ]
}
```

### Cara mengganti promosi
1. Taruh gambar band ke `images/` (mis. `images/noah.jpg`).
2. Edit `promo.json`: tambah/ubah entri, set `"active": true` untuk yang mau tampil.
3. `git add . && git commit -m "promo: noah" && git push`.

---

## 2. Layar About — `about.txt`

Plain text biasa. Edit, commit, push. App menariknya saat refresh (cache-first).

**Aturan konten About (tetap dipatuhi):** repo publik, sebut **handle** saja
(`g0dzella` / `kekalclip`) — JANGAN nama asli.

---

## Catatan teknis
- Gambar disajikan lewat CDN **jsDelivr** (`cdn.jsdelivr.net/gh/USER/REPO@main/...`),
  bukan `raw.githubusercontent.com`, supaya cepat & tidak kena rate-limit.
- `promo.json` & `about.txt` ditarik dari GitHub; app selalu punya perilaku aman
  saat gagal (popup tidak muncul, About pakai cache/bawaan).
