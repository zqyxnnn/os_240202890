📝 Laporan Tugas Akhir 

Mata Kuliah: Sistem Operasi
Semester: Genap / Tahun Ajaran 2024–2025
Nama: Zakkya Fauzan Alba’asithu
NIM: 240202890
Modul yang Dikerjakan:
Modul 5 – Audit dan Keamanan Sistem (xv6-public)

📌 Deskripsi Singkat Tugas Modul 5 – Audit dan Keamanan Sistem:
Modul ini bertujuan untuk menambahkan fitur audit pada kernel xv6, yang merekam semua pemanggilan system call yang terjadi selama runtime. Log audit ini hanya dapat diakses oleh proses dengan PID 1 (init), sebagai bentuk mekanisme keamanan dan proteksi data kernel. Selain itu, sebuah system call baru get_audit_log() juga diimplementasikan untuk membaca log secara terbatas. 🛠️ Rincian Implementasi 

Berikut langkah-langkah dan perubahan yang dilakukan dalam modul ini:

Menambahkan struktur audit_entry dan buffer audit_log[] untuk menyimpan data audit di syscall.c Menambahkan proses pencatatan system call di fungsi syscall() dengan memeriksa validitas syscall Menambahkan system call baru get_audit_log(): Tambahan definisi syscall di syscall.h Deklarasi fungsi user-level di user.h Pendaftaran syscall di usys.S dan array syscalls[] di syscall.c Implementasi syscall di sysproc.c dengan proteksi agar hanya PID 1 yang bisa mengakses log Membuat program uji audit.c untuk mengambil dan mencetak isi log audit Mengubah Makefile untuk menyertakan file _audit Mengedit init.c agar program audit berjalan sebagai proses pertama (PID 1), menggantikan shell ✅ Uji Fungsionalitas 

Program uji yang digunakan:

audit:
Program ini digunakan untuk mengambil dan mencetak isi dari audit log. Program hanya bisa mengakses log jika dijalankan sebagai proses pertama (PID 1). Bila dijalankan oleh proses lain, maka akan muncul pesan error. 📷 Hasil Uji 📍 Contoh Output audit (jika dijalankan oleh PID 1): === Audit Log === [0] PID=1 SYSCALL=5 TICK=12 [1] PID=1 SYSCALL=6 TICK=13 [2] PID=1 SYSCALL=22 TICK=14 [3] PID=1 SYSCALL=23 TICK=15 📍 Contoh Output audit (jika dijalankan sebagai proses biasa): Access denied or error. 

Catatan: Untuk menghasilkan output log, program audit dijalankan sebagai init dengan mengubah exec("sh", ...) menjadi exec("audit", argv) di init.c.

⚠️ Kendala yang Dihadapi Awalnya lupa menambahkan validasi PID di sys_get_audit_log(), sehingga semua proses bisa membaca log Salah menangani argptr() menyebabkan kernel panic saat mengakses buffer audit Proses audit tidak bisa berjalan sebagai PID 1 karena urutan eksekusi di init.c belum diubah Log audit penuh jika melebihi MAX_AUDIT, karena belum ada mekanisme rotasi log 📚 Referensi Buku xv6 MIT: https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf Repositori xv6-public: https://github.com/mit-pdos/xv6-public
