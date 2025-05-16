# 🧪 Tutorial Praktikum xv6

## 🔧 Topik: Manajemen Proses, System Call, dan Penjadwalan Proses

---

## 🎯 Tujuan Umum

* Mahasiswa mampu menelusuri dan memahami implementasi *fork()*, *exec()*, dan *exit()* pada xv6.
* Mahasiswa mampu menambahkan system call baru.
* Mahasiswa memahami dan memodifikasi algoritma penjadwalan proses pada kernel xv6.

---

## 🗂️ Persiapan Awal

### 1. Clone xv6

```bash
git clone https://github.com/mit-pdos/xv6-public.git
cd xv6-public
```

### 2. Build & Run

```bash
make qemu-nox
```

---

## 📘 Bagian 1: Manajemen Proses di xv6

### 🔍 Konsep

Manajemen proses mencakup:

* **fork()**: membuat proses anak (copy).
* **exec()**: mengganti image proses dengan program baru.
* **exit()**: keluar dari proses.
* **wait()**: menunggu anak selesai.

---

### 🔎 Eksplorasi Kode

**File utama: `proc.c`, `proc.h`, `sysproc.c`, `usys.S`**

1. `fork()`: Implementasi di `proc.c`, fungsi `fork()`
2. `exit()`: Fungsi `exit()` di `proc.c`
3. `wait()`: Tunggu child di `proc.c`
4. `exec()`: Fungsi `exec()` di `exec.c`

---

### 🧪 Praktik 1: Tambahkan System Call `helloworld`

#### A. **Tambahkan Nomor System Call**

**File:** `syscall.h`

Tambahkan di bagian akhir:

```c
#define SYS_helloworld 24  
```

---

#### B. **Tambahkan Entri ke `syscall.c`**

**File:** `syscall.c`

Tambahkan deklarasi eksternal fungsi:

```c
extern int sys_helloworld(void);
```

Tambahkan ke array `syscalls[]`:

```c
[SYS_helloworld] sys_helloworld,
```

---

#### C. **Definisikan Fungsi `sys_helloworld`**

**File:** `sysproc.c`

Tambahkan:

```c
int sys_helloworld(void) {
  cprintf("Hello from xv6 kernel [NIM_NAMA]!\n");
  return 0;
}
```

---

#### D. **Tambahkan ke `usys.S`**

**File:** `usys.S`

Tambahkan entri:

```asm
SYSCALL(helloworld)
```

⚠️ **Pastikan urutan penulisan sama dan tidak typo.**

---

#### E. **Tambahkan Deklarasi ke `user.h`**

**File:** `user.h`

Tambahkan:

```c
int helloworld(void);
```

---

#### F. **(Opsional) Tambahkan Program Penguji**

**Buat file baru misal `hello.c`:**

```c
#include "types.h"
#include "stat.h"
#include "user.h"

int main(void) {
  helloworld();
  exit();
}
```

Tambahkan ke Makefile (`UPROGS`):

```make
UPROGS=\
  _hello\
  ...
```

---

### 🔁 Terakhir: Jalankan `make clean && make qemu-nox`

```bash
make clean
make qemu-nox
```

---

## ⚙️ Bagian 2: Penjadwalan Proses

### 🔍 Konsep

* Penjadwalan menentukan proses mana yang dijalankan.
* **xv6 default** menggunakan *round-robin*.
* Bisa dimodifikasi menjadi *priority-based* scheduler.

---

### 🔎 Eksplorasi Kode Scheduler

**File penting:** `proc.c`

Fungsi scheduler:

```c
void scheduler(void)
```

* Menelusuri semua proses.
* Menjadwalkan proses yang `RUNNABLE`.

---
