# Checklist Otorisasi API: BOLA dan IDOR Antar-Toko

Panduan berbahasa Indonesia untuk menyusun pemeriksaan akses pesanan pada aplikasi multi-tenant. Materi ini memuat checklist, matriks pengujian yang bisa diisi, dan template catatan temuan serta retest.

**Seluruh akun, toko, ID, endpoint, dan versi aplikasi merupakan rancangan lab fiktif.** Repositori ini berisi dokumentasi; aplikasi lab dan layanan API tidak disertakan. Tidak ada pengujian langsung atau hasil pentest nyata yang diklaim di sini. Semua baris matriks dimulai dengan status **Belum diuji**.

## Mulai Dari Sini

| Berkas | Kegunaan |
| --- | --- |
| [matriks-otorisasi.csv](matriks-otorisasi.csv) | Delapan kasus uji dengan ekspektasi, kolom pengamatan kosong, dan status awal Belum diuji. |
| [TEMPLATE-LAPORAN-LAB.md](TEMPLATE-LAPORAN-LAB.md) | Pencatatan ruang lingkup, bukti, batas dampak, perbaikan, dan retest. |

1. Siapkan lingkungan lab milik sendiri atau yang diizinkan untuk pengujian, lalu tetapkan kebijakan aksesnya.
2. Sesuaikan endpoint, akun, dan objek sintetis di matriks dengan lab tersebut. URL produksi tidak diperlukan.
3. Jalankan kontrol akses yang diizinkan terlebih dahulu, lalu kasus penolakan yang berpasangan.
4. Catat respons aktual, data yang terlihat, versi aplikasi, waktu, dan referensi bukti. Bandingkan dengan ekspektasi.
5. Dokumentasikan penyimpangan pada template laporan. Setelah perbaikan, ulangi kasus awal dan kontrol pembandingnya.

Matriks dapat dibaca di GitHub atau dibuka sebagai CSV UTF-8 dengan pemisah koma. Simpan salinan pengujian beserta bukti sensitif secara privat. Untuk kontribusi publik, gunakan contoh sintetis yang sudah diperiksa kembali.

## Apa Yang Diperiksa ?

Autentikasi mengenali pengguna; otorisasi menentukan akses pengguna terhadap sumber daya. Pada contoh ini, pengguna yang sudah masuk tetap harus memenuhi aturan akses pesanan. Lihat [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).

**BOLA** (*Broken Object Level Authorization*) berkaitan dengan pemeriksaan izin terhadap objek. **IDOR** (*Insecure Direct Object Reference*) digunakan untuk membahas akses tidak sah melalui referensi objek. Kasus membaca pesanan toko lain dengan mengganti ID dapat berkaitan dengan keduanya. UUID tetap memerlukan pemeriksaan izin. Referensi: [OWASP API1:2023 — Broken Object Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/).

## Kebijakan Lab

Nama versi rancangan: `lab-1.0`. Endpoint: `GET /api/orders/{id}`. Toko A dan toko B adalah tenant terpisah.

| Identitas | Keanggotaan | Hak dalam rancangan ini |
| --- | --- | --- |
| Staf A | Toko A | Membaca seluruh pesanan toko A. |
| Staf B | Toko B | Membaca seluruh pesanan toko B. |
| Tanpa sesi atau sesi kedaluwarsa | Tidak ada identitas terautentikasi yang valid | Tidak membaca detail pesanan. |

Objek sintetis:

- `ORD-A-1001`: pesanan toko A.
- `ORD-B-2001`: pesanan toko B.
- `ORD-X-0000`: ID yang dipastikan tidak tersedia pada fixture lab.

Tidak ada peran administrator lintas tenant, keanggotaan ganda, atau fitur berbagi pesanan dalam rancangan ini. Jika aplikasi memiliki aturan tersebut, buat matriks tambahan berdasarkan kebijakan yang disepakati.

### Ekspektasi Respons

- Akses pesanan toko sendiri: `200`, dengan bidang pesanan yang diizinkan untuk staf.
- Akses pesanan toko lain: `403`, tanpa data pesanan yang dilindungi.
- Tanpa sesi atau dengan sesi kedaluwarsa: `401`, tanpa data pesanan.
- Akun sah meminta ID yang tidak ada: `404`, tanpa detail internal.

