# MAR KMB V2 — PWA (salinan baca)

Aplikasi lapangan untuk **Mechanic Activity Report** PT KMB: mekanik mencatat
pekerjaan dari HP, atasan menyetujuinya, dan catatan itulah yang menentukan gaji.

Repositori ini **salinan untuk dibaca**, bukan yang berjalan di produksi.

---

## ⚠️ Yang perlu diketahui lebih dulu

**URL deployment sudah dicabut.** `CONFIG.API_URL` di `app.js` dan `API_URL` di
`sw.js` sengaja dikosongkan, jadi salinan ini **tidak tersambung ke sistem yang
dipakai mekanik**. Itu disengaja, bukan kelalaian.

Kalau diisi dengan URL produksi, salinan ini akan menulis ke sistem yang sama
dengan yang sedang dipakai di lapangan — dan tidak akan ada satu pun galat yang
memberi tahu bahwa itu sedang terjadi. Data yang masuk ke sana menentukan gaji
orang.

**Riwayat git repositori ini dimulai dari nol**, bukan disalin dari repositori
aslinya, supaya URL produksi tidak bisa ditemukan di commit lama.

Untuk menjalankannya sendiri: buat deployment Apps Script Anda sendiri, lalu isi
**kedua** tempat itu dengan nilai yang sama.

---

## Isinya cuma empat berkas

| berkas | isi |
|---|---|
| `app.js` | seluruh logika aplikasi — layar, antrean offline, sinkronisasi |
| `sw.js` | service worker: cache, background sync, notifikasi |
| `index.html` | kerangka halaman (hampir semua isi digambar oleh `app.js`) |
| `manifest.json` | nama & ikon saat dipasang di layar utama HP |

---

## Prinsip yang menjelaskan hampir semua keputusan di dalamnya

> **CACHE → ANTRE → SINKRON. Server selalu benar.**

Sinyal di tambang sering hilang. Jadi aplikasi ini tidak pernah menunggu
jaringan: setiap tindakan masuk **outbox** di IndexedDB lebih dulu, lalu dikirim
belakangan saat sinyal ada — bahkan setelah aplikasi ditutup, lewat background
sync.

Beberapa hal yang mungkin terlihat aneh sebenarnya disengaja:

- **Tiap operasi punya `op_id`.** Server menyimpannya dan menolak yang kembar,
  jadi pengiriman ulang tidak pernah menghasilkan data dobel.
- **Nomor versi ada di dua tempat** — `APP_VERSION` di `app.js` dan `CACHE` di
  `sw.js` — dan keduanya wajib naik bersamaan. Pernah tertinggal: `APP_VERSION`
  masih v26 sementara `CACHE` sudah v34, dan peramban memuat kode baru di atas
  cache lama selama berhari-hari sebelum ketahuan.
- **Kegagalan tidak selalu berarti gagal.** HTTP 502 bisa berarti server sudah
  menulis tapi jawabannya yang putus di jalan. Karena itu banyak jalur menandai
  operasi "tetap antre" alih-alih "gagal".

---

## Dua panduan, pilih sesuai kebutuhan

**[PANDUAN.md](PANDUAN.md)** — bahasa sehari-hari, tanpa istilah teknis. Apa ini,
kenapa tetap jalan tanpa sinyal, cara memasang di HP, cara memakainya, dan
gambaran sederhana cara menyambungkannya. Mulai dari sini.

**[SETUP.md](SETUP.md)** — rincian teknis: kontrak API yang harus disediakan
backend, daftar aksi, setelan deploy, dan syarat yang paling mudah dilewatkan.

Perlu diketahui: **backend-nya tidak ada di repositori ini.** Yang ada hanya
antarmuka. Seluruh perhitungan poin, alur approval, dan payroll hidup di Apps
Script yang terpisah.
