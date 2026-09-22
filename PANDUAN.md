# Panduan sederhana — apa ini dan bagaimana memakainya

Ditulis untuk siapa saja, tanpa latar belakang teknis.

---

## Apa ini sebenarnya

Aplikasi pencatat pekerjaan mekanik, dipakai lewat HP.

Bedanya dengan aplikasi biasa: **tidak diunduh dari Play Store.** Ia dibuka
lewat tautan di peramban, lalu bisa "dipasang" ke layar utama HP sehingga
terlihat dan terasa seperti aplikasi biasa — ada ikonnya, terbuka layar penuh,
tanpa bilah alamat.

Jenis ini disebut **PWA**. Bagi pemakainya tidak ada bedanya dengan aplikasi
lain. Bagi yang mengelola, bedanya besar: memperbarui cukup di satu tempat, dan
semua HP ikut terbarui sendiri. Tidak ada yang perlu mengunduh apa-apa lagi.

---

## Kenapa tetap jalan tanpa sinyal

Ini bagian terpentingnya, dan yang paling sering disalahpahami.

Di tambang sinyal hilang-timbul. Kalau aplikasi menunggu sinyal setiap kali
mekanik menekan tombol, ia akan berhenti bekerja separuh hari.

Jadi aplikasi ini **tidak pernah menunggu**. Bayangkan mekanik punya buku catatan
di sakunya:

1. Ia menulis di buku itu — **seketika**, sinyal ada atau tidak
2. Begitu sinyal datang, isi buku itu **dikirim sendiri** ke kantor
3. Setelah terkirim, catatannya dicoret dari buku

Buku catatan itu disebut **antrean**. Kalau mekanik melihat tulisan "menunggu
dikirim", artinya pekerjaannya **sudah aman tersimpan di HP** — hanya belum
sampai ke kantor. Ia tidak perlu mengulang.

**Satu hal penting:** pengiriman butuh aplikasinya hidup sebentar. Kalau HP
langsung dikunci dan aplikasi ditutup di area tanpa sinyal, antreannya menunggu
sampai aplikasi dibuka lagi di tempat bersinyal. Datanya tidak hilang — cuma
belum berangkat.

---

## Cara memasang di HP

1. Buka tautan yang diberikan atasan Anda di **Chrome** (Android) atau **Safari**
   (iPhone)
2. Tekan menu tiga titik → **"Tambahkan ke layar utama"**
3. Ikonnya muncul di layar HP seperti aplikasi lain

Setelah itu cukup tekan ikonnya. Tautannya tidak perlu disimpan atau diingat.

---

## Tautan Anda adalah kunci Anda

Tautan yang diberikan kepada Anda mengandung **token** — semacam kunci yang
memberi tahu sistem Anda siapa.

Bentuknya kira-kira begini:

```
https://...../index.html?token=a1b2c3d4e5f6
```

Tiga hal yang perlu dipahami:

- **Jangan dibagikan.** Siapa pun yang memegang tautan Anda bisa mencatat
  pekerjaan atas nama Anda.
- **Cukup dibuka sekali.** Sesudah itu HP mengingatnya sendiri.
- **Tautan tiap orang berbeda.** Memakai tautan orang lain berarti pekerjaan
  Anda tercatat sebagai pekerjaannya.

Kalau tautan Anda hilang, minta atasan membuatkan yang baru. Yang lama bisa
dimatikan.

---

## Sehari-hari

**Mekanik**

1. Buka aplikasi → daftar tugas muncul
2. Tekan tugas yang dikerjakan → tekan **mulai** saat mulai bekerja
3. Selesai bekerja → tekan **selesai** → tekan **Kirim**
4. Tulisan "menunggu dikirim" akan hilang sendiri saat sinyal ada

**Atasan (L1 / L2)**

1. Buka aplikasi → daftar pekerjaan yang menunggu persetujuan
2. Periksa jam kerja dan orang yang terlibat
3. Setujui, atau kembalikan bila ada yang perlu diperbaiki

Pekerjaan melewati dua tahap persetujuan. Setelah tahap kedua, barulah ia
terhitung.

---

## Kalau ada yang aneh

| yang terlihat | artinya | yang dilakukan |
|---|---|---|
| "Menunggu dikirim" lama sekali | belum ada sinyal | buka aplikasi di tempat bersinyal, tunggu sebentar |
| Tanda merah di sebuah kiriman | pengiriman ditolak | baca pesannya; yang berbunyi "sibuk" cukup ditekan "Coba lagi" |
| Aplikasi terlihat versi lama | HP masih memakai simpanan lama | tutup penuh aplikasinya, buka lagi |
| Daftar tugas kosong padahal ada kerjaan | tugasnya belum diberikan ke Anda | tanyakan ke atasan |

**Jangan** menghapus data aplikasi atau membuang pemasangannya saat masih ada
tulisan "menunggu dikirim". Pekerjaan yang belum terkirim ada di situ, dan
menghapusnya berarti menghapus pekerjaan itu.

---

## Untuk yang menyiapkan sistemnya

Bagian ini untuk orang yang memasang, bukan yang memakai.

### Aplikasi ini hanya separuh

Yang ada di sini cuma **tampilannya** — yang dilihat dan disentuh di HP.

Separuh lainnya adalah **spreadsheet Google beserta programnya**, tempat semua
data sungguhan disimpan dan dihitung: siapa mengerjakan apa, berapa jam, berapa
poin. Bagian itu **tidak ada di sini**.

Perumpamaannya: ini kasir dan mesinnya, tapi gudangnya di tempat lain. Tanpa
gudang, kasirnya menyala tapi tidak ada barang.

### Menyambungkan keduanya

Bayangkan spreadsheet Anda punya satu **alamat**. Aplikasi harus tahu alamat itu
untuk bisa mengirim dan mengambil data.

Tiga langkah:

1. **Dapatkan alamatnya.** Di program spreadsheet Anda, terbitkan sebagai
   aplikasi web. Google memberi satu alamat panjang yang diakhiri `/exec`.

2. **Tuliskan alamat itu di dua berkas.** Di `app.js` dan di `sw.js`. **Harus
   sama persis.** Kalau berbeda, sebagian data masuk dan sebagian tidak — dan
   itu jenis kerusakan yang paling sulit ditemukan, karena tidak ada pesan galat
   apa pun.

3. **Naikkan dua nomor versi.** Satu di `app.js`, satu di `sw.js`. Ini yang
   memberi tahu HP bahwa ada versi baru. Kalau lupa, HP tetap memakai yang lama
   dan Anda akan mengira perubahannya tidak jalan.

Rinciannya, termasuk apa yang harus disediakan program spreadsheet-nya, ada di
**[SETUP.md](SETUP.md)**.

### Satu peringatan

**Jangan mengisi alamat milik sistem orang lain.** Aplikasi ini menulis data
yang menentukan gaji. Kalau diarahkan ke sistem yang sedang dipakai orang lain,
tulisannya masuk ke sana — dan **tidak ada satu pun pesan galat** yang memberi
tahu bahwa itu terjadi. Semuanya akan terlihat normal sampai ada yang sadar
gajinya salah.

Karena itu alamat di salinan ini sengaja dikosongkan.