Angka tersebut adalah **kontrak lab yang diusulkan**, bukan hasil pengujian. HTTP juga mengizinkan `404` untuk menyembunyikan keberadaan sumber daya yang ditolak. Jika lab memakai pilihan itu, ubah ekspektasi sebelum pengujian dan periksa konsistensinya. Respons `401` harus menyertakan challenge `WWW-Authenticate` yang sesuai skema autentikasi. Rujukan: [RFC 9110 §15.5.2](https://www.rfc-editor.org/rfc/rfc9110.html#section-15.5.2) dan [§15.5.4](https://www.rfc-editor.org/rfc/rfc9110.html#section-15.5.4).

## Checklist Persiapan

- [ ] Lingkungan dan pemberi izin pengujian tercatat.
- [ ] Endpoint, metode, versi aplikasi, waktu, dan batas dampak ditetapkan.
- [ ] Akun staf A dan B hanya memiliki keanggotaan yang sesuai rancangan.
- [ ] Dua sesi terpisah digunakan agar identitas tidak tertukar.
- [ ] Kepemilikan objek sintetis dikonfirmasi dari fixture atau sumber tepercaya lab.
- [ ] Ekspektasi akses dan bidang respons ditulis sebelum pengujian.
- [ ] Pengambilan data nyata, enumerasi massal, dan perubahan pesanan berada di luar cakupan ini.

## Checklist Saat Pemeriksaan

- [ ] Akses yang diizinkan berfungsi untuk kedua staf.
- [ ] Penolakan akses lintas toko diperiksa pada kedua arah.
- [ ] Penolakan diperiksa pada isi respons, bukan hanya kode HTTP.
- [ ] Pengujian tanpa sesi dan sesi kedaluwarsa menggunakan konteks yang terpisah dari sesi aktif.
- [ ] ID yang tidak ada telah dipastikan tidak tersedia di fixture.
- [ ] Kasus cache menjalankan permintaan staf B yang diizinkan, lalu permintaan staf A ke objek yang sama, melalui jalur cache lab yang memang digunakan aplikasi.
- [ ] Bukti menunjukkan akun, objek, aturan akses, serta pengamatan yang saling berkaitan.
- [ ] Tidak ada token, cookie, data pribadi, atau kredensial dalam berkas yang akan dipublikasikan.

Jika cache tidak ada atau tidak dapat diamati pada lab, tulis keterbatasannya. Respons yang sesuai pada satu percobaan tidak membuktikan seluruh konfigurasi cache aman.

## Cara Menilai Pengamatan

Gunakan kolom status sebagai berikut:

| Status | Arti |
| --- | --- |
| Belum diuji | Kasus belum dijalankan. |
| Sesuai ekspektasi | Pengamatan dan bukti mendukung ekspektasi pada kasus serta versi yang dicatat. |
| Tidak sesuai ekspektasi | Ada penyimpangan yang perlu ditelaah dan didokumentasikan. |
| Terhalang | Prasyarat atau akses pengujian tidak tersedia. |
| Tidak berlaku | Kasus tidak relevan terhadap desain lab; alasan wajib dicatat. |

Respons `200` dengan data lintas toko perlu diperiksa bersama keanggotaan akun, kepemilikan objek, dan aturan akses. Peran tambahan atau sesi yang tertukar dapat mengubah penafsiran. Jangan menyatakan seluruh data bisa diambil atau diubah dari bukti pembacaan satu objek.

Untuk pemeriksaan implementasi, pastikan keputusan akses dibuat di server berdasarkan identitas terverifikasi, hubungan pengguna dengan tenant, objek, dan tindakan. Pemeriksaan dilakukan pada setiap jalur akses objek yang relevan. Setelah perubahan, ulangi kasus penolakan serta akses yang seharusnya diizinkan; simpan hasil awal dan retest secara terpisah.

## Batas Cakupan

Delapan kasus ini berfokus pada pembacaan detail pesanan. Daftar, pencarian, ekspor, lampiran, perubahan data, GraphQL, pekerjaan asinkron, pencabutan keanggotaan, serta peran lain memerlukan rancangan tambahan. Penggunaan checklist ini tidak membuktikan cakupan penuh OWASP atau keamanan seluruh aplikasi.

## Panduan Pendamping

- [BOLA dan IDOR pada API: Menguji Akses Pesanan Antar-Toko di DEV](https://dev.to/nusantarasecure/bola-dan-idor-pada-api-menguji-akses-pesanan-antar-toko-659) — pembahasan skenario dan penafsiran bukti.
- [Contoh laporan pentest API dan retest — Nusantara Secure](https://nusantarasecure.com/contoh-laporan-pentest/) — contoh laporan publik berbasis lab fiktif.

Materi edukasi disiapkan oleh [Nusantara Secure](https://github.com/nusantarasecure) . Referensi mendukung penjelasan konsep; materi ini bukan dokumen resmi atau pengesahan OWASP. Jangan memasukkan temuan klien atau laporan sistem nyata ke issue maupun pull request publik.
