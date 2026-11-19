# Template Catatan Pengujian Otorisasi API

**Status dokumen: template kosong.** Semua bagian bertanda kurung siku harus diisi berdasarkan pengamatan. Template ini tidak menyatakan adanya kerentanan, pengujian yang sudah dilakukan, atau perbaikan yang berhasil.

Untuk salinan publik, gunakan data sintetis. Simpan bukti sistem nyata secara privat sesuai izin pengujian.

## 1. Identitas pemeriksaan

| Elemen | Catatan |
| --- | --- |
| ID catatan | [Isi ID internal lab] |
| Jenis lingkungan | [Lab lokal atau lingkungan yang diizinkan] |
| Pemberi izin dan batas pengujian | [Catat secara privat jika sensitif] |
| Penguji | [Nama atau identitas penguji] |
| Waktu dan zona waktu | [Tanggal, jam, zona waktu] |
| Versi aplikasi | [Build, commit, atau versi yang diperiksa] |
| Alat dan versi | [Alat yang benar-benar digunakan] |
| Endpoint dan metode | [Endpoint dalam cakupan] |
| ID kasus pada matriks | [Misalnya AUTH-03] |

## 2. Kebijakan dan prasyarat

- Aturan akses yang disepakati: [Siapa boleh melakukan tindakan apa terhadap objek mana?]
- Identitas dan peran akun: [Akun sintetis; jangan tulis token]
- Keanggotaan tenant: [Hubungan yang sudah diperiksa]
- Objek dan pemiliknya: [ID sintetis serta sumber konfirmasi kepemilikan]
- Kondisi sesi: [Aktif, kedaluwarsa, atau tanpa sesi]
- Konteks cache: [Jalur yang digunakan atau keterbatasan pemeriksaan]
- Di luar cakupan: [Endpoint, tindakan, atau tenant yang tidak diperiksa]

## 3. Ekspektasi sebelum pengujian

| Aspek | Ekspektasi sesuai kontrak lab |
| --- | --- |
| Keputusan akses | [Diizinkan atau ditolak beserta alasannya] |
| Respons HTTP | [Kode yang sudah ditetapkan] |
| Data yang boleh terlihat | [Bidang atau cakupan data] |
| Data yang tidak boleh terlihat | [Data yang dilindungi] |

## 4. Langkah reproduksi dan bukti

1. [Siapkan fixture dan sesi sesuai cakupan.]
2. [Jalankan kontrol pembanding yang seharusnya diizinkan.]
3. [Jalankan kasus penolakan dengan identitas yang dipastikan benar.]
4. [Catat respons dan bukti kepemilikan objek.]

| Bukti | Referensi tersanitasi |
| --- | --- |
| Identitas dan keanggotaan akun | [Nama berkas atau referensi bukti privat] |
| Request kontrol pembanding | [Metode, path, konteks akun; rahasia dihapus] |
| Response kontrol pembanding | [Kode dan bidang sintetis yang relevan] |
| Request kasus yang diperiksa | [Metode, path, konteks akun; rahasia dihapus] |
| Response kasus yang diperiksa | [Kode, isi, header relevan yang tersanitasi] |
| Log aplikasi bila tersedia | [Referensi, waktu, dan batas akses] |

Respons HTTP saja belum cukup untuk menghubungkan pengguna, objek, dan pelanggaran kebijakan. Hapus token, cookie, pengenal pribadi, serta rahasia lain dari cuplikan publik.

## 5. Pengamatan dan penafsiran

- Hasil aktual: [Tulis apa yang benar-benar terlihat.]
- Perbandingan dengan ekspektasi: [Sesuai atau penyimpangan yang teramati.]
- Pemeriksaan pembanding: [Apa yang diperiksa untuk menyingkirkan sesi tertukar, keanggotaan tambahan, atau data fixture yang salah?]
- Status: [Belum diuji / Sesuai ekspektasi / Tidak sesuai ekspektasi / Terhalang / Tidak berlaku]
- Dampak yang terbukti: [Batasi pada data atau tindakan yang benar-benar diamati.]
- Hal yang belum dibuktikan: [Contohnya akses massal atau perubahan pesanan.]
- Penyebab: [Terbukti dari bukti tertentu / belum dikonfirmasi; sertakan referensi.]

## 6. Prioritas dan perbaikan

- Prioritas dan alasan: [Paparan, prasyarat, jenis data, dan konteks bisnis lab.]
- CVSS bila digunakan: [Versi, vector, skor, serta alasan; biarkan kosong jika tidak dihitung.]
- Usulan perubahan: [Kontrol akses server yang sesuai kebijakan.]
- Pemilik tindak lanjut: [Jika diketahui.]
- Batas perbaikan: [Jalur lain yang belum ditinjau.]

## 7. Retest

Salin kasus relevan ke matriks retest terpisah. Pertahankan hasil pemeriksaan awal.

| Elemen | Catatan retest |
| --- | --- |
| Versi setelah perubahan | [Versi atau commit] |
| Waktu dan penguji | [Waktu, zona waktu, penguji] |
| Kasus awal yang diulang | [ID dan pengamatan aktual] |
| Kontrol akses yang diizinkan | [ID dan pengamatan aktual] |
| Pemeriksaan arah sebaliknya | [ID dan pengamatan aktual] |
| Bukti retest | [Referensi tersanitasi] |
| Kesimpulan dalam cakupan | [Diperbaiki pada kasus yang diperiksa / masih menyimpang / terhalang] |
| Risiko dan batas tersisa | [Apa yang belum diperiksa?] |

Tandai diperbaiki setelah bukti retest mendukungnya. Kesimpulan berlaku pada versi, konfigurasi, dan kasus yang dicatat.

Kembali ke [panduan dan checklist](README.md) atau [matriks pengujian](matriks-otorisasi.csv).
