# Tugas 2 (Pekan 2) — Perancangan Arsitektur untuk FoodGo

**Materi terkait:** Architectural style (Layered, SOA, Peer-to-Peer, Publish-Subscribe).

**Kelompok :** Kelompok 2

| NAMA                                  | NIM          |
| ------------------------------------- | ------------ |
| Yan Chrisdaniel Partogi rayano Ludjen | 103072400010 |
| Bima Luthfi Nurhakim                  | 103072400030 |
| Jeremy Joving Winargo                 | 103072400085 |
| Ahmad Nur Fajri                       | 103072430007 |

## Studi Kasus

Melanjutkan Tugas 1: FoodGo butuh sistem yang **decoupled** agar tim kurir dan tim resto tidak saling mengganggu ketika salah satu modul diperbarui/deploy ulang. Saat ini semua modul (pesanan, pembayaran, notifikasi kurir, katalog resto) berjalan sebagai satu aplikasi monolitik — sekali deploy, semua modul ikut restart dan berisiko downtime total.

## Tugas Kelompok

1. Pilih **satu** gaya arsitektur utama: **Service-Oriented Architecture (SOA)** atau **Publish-Subscribe**. Boleh dikombinasikan (mis. SOA untuk service inti + Pub-Sub untuk notifikasi), tapi harus dijustifikasi kenapa kombinasi ini yang dipilih.
2. Gambarkan minimal 4 komponen berikut dan interaksinya: modul Pesanan, modul Pembayaran, modul Kurir/Notifikasi, modul Katalog Resto (dan message broker/API gateway jika relevan).
3. Jelaskan alur satu skenario penuh secara end-to-end di diagram (misalnya: pelanggan buat pesanan → bayar → resto terima notifikasi → kurir ditugaskan) — tunjukkan komponen mana berkomunikasi dengan siapa, dan **jenis komunikasinya** (sinkron/asinkron, request-response/event).
4. Analisis tertulis: kenapa gaya ini mengatasi masalah _coupling_ dari Tugas 1, dan apa trade-off-nya (mis. Pub-Sub menambah kompleksitas debugging karena alur tidak linear).

## Hasil Diskusi

### Planning Arsitektuk 
Memutuskan untuk memilih Service Oriented Architecture sebagai arsitektur utamanya dan memilih Publish Subscribe sebagai pola komunikasi asinkron untuk event dan notifikasi. mengapa kami memilih SOA tersebut, karena kebutuhan utamanya adalah memisahkan fungsi pesanan, pembayaran, katalog resto, dan kurir/notifikasi menjadi layanan dengan tanggung jawab dan kontrak yang jelas. Tim dapat mengembangkan dan memperbarui layanan secara terpisah. Lalu mengapa kita juga memilih Publish-Subcribe, karena untuk melengkapi SOA. ia digunakan sebagai alur yang tidak perlu menahan permintaan pelanggan sambil menunggu layanan lain, khususnya hasil pembayaran dan pemberitahuan ke resto maupun kurir. Jadi Pub-Sub bukan gaya arsitektuk utama kedua yang berdiri sendiri, ia merupakan pola komunikasi antar layanan dalam rancangan SOA FoodGo.  


