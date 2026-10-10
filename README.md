# Cari Apa

Frontend pencarian web privat bergaya shadcn/Claude. Satu file `index.html`
mandiri — tanpa build step, tanpa backend. Deploy sebagai situs statis
(GitHub Pages, dsb).

## Fitur

- Halaman utama ala Google: brand serif, search box besar, tombol **Cari** +
  **Saya Lagi Beruntung** (buka hasil pertama langsung)
- Halaman hasil: tab **Semua / Gambar / Berita / Video**, filter rentang waktu,
  breadcrumb favicon + domain, snippet, badge engine, "Maksud Anda",
  kartu jawaban kalkulator, "Penelusuran terkait", pagination
- Pengaturan: bahasa, SafeSearch, tema (terang/gelap/ikuti sistem)
- Riwayat pencarian tersimpan lokal (localStorage), shortcut `/` untuk fokus,
  skeleton loading, state error yang jelas

## Syarat server pencarian

Aplikasi memanggil `GET {base}/search?q=...&format=json` langsung dari browser,
jadi server pencarian (SearXNG) harus:

1. **Mengaktifkan format JSON** — di `settings.yml`:
   ```yaml
   search:
     formats:
       - html
       - json
   ```
   lalu restart SearXNG. (Tanpa ini API mengembalikan HTTP 403.)

2. **Mengizinkan CORS** — tambahkan header respons untuk path `/search`:
   `Access-Control-Allow-Origin: *`
   Bisa lewat reverse proxy (nginx/caddy) atau Cloudflare Transform Rules
   (Modify Response Header) tanpa menyentuh server.

Jika salah satu belum aktif, aplikasi menampilkan kartu error yang menjelaskan
persis apa yang kurang — bukan halaman blank.

## Mode mock (pengujian lokal tanpa server)

Buka dengan parameter `?mock=<skenario>` untuk menguji tampilan memakai data
palsu, misalnya:

```
index.html?mock=results#/?q=cara%20membuat%20rendang&cat=general&page=1
```

Skenario: `results`, `images`, `calc` (kartu jawaban), `typo` (koreksi),
`empty` (hasil kosong).

## Struktur

- `index.html` — seluruh aplikasi (HTML + CSS + JS dalam satu file)
