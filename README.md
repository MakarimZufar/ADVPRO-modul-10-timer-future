# ADVPRO Modul 10

## Nama: Makarim Zufar Prambudyo

## NPM: 2306241751

## Kelas: ADVPRO-B

----
### Experiment 1.2: Understanding how it works

![Experiment 1.2 zufar's console](/README_image/Screenshot%202025-05-19%20160954.png)
Ketika program dijalankan, kita melihat urutan output: `"hey hey"` muncul pertama, diikuti oleh `"howdy!"`, dan akhirnya `"done!"`. Fenomena ini terjadi karena mekanisme berikut:
#### Alasan Urutan Eksekusi

1. Proses Thread Utama:
Perintah println!("Zufar's Komputer: hey hey") langsung dieksekusi oleh thread utama
Eksekusi ini terjadi secara langsung sebelum executor asinkron mulai bekerja

2. Sistem Antrian Task:
Fungsi spawner.spawn(...) hanya memasukkan tugas ke dalam antrian
Tugas-tugas ini belum dijalankan saat didaftarkan, hanya disimpan


3. Proses Eksekusi Asinkron:
Tugas-tugas baru dijalankan setelah pemanggilan executor.run()
Output "howdy!" muncul saat tugas pertama dieksekusi
Kemudian terjadi penundaan selama dua detik (TimerFuture)
Terakhir, "done!" ditampilkan setelah waktu tunggu selesai

Urutan ini menggambarkan bagaimana proses sinkron dan asinkron berinteraksi dalam lingkungan eksekusi program.

### Experiment 1.3: Multiple Spawn and removing drop

#### multiple Spawn:

![multiple spawn console screenshots](/README_image/Screenshot%202025-05-19%20162330.png)
Saat menggunakan beberapa operasi spawn, seluruh tugas akan dieksekusi secara konkuren. Hal ini menimbulkan fenomena menarik pada output program. Kita dapat mengamati bahwa pesan "done!" muncul setelah "done2!", meskipun dalam kode sumber urutan pendefinisiannya terbalik.
#### Karakteristik Eksekusi Asinkron
Perilaku ini menunjukkan sifat asinkron yang penting:

- Task-task diproses secara bersamaan, bukan berurutan
- Waktu penyelesaian setiap task bergantung pada beberapa faktor
- Urutan penyelesaian tidak dapat diprediksi dengan pasti
- Task yang didefinisikan lebih awal tidak selalu selesai lebih dulu

Kesimpulannya, pada pemrograman asinkron dengan multiple spawn, urutan penyelesaian task bisa bervariasi setiap kali program dijalankan dan tidak selalu mengikuti urutan pendefinisian dalam kode.

#### multiple Spawn without drop spawner:
![multiple Spawn without drop spawner scanner](/README_image/Screenshot%202025-05-19%20162814.png)
Ketika instruksi drop(spawner) dihilangkan dari kode, kita dapat mengamati bahwa program tetap berjalan namun tidak pernah berakhir. Fenomena ini memiliki penjelasan teknis yang penting:

##### Mekanisme Pengakhiran Program Asinkron
Tanpa perintah drop(spawner), sistem mengalami kondisi berikut:

- Eksekutor terus dalam keadaan aktif, menantikan kemungkinan tugas baru
- Program tidak memiliki cara untuk mengetahui bahwa semua tugas telah selesai
- Sumber daya tetap dialokasikan meskipun tidak ada operasi yang dijalankan

Fungsi drop(spawner) berperan krusial sebagai sinyal kepada sistem bahwa:

- Tidak akan ada pendaftaran tugas baru ke dalam antrian
- Eksekutor dapat berhenti setelah menyelesaikan tugas-tugas yang tersisa
- Sumber daya yang dialokasikan untuk spawner dapat dibebaskan

Dengan demikian, drop(spawner) merupakan komponen penting untuk terminasi yang benar pada program asinkron.