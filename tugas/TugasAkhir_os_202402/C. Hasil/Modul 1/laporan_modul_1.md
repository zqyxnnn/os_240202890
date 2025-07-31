# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: Zakkya Fauzan Alba'asithu
**NIM**: 240202890
**Modul yang Dikerjakan**:
`Modul 1 – System Call dan Instrumentasi Kernel`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 1 – System Call dan Instrumentasi Kernel**
  Pada modul ini, saya diminta menambahkan dua buah system call baru pada kernel `xv6-public`. System call pertama adalah `getpinfo()` yang mengembalikan informasi seluruh proses aktif di sistem (PID, penggunaan memori, dan nama proses). System call kedua adalah `getreadcount()` yang menghitung total pemanggilan fungsi `read()` sejak sistem booting.

---

## 🛠️ Rincian Implementasi

### 🔧 Perubahan Struktural:

* Menambahkan struktur `struct pinfo` ke dalam file `proc.h` untuk menyimpan informasi proses aktif.
* Menambahkan variabel global `readcount` di file `sysproc.c` sebagai penghitung jumlah pemanggilan `read()`.

### ⚙️ Penambahan System Call:

* Menambahkan nomor system call baru `SYS_getpinfo` dan `SYS_getreadcount` pada `syscall.h`.
* Menambahkan deklarasi fungsi system call pada `user.h` dan `usys.S`.
* Mendaftarkan handler system call `sys_getpinfo()` dan `sys_getreadcount()` dalam `syscall.c`.
* Mengimplementasikan kedua fungsi tersebut dalam `sysproc.c`.

### 📝 Perubahan Fungsi `read`:

* Memodifikasi fungsi `sys_read()` dalam `sysfile.c` dengan menambahkan `readcount++` di bagian awal untuk menghitung setiap pemanggilan fungsi `read`.

### 🧪 Pembuatan Program Uji:

* `ptest.c`: Menguji system call `getpinfo()`.
* `rtest.c`: Menguji system call `getreadcount()` sebelum dan sesudah pemanggilan `read()`.

### 🛠️ Build System:

* Menambahkan file `_ptest` dan `_rtest` ke dalam daftar `UPROGS` di `Makefile`.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `ptest`: Menampilkan daftar proses aktif beserta PID, ukuran memori, dan nama proses.
* `rtest`: Menampilkan jumlah pemanggilan `read()` sebelum dan sesudah fungsi `read()` dipanggil.

---

## 📷 Hasil Uji

### 📍 Contoh Output `ptest`

```
$ ptest
PID	MEM	NAME
1	4096	init
2	2048	sh
3	2048	ptest
```

### 📍 Contoh Output `rtest`

```
$ rtest
Read Count Sebelum: 4
hello
Read Count Setelah: 5
```

> Output menunjukkan bahwa `readcount` berhasil bertambah setelah `read()` dipanggil dari user program.

---

## ⚠️ Kendala yang Dihadapi

* Saat pertama kali mengakses `ptable->proc`, kernel mengalami crash karena pointer belum dikonversi dengan benar melalui `argptr()`. Solusi: memperbaiki casting pointer dan memastikan pointer diarahkan dengan benar.
* Struktur `ptable_lock` tidak tersedia pada `xv6-public`, sehingga saya menggunakan `ptable.lock` bawaan dan memastikan semua akses ke tabel proses diamankan menggunakan `acquire()` dan `release()`.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi dan dokumentasi praktikum
* Stack Overflow dan GitHub Issues terkait pengembangan xv6

---
