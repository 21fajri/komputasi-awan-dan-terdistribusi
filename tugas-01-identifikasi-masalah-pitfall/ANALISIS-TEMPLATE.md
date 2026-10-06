# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** [nama kelompok]

| Nama                                    | NIM            | Kontribusi                       |
| --------------------------------------- | -------------- | -------------------------------- |
| [Yan Chrisdaniel Partogi rayano Ludjen] | [103072400010] | [pitfall/bagian yang dikerjakan] |
| [Bima Luthfi Nurhakim]                  | [103072400030] | [pitfall 1]                      |
| [Jeremy Joving Winargo]                 | [103072400085] | [pitfall/bagian yang dikerjakan] |
| [Ahmad Nur Fajri]                       | [103072430007] | [pitfall/bagian yang dikerjakan] |

## Pitfall 1: [The network is reliable] — ditulis oleh [Bima Luthfi Nurhakim]

**Bukti di skenario:** [Tim menemukan bahwa kode mereka menulis asumsi seperti `# network is always reliable, no need for retry` dan tidak ada *timeout* sama sekali pada pemanggilan antar service (modul pesanan memanggil modul pembayaran dan menunggu tanpa batas waktu).]

**Kenapa ini keliru:** [Karena aplikasi FoodGo mengalami gangguan jaringan, packet loss, koneksi terputus, dan respon yg gagal. Bisa dilihat dari aplikasi yg menjadi sangat lambat dan beberapa permintaan timeout dan keberhasilan pengiriman request tidak selalu dapat terjamin.]

**Dampak ke FoodGo:** [Ketika trafik meningkat sebagian komunikasi antar service bisa gagal/ mengalami gangguan karena FoodGo tidak memiliki mekanisme retry.]

**Solusi desain awal:** [Menmabhakan *retry* untuk mencoba kembali request yang sebelumnya gagal]

**Trade-off:** [*retry* dapat menambah beban jaringan dan service tujuan, jika service sedang overload dan banyak request melakukan retry secara bersamaan. Kondisi ini dapat memperparah `cascading faillure`, karena itu] retry harus dibatasi dan menggunakan backoff

---

## Pitfall 2: Latency is zero — ditulis oleh Ahmad Nur Fajri

**Bukti di skenario:** Modul pesanan meminta modul pembayaran memproses transaksi, lalu menunggu jawabannya tanpa batas waktu. Saat makan siang atau promo, aplikasi melambat dan beberapa permintaan mengalami timeout. Berarti disini sudah nampak bahwa pembayaran tidak selalu bisa merespons dengan cepat.

**Kenapa ini keliru:** Modul pesanan dan pembayaran tidak bekerja sebagai satu langkah yang langsung selesai. Pembayaran perlu memproses transaksi dan mengirimkan hasilnya kembali. Saat banyak orang memesan sekaligus, proses ini bisa memakan waktu lebih lama. Jadi, sistem harus siap menghadapi jawaban yang terlambat.

**Dampak ke FoodGo:** Selama menunggu pembayaran, permintaan pesanan terus memakai sumber daya server, seperti thread dan koneksi. Jika banyak permintaan menunggu bersamaan, sumber daya untuk melayani pesanan lain ikut berkurang. Akibatnya, pesanan makin lambat, permintaan menumpuk, sebagian mengalami timeout, dan server bisa kehabisan sumber daya hingga crash.

**Solusi desain awal:** Beri batas waktu atau timeout pada permintaan dari modul pesanan ke pembayaran. Jika batas waktu habis, hentikan penantian dan memberitau pengguna bahwa pembayaran belum terkonfirmasi, bukan langsung menyatakan seperti pembayaran gagal atau berhasil. Batas waktunya perlu ditentukan berdasarkan target waktu respons FoodGo. Jika mencoba kembali permintaan, batasi jumlah percobaan dan beri jeda yang makin panjang. Disini tetap memastikan percobaan ulang tidak membuat pelanggan tertagih dua kali, misalnya dengan memakai ID transaksi yang sama. Jika pembayaran terus lambat, circuit breaker dapat menghentikan sementara permintaan baru ke pembayaran. jadi circuit breaker ini seperti menjeda sementara permintaan agar request tidak menumpuk.

**Trade-off:** Timeout membuat modul pesanan tidak menunggu terlalu lama, tetapi pembayaran mungkin sebenarnya berhasil meskipun jawabannya terlambat. Karena itu, FoodGo perlu menyediakan cara untuk memeriksa status pembayaran sebelum meminta pelanggan mencoba lagi. Selain itu, terlalu banyak percobaan ulang bisa menambah beban pada pembayaran yang sedang lambat.

---

## Pitfall 3: [nama pitfall] — ditulis oleh [nama]

(ulangi struktur di atas)

---

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.]
