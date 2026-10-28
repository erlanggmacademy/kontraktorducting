# Laporan Audit & Rencana Optimasi: kontraktorducting.web.id

Tanggal audit: 26 Oktober 2026
Situs: https://kontraktorducting.web.id/
Stack: HTML statis + Bootstrap CSS murni (tanpa framework, tanpa build tool)
Bisnis: Jasa ducting AC dan exhaust kitchen hood, alamat di Cibitung, Bekasi, melayani seluruh Indonesia.

---

## 0. ATURAN KERJA UNTUK AGENT (WAJIB DIBACA)

1. **Review dulu, jangan langsung implementasi.** Baca file yang relevan, rangkum temuan dan rencana, tunggu konfirmasi sebelum menulis kode.
2. **Jangan generate kode yang tidak diminta.** Kerjakan hanya tahap yang secara eksplisit diminta.
3. **Jangan ubah halaman yang sudah ada dan sudah terindeks Google**, kecuali perubahan minimal yang disebut di bagian 6 (hanya mengganti `href`). Halaman yang dilindungi:
   - `index.html`, `about.html`, `produk.html`, `layanan.html`, `blog.html`
   - `perbedaan-ducting-pu-vs-bjls.html`
   - `exhaust-hood-restoran.html`
4. **Saat memperbaiki layout/styling, jangan sentuh style desktop yang sudah ada.** Hanya tambahkan lapisan responsif (media query) di atasnya.
5. **Saat mengubah file yang ada, kirim hanya baris yang berubah**, bukan seluruh file ditulis ulang, kecuali diminta eksplisit.
6. Semua halaman baru memakai **path absolut** (`/css/...`, `/js/...`, `/img/...`) karena berada di subfolder.

---

## 1. Apa yang sudah dicek

Sudah dibaca (versi teks hasil render + meta tag, bukan source HTML mentah):

| Halaman | URL |
|---|---|
| Beranda | https://kontraktorducting.web.id/ |
| Blog (daftar) | https://kontraktorducting.web.id/blog.html |
| Produk (daftar) | https://kontraktorducting.web.id/produk.html |
| Layanan (daftar) | https://kontraktorducting.web.id/layanan.html |

**Belum dicek / belum bisa diverifikasi:**

- Source HTML mentah (apakah JSON-LD schema sudah ada atau belum)
- `robots.txt` dan `sitemap.xml`
- Halaman `about.html`
- Dua artikel yang sudah ada (`perbedaan-ducting-pu-vs-bjls.html`, `exhaust-hood-restoran.html`)
- Isi folder `/img/` dan gambar mana yang benar-benar 404
- Skor performa (Lighthouse / PageSpeed / Core Web Vitals)
- Redirect www vs non-www, http vs https

Agent diminta memverifikasi poin-poin di atas lebih dulu sebelum menyusun rencana final.

---

## 2. Temuan masalah

### 2.1 Konten dan konsistensi

| # | Masalah | Lokasi |
|---|---|---|
| C1 | Satuan harga tidak konsisten: beranda menulis ducting "/meter", halaman produk menulis "/m²" | `index.html`, `produk.html` |
| C2 | Harga Hood BJLS tidak konsisten: beranda "Rp 365.000 – 1.5 Juta", produk "Rp 365.000 / meter", tabel menyebut "Full Set Kitchen Hood (Custom)" | `index.html`, `produk.html` |
| C3 | Salah ketik "Volume Dumper" (seharusnya "Volume Damper") di beranda | `index.html` |
| C4 | Salah ketik "exhasut" di meta keywords beranda | `index.html` |
| C5 | Footer masih "© 2024" padahal artikel bertanggal 2025 | Semua halaman |
| C6 | Judul beranda "...se Jawa Bali dan Indonesia" kurang natural | `index.html` |
| C7 | Klaim "Best HVAC Contractor Indonesia" tanpa bukti pendukung | `index.html` |
| C8 | Tidak terlihat testimoni, portofolio proyek, nama klien, legalitas usaha (NIB/CV/PT), atau profil tim | Semua halaman |

### 2.2 Gambar

| # | Masalah | Detail |
|---|---|---|
| G1 | Gambar diambil dari Unsplash (hotlink) | Foto stok tidak relevan, bisa hilang kapan saja, menurunkan kepercayaan. Dipakai di produk beranda dan hampir semua thumbnail blog |
| G2 | `img/ducting1.webp` dipakai berulang | Hero beranda, Ducting HVAC PU, Exhaust Hood Stainless, Fresh Air & Ventilasi |
| G3 | `img/ducting3.webp` dipakai untuk produk yang berbeda | "Hood BJLS Premium" di beranda dan "Grease Trap" di produk |
| G4 | Beberapa produk tanpa gambar | Volume Damper (beranda dan produk) |
| G5 | Laporan pemilik: banyak gambar hilang | Perlu dicek file mana yang 404 |
| G6 | Belum terlihat atribut `width`/`height`, `loading="lazy"`, atau alt yang spesifik | Perlu dicek di source |

