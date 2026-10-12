# Latihan MySQL

Website latihan SQL untuk mahasiswa mata kuliah Basis Data. Seluruhnya berjalan di browser (SQL.js), jadi tidak butuh server, database, atau akun. Cukup satu file: `index.html`.

## Menayangkan secara gratis

Semua opsi di bawah gratis dan cukup untuk satu kelas.

**GitHub Pages**
1. Buat repository publik di GitHub, unggah `index.html`.
2. Buka Settings, lalu Pages. Pilih branch `main` dan folder `/ (root)`, klik Save.
3. Alamat situs muncul dalam beberapa menit, bentuknya `https://nama-akun.github.io/nama-repo/`.

**Netlify**
1. Buka app.netlify.com/drop.
2. Seret folder yang berisi `index.html` ke halaman itu. Alamat situs langsung dibuat.

**Cloudflare Pages**
1. Buat proyek baru lewat Direct Upload.
2. Unggah folder berisi `index.html`.

## Memakai tanpa internet (laboratorium dengan koneksi terbatas)

Halaman memuat SQL.js dan font dari internet. Untuk jaringan yang lambat atau tertutup:
1. Unduh `sql-wasm.js` dan `sql-wasm.wasm` dari paket sql.js versi 1.8.0.
2. Letakkan keduanya di folder yang sama dengan `index.html`.
3. Di `index.html`, ubah alamat pada tag `<script src="...sql-wasm.js">` dan pada `locateFile` menjadi `sql-wasm.js` dan `sql-wasm.wasm` (jalur relatif).
4. Buka lewat server lokal atau hosting, bukan dengan klik ganda berkas, karena browser memblokir pemuatan `.wasm` dari `file://`.

## Menambah atau mengubah soal

Buka `index.html` dan cari bagian `const SOAL = [`. Setiap soal berbentuk:

```js
{id:20, level:'join', judul:'Judul soal',
 teks:'Pertanyaan untuk mahasiswa. Boleh memakai <code>nama_kolom</code>.',
 hint:'Petunjuk singkat.',
 ordered:true,            // opsional: urutan baris ikut dinilai
 kunci:'SELECT ... '}     // query jawaban yang benar
```

- `level` salah satu dari: `dasar`, `menengah`, `join`, `subquery`, `dml`.
- Soal yang mengubah data (INSERT, UPDATE, DELETE) memakai `tipe:'dml'` dan `cek:'SELECT ...'`. Jawaban mahasiswa dan kunci sama-sama dijalankan pada data awal, lalu hasil query `cek` dibandingkan.
- Penilaian membandingkan isi hasil, bukan nama kolom atau cara menulisnya. Mahasiswa boleh memakai alias, huruf besar atau kecil, dan JOIN atau subquery selama hasilnya sama.

Kolom kunci mahasiswa pada data latihan juga bernama `npm`. Data tabel ada di bagian `MAHASISWA`, `DOSEN`, `MATAKULIAH`, dan `NILAI` pada file yang sama. Kalau data diubah, jalankan kembali semua kunci jawaban untuk memastikan hasilnya masuk akal.

## Login dengan NPM

Mahasiswa masuk hanya dengan NPM (Nomor Pokok Mahasiswa) sebelum mulai berlatih. Progres soal disimpan per NPM di browser yang dipakai, jadi dua mahasiswa yang bergantian di satu komputer tidak saling menimpa progres. Tombol Keluar mengembalikan ke halaman masuk, dan tombol Laporan menghasilkan teks ringkas (NPM, soal selesai, jumlah percobaan, dan nama bila NPM ada di daftar peserta) yang bisa disalin dan dikirim ke dosen.

Secara bawaan NPM mana pun yang berisi 6 sampai 15 angka boleh masuk. Untuk membatasi hanya peserta kelas Anda, isi daftar `PESERTA` di `index.html`:

```js
const PESERTA = [
  ['2021001','Ahmad Fauzi'],
  ['2021002','Aisyah Putri']
];
```

Bila daftar terisi, NPM di luar daftar ditolak, dan nama peserta ditampilkan di pojok atas serta di Laporan. Bila daftar kosong, hanya NPM yang dipakai.

## Batasan

- Login ini hanya mengenali dan memisahkan progres. NPM bukan rahasia dan tidak ada kata sandi, jadi orang yang tahu NPM teman bisa masuk dengan NPM itu, dan teks Laporan bisa diketik ulang oleh siapa saja. Jangan dipakai sebagai dasar nilai resmi.
- Daftar `PESERTA` terlihat oleh siapa pun yang membuka kode halaman.

- Mesin yang dipakai SQLite, bukan MySQL. Perbedaannya dijelaskan di tab Catatan pada halaman.
- Progres mahasiswa tersimpan di browser masing-masing. Dosen tidak bisa melihat nilai mereka dari satu tempat. Kalau rekap nilai diperlukan, tambahkan penyimpanan di server atau formulir pengumpul hasil.
