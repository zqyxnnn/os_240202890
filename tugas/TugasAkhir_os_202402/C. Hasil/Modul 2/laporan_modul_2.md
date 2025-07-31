# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: Zakkya Fauzan Alba'asithu
**NIM**: 240202890
**Modul yang Dikerjakan**:
`Modul 2 – Penjadwalan CPU Lanjutan (Priority Scheduling Non-Preemptive)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 2 – Penjadwalan CPU Lanjutan (Priority Scheduling Non-Preemptive)**:
  Pada modul ini, dilakukan perubahan algoritma penjadwalan proses pada kernel `xv6-public`. Algoritma default Round Robin diubah menjadi Non-Preemptive Priority Scheduling. Implementasi melibatkan penambahan field `priority` pada struktur proses dan penambahan system call `set_priority(int)` agar proses dapat mengatur prioritasnya. Scheduler dimodifikasi agar selalu menjalankan proses RUNNABLE dengan prioritas tertinggi (nilai numerik terkecil).

---

## 🛠️ Rincian Implementasi

* Menambahkan field `priority` ke `struct proc` di file `proc.h`
* Inisialisasi nilai `priority` default pada proses baru di `allocproc()` (`proc.c`)
* Membuat system call baru `set_priority(int)`:

  * Tambah definisi syscall di `syscall.h`, `user.h`, `usys.S`
  * Registrasi syscall pada `syscall.c`
  * Implementasi fungsi `sys_set_priority()` di `sysproc.c`
* Memodifikasi fungsi `scheduler()` di `proc.c` untuk memilih proses RUNNABLE dengan prioritas tertinggi (angka kecil = prioritas tinggi)
* Membuat program uji `ptest.c` untuk menguji penjadwalan berdasarkan prioritas
* Mendaftarkan program uji `ptest` ke dalam `Makefile`

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `ptest`: Untuk menguji apakah proses dengan prioritas lebih tinggi (angka lebih kecil) dijalankan lebih dulu dibanding proses prioritas lebih rendah, meskipun proses lain sudah dalam keadaan RUNNABLE.

---

## 📷 Hasil Uji

### 📍 Contoh Output `ptest`

```
$ ptest
Child 2 selesai
Child 1 selesai
Parent selesai
```

Penjelasan:

* `Child 2` memiliki prioritas 10 → dijalankan terlebih dahulu.
* `Child 1` memiliki prioritas 90 → menunggu hingga `Child 2` selesai.
* `Parent` menunggu kedua anak selesai sebelum mencetak output terakhir.

---

## ⚠️ Kendala yang Dihadapi

* Saat pertama kali menguji scheduler, kedua proses anak tampak berjalan tidak sesuai prioritas. Setelah ditelusuri, penyebabnya adalah `priority` tidak diinisialisasi dengan benar di `allocproc()`.
* Karena sifat non-preemptive, jika satu proses dengan prioritas tinggi tidak pernah melepaskan CPU (misalnya loop tanpa `yield`), proses lain tidak akan pernah dijadwalkan — ini sifat yang memang diharapkan namun perlu dipahami dengan benar saat menguji.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