### 2.3 Struktur dan link

| # | Masalah | Detail |
|---|---|---|
| L1 | Menu mengarah ke `/index.html` sedangkan canonical beranda adalah `/` | Berpotensi duplikat URL |
| L2 | 4 dari 6 artikel blog berlink `#` | Judul: "5 Tanda Ducting AC Anda Perlu Segera Dibersihkan", "Mengapa Gedung Tertutup Wajib Punya Sistem Fresh Air?", "Cara Sederhana Menghitung Kapasitas Exhaust Fan Dapur", "Ducting Spiral vs Kotak: Mana yang Lebih Efisien?" |
| L3 | Pagination blog (Prev, 1, 2, Next) berlink `#` | `blog.html` |
| L4 | "Pencarian Populer" di footer semua `href="#"` | Semua halaman |
| L5 | Link Facebook footer hanya `https://www.facebook.com` (generik); di `layanan.html` link sosial `#` | Footer |
| L6 | Teks "Artikel tidak ditemukan" muncul di markup blog (kemungkinan disembunyikan CSS) | `blog.html` |
| L7 | Tidak ada halaman detail untuk produk dan layanan; semua hanya satu halaman daftar | `produk.html`, `layanan.html` |

### 2.4 SEO dan schema

| # | Masalah | Detail |
|---|---|---|
| S1 | `meta keywords` masih dipakai dan berisi keyword stuffing | Tidak dipakai Google, sebaiknya dihapus |
| S2 | Schema markup belum terverifikasi | Perlu dicek JSON-LD di source; kemungkinan belum ada |
| S3 | Sinyal lokal lemah | Alamat Bekasi tetapi klaim seluruh Indonesia; belum terlihat embed peta, Google Business Profile, atau ulasan |
| S4 | Meta tag yang terlihat sudah ada (bagus, dipertahankan) | canonical, og:*, meta description, robots, theme-color, viewport |
| S5 | Open Graph image sama untuk semua halaman (`ducting1.webp`) | Seharusnya spesifik per halaman |
| S6 | Belum terverifikasi: `sitemap.xml`, `robots.txt`, Twitter Card | Perlu dicek |

---

## 3. Rancangan struktur folder baru

```
/
├── index.html            (JANGAN DIUBAH)
├── about.html            (JANGAN DIUBAH)
├── blog.html             (hanya ganti href, lihat bagian 6)
├── produk.html           (hanya ganti href, lihat bagian 6)
├── layanan.html          (hanya ganti href, lihat bagian 6)
├── perbedaan-ducting-pu-vs-bjls.html   (JANGAN DIUBAH)
├── exhaust-hood-restoran.html          (JANGAN DIUBAH)
├── blog/       → detail artikel baru
├── produk/     → detail produk baru
└── layanan/    → detail layanan baru
```

### Halaman detail yang perlu dibuat

**`/layanan/`**
1. Kitchen Hood Exhaust System
2. Ducting AC Central & HVAC
3. Fresh Air & Ventilasi Pabrik
4. Custom Fabrikasi & Maintenance (opsional: Cleaning Ducting dipisah)

**`/produk/`**
1. Exhaust Hood Stainless Steel
2. Exhaust Hood BJLS (Galvanis)
3. Grease Trap
4. Ducting PU (Polyurethane)
5. Ducting BJLS (Seng)
6. Volume Damper

**`/blog/`** (4 artikel yang saat ini `#`)
1. 5 Tanda Ducting AC Anda Perlu Segera Dibersihkan
2. Mengapa Gedung Tertutup Wajib Punya Sistem Fresh Air?
3. Cara Sederhana Menghitung Kapasitas Exhaust Fan Dapur
4. Ducting Spiral vs Kotak: Mana yang Lebih Efisien?

---

## 4. Isi tiap jenis halaman detail

**Layanan:** H1 berisi keyword utama, masalah yang diselesaikan, cakupan kerja, alur kerja (survei, RAB, fabrikasi, instalasi, serah terima), material, area layanan, garansi, 3 sampai 5 FAQ, foto proyek asli, CTA WhatsApp, link ke produk terkait.

**Produk:** tabel spesifikasi, rentang harga dengan satuan yang jelas dan konsisten, kegunaan, perbandingan dengan produk lain, galeri, FAQ, link ke layanan terkait, CTA WhatsApp.

**Blog:** penulis, tanggal terbit dan update, daftar isi, isi 800 sampai 1.500 kata, gambar dengan alt deskriptif, link ke produk dan layanan terkait, CTA di akhir.

Setiap halaman wajib punya: `title` unik (sekitar 60 karakter), `meta description` unik (sekitar 155 karakter), canonical self-referencing, Open Graph dan Twitter Card spesifik, breadcrumb HTML, dan satu `<h1>`.

---

## 5. Rencana schema markup (JSON-LD)

