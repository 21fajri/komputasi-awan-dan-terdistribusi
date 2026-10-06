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

## Pitfall 2: [nama pitfall] — ditulis oleh [Ahmad Nur Fajri]

(ulangi struktur di atas)

---

## Pitfall 3: [nama pitfall] — ditulis oleh [nama]

(ulangi struktur di atas)

---

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.]
