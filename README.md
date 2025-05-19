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