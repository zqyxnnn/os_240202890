📝 Laporan Tugas Akhir 

Mata Kuliah: Sistem Operasi
Semester: Genap / Tahun Ajaran 2024–2025
Nama: Zakkya Fauzan Alba’asithu
NIM: 240202890
Modul yang Dikerjakan:
Modul 4 – Subsistem Kernel Alternatif (xv6-public)

📌 Deskripsi Singkat Tugas 

Modul ini berfokus pada dua implementasi utama dalam sistem operasi xv6:

• Menambahkan system call chmod(path, mode) untuk mengatur mode file antara read-only dan read-write. Membuat dan mendaftarkan device pseudo /dev/random yang mengembalikan byte acak ketika dibaca. 
• Melalui modul ini, mahasiswa belajar mengenai modifikasi kernel file system serta mekanisme kerja device driver pada sistem Unix-like minimalis seperti xv6.

🛠️ Rincian Implementasi 

✏️ Bagian A: Implementasi System Call chmod (path, mode) Menambahkan field short mode pada struktur struct inode di fs.h untuk menandai mode file (0: read-write, 1: read-only). 
• Menambahkan definisi system call baru chmod: 
○ syscall.h → #define SYS_chmod 27 
○ user.h → deklarasi fungsi: int chmod(char *path, int mode); 
○ usys.S → penambahan syscall macro: SYSCALL(chmod) 
○ syscall.c → pendaftaran syscall baru: [SYS_chmod] sys_chmod, 
• Implementasi fungsi sys_chmod() di sysfile.c: Mencari inode berdasarkan path, lalu mengatur nilai mode. Di file.c, fungsi filewrite() dimodifikasi untuk menolak penulisan jika file mode == 1 (read-only). Menambahkan program uji chmodtest.c untuk memverifikasi fungsionalitas syscall baru. 

🔧 Bagian B: Device Pseudo /dev/random 
• Menambahkan file random.c yang berisi handler randomread() untuk menghasilkan byte acak menggunakan PRNG (Pseudo-Random Number Generator). 
• Mendaftarkan handler randomread() ke dalam array devsw[] di file.c pada index 3 (dev major 3). 
• Menambahkan mknod("/dev/random", 1, 3) di akhir fungsi init() untuk membuat device node. 
• Membuat program uji randomtest.c untuk membaca 8 byte dari /dev/random. 

🛠️ Makefile 
Menambahkan program uji chmodtest dan randomtest pada bagian UPROGS Makefile: 
_chmodtest\ 
_randomtest\ 

✅ Uji Fungsionalitas 
Program uji yang digunakan:
• chmodtest.c → menguji system call chmod() dan menolak penulisan file dalam mode read-only. 
• randomtest.c → membaca byte dari /dev/random. 📷 Hasil Uji 

📍 Output chmodtest: 
Write blocked as expected 
📍 Output randomtest (berubah-ubah sesuai hasil acak): 
12 87 203 19 221 3 177 240 

⚠️ Kendala yang Dihadapi 

• Pada awalnya, sistem tidak menolak penulisan file read-only karena modifikasi pada filewrite() belum mempertimbangkan nilai ip->mode. Solusinya adalah memeriksa nilai f->ip->mode sebelum melakukan writei(). 
• Kesalahan awal dalam menempatkan mknod() bukan di akhir init(), menyebabkan device /dev/random tidak muncul otomatis saat boot. 
• Debugging cukup menantang karena xv6 memiliki sistem logging yang minim. 

📚 Referensi Buku xv6 MIT: https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf 
Repositori xv6-public: https://github.com/mit-pdos/xv6-public 
Diskusi internal praktikum dan dokumentasi fungsi kernel xv6 Stack Overflow untuk debugging system call dan device driver 
