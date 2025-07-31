📝 Laporan Tugas Akhir 
Mata Kuliah: Sistem Operasi
Semester: Genap / Tahun Ajaran 2024–2025
Nama: Zakkya Fauzan Alba’asithu 
NIM: 24202890
Modul yang Dikerjakan:
Modul 3 – Manajemen Memori Tingkat Lanjut (xv6-public x86)

📌 Deskripsi Singkat Tugas
Mengimplementasikan dua fitur memori tingkat lanjut pada kernel xv6: Copy-on-Write Fork – Optimalisasi fungsi fork() agar lebih efisien dalam penggunaan memori melalui teknik copy-on-write. 
Shared Memory ala System V – Menambahkan kemampuan antar proses untuk saling berbagi satu halaman memori menggunakan sistem shmget() dan shmrelease() dengan mekanisme reference counting. 

🛠️ Rincian Implementasi Copy-on-Write Fork: Menambahkan array ref_count[] dan fungsi incref() / decref() pada vm.c untuk melacak penggunaan memori fisik. Menambahkan flag baru PTE_COW pada mmu.h sebagai penanda halaman COW. Mengganti fungsi copyuvm() menjadi cowuvm() yang hanya menyalin page table dengan flag COW, bukan isi halaman. Memodifikasi fork() di proc.c agar menggunakan cowuvm() alih-alih copyuvm(). Menangani page fault di trap.c untuk mendeteksi penulisan ke halaman COW dan membuat salinan fisik secara langsung. Shared Memory ala System V: Menambahkan struktur shmtab[] di vm.c untuk menyimpan key, frame, dan refcount. Menambahkan syscall baru shmget() dan shmrelease() pada sysproc.c untuk mengakses shared memory berdasarkan key. Mendaftarkan syscall baru di syscall.h, syscall.c, user.h, dan usys.S. ✅ Uji Fungsionalitas 

Program uji yang digunakan:

cowtest.c — menguji apakah fork() menggunakan Copy-on-Write, dan apakah halaman hanya disalin saat terjadi penulisan (write). shmtest.c — menguji apakah shared memory dapat diakses oleh proses parent dan child, dan data dapat terbagi secara konsisten. 📷 Hasil Uji 📍 Contoh Output cowtest: Child sees: Y Parent sees: X 

Hasil ini menunjukkan bahwa halaman hanya disalin ketika child menulis, bukan saat fork().

📍 Contoh Output shmtest: Child reads: A Parent reads: B 

Hasil ini menunjukkan bahwa child berhasil membaca isi dari halaman shared memory yang ditulis parent, dan sebaliknya.

⚠️ Kendala yang Dihadapi 
Beberapa kali terjadi panic karena page fault tidak ditangani dengan benar (akses halaman tanpa PTE_P atau PTE_COW). 
Salah alamat pemetaan shared memory menyebabkan proses membaca halaman yang salah. 
Lupa mengurangi ref_count pada saat halaman tidak lagi digunakan, menyebabkan memory leak.

📚 Referensi 
MIT xv6 Book (x86): https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf 
Repositori xv6-public: https://github.com/mit-pdos/xv6-public Diskusi praktikum dan dokumentasi internal Stack Overflow dan forum diskusi kernel open source.
