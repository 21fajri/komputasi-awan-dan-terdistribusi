# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** [nama kelompok]

| Nama | NIM | Kontribusi |
|---|---|---|
| [Yan Chrisdaniel Partogi rayano Ludjen] | [103072400010] | [pitfall/bagian yang dikerjakan] |
| [Bima Luthfi Nurhakim] | [103072400030] | [pitfall 1] |
| [Jeremy Joving Winargo] | [103072400085] | [pitfall/bagian yang dikerjakan] |
| [Ahmad Nur Fajri] | [103072430007] | [pitfall/bagian yang dikerjakan] |

## Pitfall 1: [The network is reliable] — ditulis oleh [Bima Luthfi Nurhakim]

**Bukti di skenario:** [Tim menemukan bahwa kode mereka menulis asumsi seperti `# network is always reliable, no need for retry` dan tidak ada *timeout* sama sekali pada pemanggilan antar service (modul pesanan memanggil modul pembayaran dan menunggu tanpa batas waktu).]

**Kenapa ini keliru:** [Karena aplikasi FoodGo mengalami gangguan jaringan, packet loss, koneksi terputus, dan respon yg gagal. Bisa dilihat dari aplikasi yg menjadi sangat lambat dan beberapa permintaan timeout dan keberhasilan pengiriman request tidak selalu dapat terjamin.]

**Dampak ke FoodGo:** [Ketika trafik meningkat sebagian komunikasi antar service bisa gagal/ mengalami gangguan karena FoodGo tidak memiliki mekanisme retry.]

**Solusi desain awal:** [Menmabhakan *retry* untuk mencoba kembali request yang sebelumnya gagal]

**Trade-off:** [*retry* dapat menambah beban jaringan dan service tujuan, jika service sedang overload dan banyak request melakukan retry secara bersamaan. Kondisi ini dapat memperparah `cascading faillure`, karena itu] retry harus dibatasi dan menggunakan backoff

---

## Pitfall 2: [nama pitfall] — ditulis oleh [nama]

(ulangi struktur di atas)

---

## Pitfall 3: [Arsitektur Monolitik (SPOF)] — ditulis oleh [Jeremy Joving Winargo]

**Bukti di skenario:** ["Saat trafik naik, satu server yang menangani semua modul (pesanan, pembayaran, notifikasi kurir) kewalahan karena semuanya berjalan di satu proses monolitik yang sama."]

**Kenapa ini keliru:** [Karena arsitektur monolitik menghalangi fault isolation, sehingga disaat satu modul mengalami lonjakan beban akan berdampak pada modul yang lain, dan juga menghalangi scaling boundary, setiap modul memiliki beban yang berbeda tetapi berada di satu server yang sama, sehingga kita tidak bisa menambah kapasitas kepada modul yang membutuhkan saja,tetapi harus menduplikasi keseluran monolit secara tidak efisien]

**Dampak ke FoodGo:** [terjadinya lonjakan trafik pada modul pesanan atau masalah pada modul notifikasi akan memakan CPU dan memori di sever yang sama. sehingga, modul lain yang seharusnya sehat seperti modul pembayaran ikut melambat. Terjadi Single point of failure yang mewajibkan restart secara keseluruhan]

**Solusi desain awal:** [memisahkan modul monolitik menjadi beberapa bagian kecil(microservices) yang terpisah.Tempatkan tiap service di dalam container yang dapat di-skala secara horizontal berdasarkan beban trafik. ]

**Trade-off:** [] 

---

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.]