| Halaman | Schema |
|---|---|
| Beranda | `Organization` atau `HVACBusiness` (turunan LocalBusiness), `WebSite` |
| Semua halaman | `BreadcrumbList` |
| Layanan | `Service` (`provider`, `areaServed`, `OfferCatalog` bila relevan) |
| Produk | `Product` dengan `Offer` atau `AggregateOffer` untuk rentang harga |
| Blog | `BlogPosting` (`author`, `datePublished`, `dateModified`, `image`, `publisher`) |
| Halaman ber-FAQ | `FAQPage` |

Catatan:
- Rich result FAQ di Google dibatasi untuk situs pemerintah dan kesehatan sejak 2023. FAQPage tetap berguna untuk AEO dan pemahaman mesin, tetapi jangan dijanjikan tampil sebagai rich result.
- Rentang harga (misal Rp 300.000 – 450.000) memakai `AggregateOffer` dengan `lowPrice` dan `highPrice`.
- NAP (nama, alamat, telepon) harus identik di schema, footer, dan Google Business Profile.
- Validasi dengan Rich Results Test dan Schema Markup Validator sebelum publish.

---

## 6. Perubahan minimal pada halaman yang sudah ada

Hanya jika disetujui pemilik. Tujuannya agar halaman detail baru punya internal link masuk.

| File | Perubahan yang diizinkan |
|---|---|
| `blog.html` | Ganti `href="#"` pada 4 artikel menjadi URL `/blog/...` |
| `produk.html` | Tambah/ubah `href` tombol atau judul produk ke `/produk/...` |
| `layanan.html` | Tambah/ubah `href` ke `/layanan/...` |

Selain `href`, tidak ada isi yang diubah. Perbaikan lain di halaman yang sudah ada (typo, harga, footer, gambar) **tidak dikerjakan** kecuali pemilik meminta secara terpisah.

---

## 7. Teknis dan performa

- Buat `sitemap.xml` yang memuat semua URL lama dan baru, lalu tambahkan ke `robots.txt` dan kirim ke Google Search Console.
- Pastikan satu versi domain (https dan non-www atau www) dengan redirect 301.
- Gambar baru: format WebP, ada `width` dan `height`, `loading="lazy"` kecuali gambar hero, `alt` deskriptif. Jangan memakai Unsplash.
- Tombol WhatsApp melayang untuk mobile.
- Jalankan PageSpeed Insights sebelum dan sesudah, catat LCP, CLS, dan INP.

---

## 8. Kepercayaan dan SEO lokal (di luar scope kode, rekomendasi untuk pemilik)

- Rapikan atau buat Google Business Profile, lalu embed peta dan ulasan.
- Siapkan portofolio proyek (jenis, lokasi, tahun, foto asli) dan testimoni.
- Tampilkan legalitas usaha dan syarat garansi yang jelas.
- Halaman per kota (Bekasi, Cikarang, Jakarta, dan lainnya) hanya dibuat jika isinya benar-benar unik, bukan template dengan nama kota diganti (risiko doorway page).

---

## 9. Urutan pengerjaan yang disarankan

1. **Verifikasi:** cek source HTML, robots.txt, sitemap.xml, isi `/img/`, dan dua artikel yang sudah ada. Laporkan hasilnya, tanpa mengubah file.
2. **Template:** buat template halaman detail (head, navbar, breadcrumb, footer) yang konsisten dengan desain yang ada.
3. **Layanan:** 4 halaman detail beserta schema.
4. **Produk:** 6 halaman detail beserta schema.
5. **Blog:** 4 artikel yang tertunda.
6. **Internal link:** perubahan `href` minimal pada halaman daftar (bagian 6), setelah disetujui.
7. **Sitemap dan robots:** perbarui, lalu kirim ke Search Console.

Setiap tahap dikerjakan satu per satu dan menunggu persetujuan pemilik sebelum lanjut.

---

## 10. Informasi bisnis yang dipakai di halaman baru

- Nama: Kontraktor Ducting
- Telepon/WhatsApp: 0852-1062-0252 (`https://wa.me/6285210620252`)
- Alamat: Jl. Gramapuri Persada Blok H9 No.2, Cibitung, Bekasi
- Jam: Buka 24 jam
- Instagram: https://www.instagram.com/kontraktorducting
- Warna tema: `#0d2e5c`
- Bahasa: Indonesia (`id_ID`)
- Layanan inti: Kitchen Hood Exhaust, Ducting AC Central & HVAC, Fresh Air & Ventilasi, Custom Fabrikasi & Maintenance
- Harga referensi (perlu dikonfirmasi pemilik karena saat ini tidak konsisten): Ducting BJLS Rp 300.000 – 450.000; Ducting PU Rp 400.000 – 500.000; Exhaust Hood BJLS mulai Rp 365.000; Exhaust Hood Stainless mulai Rp 1.500.000/unit; Volume Damper Rp 350.000/unit; Grease Trap: hubungi untuk harga
