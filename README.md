## Task 2 - Liburan Bersama Rudi

### Soal
Mengisi waktu liburan, Rudi membuat sebuah website untuk personal brandingnya yang sementara berjalan di local pada komputer laboratorium. Rudi yang baru belajar kebingungan karena sering muncul error yang tidak ia pahami. Untuk itu dia meminta ketiga temannya Andi, Budi, dan Caca untuk melakukan pengecekan secara berkala pada website Rudi. Dari pengecekan secara berkala, Rudi mendapatkan sebuah file [access.log](https://drive.google.com/file/d/1yf4lWB4lUgq4uxKP8Pr8pqcAWytc3eR4/view?usp=sharing) yang berisi catatan akses dari ketiga temannya. Namun, Rudi masih tidak paham cara untuk membaca data pada file tersebut sehingga meminta bantuanmu untuk mencari data yang dibutuhkan Rudi.

a. Karena melihat ada IP dan Status Code pada file access.log. Rudi meminta praktikan untuk menampilkan total request yang dibuat oleh setiap IP dan menampilkan jumlah dari setiap status code.

b. Karena banyaknya status code error, Rudi ingin tahu siapa yang menemukan error tersebut. Setelah melihat-lihat, ternyata IP komputer selalu sama. Dengan bantuan [peminjaman_komputer.csv](https://drive.google.com/file/d/1-aN4Ca0M3IQdp6xh3PiS_rLQeLVT1IWt/view?usp=drive_link), Rudi meminta kamu untuk membuat sebuah program bash yang akan menerima inputan tanggal dan IP serta menampilkan siapa pengguna dan membuat file backup log aktivitas, dengan format berikut:

- **Tanggal** (format: `MM/DD/YYYY`)

- **IP Address** (format: `192.168.1.X`, karena menggunakan jaringan lokal, di mana `X` adalah nomor komputer)

- Setelah pengecekan, program akan memberikan **message pengguna dan log aktivitas** dengan format berikut:

  ```
  Pengguna saat itu adalah [Nama Pengguna Komputer]
  Log Aktivitas [Nama Pengguna Komputer]
  ```

  atau jika data tidak ditemukan:

  ```
  Data yang kamu cari tidak ada
  ```

- File akan disimpan pada directory “/backup/[Nama file]”, dengan format nama file sebagai berikut

  ```
  [Nama Pengguna Komputer]_[Tanggal Dipilih (MMDDYYY)]_[Jam saat ini (HHMMSS)].log
  ```

- Format isi log

  ```
  [dd/mm/yyyy:hh:mm:ss]: Method - Endpoint - Status Code
  ```

c. Rudi ingin memberikan hadiah kepada temannya yang sudah membantu. Namun karena dana yang terbatas, Rudi hanya akan memberikan hadiah kepada teman yang berhasil menemukan server error dengan `Status Code 500` terbanyak. Bantu Rudi untuk menemukan siapa dari ketiga temannya yang berhak mendapat hadiah dan tampilkan jumlah `Status Code 500` yang ditemukan

---

### Penyelesaian

a. Code Lengkap

```bash
#!/bin/bash

LOG_PATH="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/access.log"

if [[ ! -f "$LOG_PATH" ]]; then
    echo "Error: File access.log tidak ditemukan di $LOG_PATH"
    exit 1
fi

echo "Total Request yang dibuat oleh setiap IP:"
awk '{print $1}' "$LOG_PATH" | sort | uniq -c | sort -nr

echo ""
echo "Jumlah dari setiap status code:"
awk '{print $9}' "$LOG_PATH" | sort | uniq -c | sort -nr
```
Penjelasan :
```bash
#!/bin/bash
```
Baris pertama pada kode ini adalah shebang yang berfungsi sebagai tanda yang menunjukkan interpreter apa yang akan digunakan untuk menjalankan script ini. Dalam hal ini, script akan dijalankan menggunakan Bash, sebuah shell di sistem Linux/Unix.

```bash
LOG_PATH="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/access.log"
```
LOG_PATH adalah sebuah variabel yang menyimpan lokasi (path) file access.log. Jadi daripada saya menuliskan path yang panjang berkali-kali, saya menggunakan variabel ini agar lebih rapi.
Tanda kutip ganda (") digunakan agar value dari variabel tetap utuh, meskipun nanti ada karakter khusus. Jika saya memakai $LOG_PATH di script ini, Bash akan menggantinya dengan isi dari path yang didefinisikan.

```bash
if [[ ! -f "$LOG_PATH" ]]; then
    echo "Error: File access.log tidak ditemukan di $LOG_PATH"
    exit 1
fi
```
pada bagian ini adalah conditional statement yang mengecek apakah file access.log ada. Berikut adalah penjelasannya :
- `if` adalah pernyataan kondisi yang digunakan untuk memeriksa apakah suatu kondisi benar.
- `[[ ... ]]` adalah kondisi atau ekspresi yang dicek oleh `if`.
Ini disebut test brackets dalam Bash, dan digunakan untuk membuat perbandingan.
- `!` berarti negasi (not), yang digunakan untuk membalik kondisi.
- `-f` digunakan untuk mengecek apakah file yang diberikan (dalam hal ini `"$LOG_PATH"`) benar-benar ada.
- `;` digunakan sebagai pemisah agar perintah then dapat ditulis di baris yang sama.
- `then` digunakan untuk memulai blok kode yang akan dieksekusi jika kondisi di dalam `if` bernilai benar.
- `echo` digunakan untuk mencetak teks atau pesan ke terminal. Dalam hal ini, pesan yang dicetak adalah "Error: File access.log tidak ditemukan di $LOG_PATH".
- `$` digunakan untuk memanggil nilai dari variabel.
- `exit` digunakan untuk menghentikan eksekusi script dan keluar dari program. Angka 1 pada exit ini menandakan bahwa script berakhir karena ada kesalahan (error).
- `fi` adalah penutup dari pernyataan `if`. Jika di awal kita menggunakan `if` untuk memulai blok kode, maka blok tersebut harus ditutup dengan `fi`.

```bash
awk '{print $1}' "$LOG_PATH" | sort | uniq -c | sort -nr
```
- `awk` adalah alat pemrosesan teks yang kuat dalam Unix/Linux.
Dalam perintah ini, `awk` digunakan untuk membaca file [access.log](https://drive.google.com/file/d/1yf4lWB4lUgq4uxKP8Pr8pqcAWytc3eR4/view?usp=sharing) dan mencetak kolom pertama dari setiap baris.
- `'{print $1}'` adalah aturan yang diberikan kepada `awk` dan tanda `$1` berarti kolom pertama pada setiap baris. Jadi, perintah ini akan mencetak alamat IP dari setiap baris di file [access.log](https://drive.google.com/file/d/1yf4lWB4lUgq4uxKP8Pr8pqcAWytc3eR4/view?usp=sharing).

  `|` (Pipa atau Pipe)
- Simbol ini disebut pipe dan digunakan untuk mengalirkan output dari satu perintah ke perintah berikutnya.
  Contoh: Output dari perintah `awk` akan dikirim langsung sebagai input untuk perintah berikutnya, yaitu `sort`.

  `sort`
- Perintah ini digunakan untuk mengurutkan data secara alfabetis atau numerik. Di sini, `sort` akan mengurutkan daftar IP yang dihasilkan oleh `awk`.

  `uniq -c`
- `uniq` digunakan untuk menghapus baris duplikat dari output yang sudah diurutkan sebelumnya dan `-c` adalah opsi yang akan menghitung jumlah kemunculan setiap baris unik. Jadi, perintah ini akan menghitung berapa kali setiap IP muncul di file log.

  `sort -nr`
- `-n` digunakan untuk mengurutkan data berdasarkan angka (secara numerik) dan `-r` digunakan untuk mengurutkan data dalam urutan menurun (dari terbesar ke terkecil). Jadi perintah ini akan mengurutkan IP berdasarkan jumlah request yang dibuat, dimulai dari IP dengan jumlah request terbanyak.

- `echo ""` berfungsi menampilkan baris kosong pada terminal.
```bash
awk '{print $9}' "$LOG_PATH" | sort | uniq -c | sort -nr
```
- Bagian ini sama seperti bagian sebelumnya, hanya saja pada bagian ini mengambil kolom ke-9.

- Single quote `(')` : Semua karakter di dalam single quote akan dianggap sebagai string.
- Double quote `(")`	: Karakter `$`, `, dan \ dalam double quote dapat digunakan sebagaimana fungsinya di shell.
### Foto Hasil Output
![image](https://github.com/user-attachments/assets/dba11e8a-7dad-4694-99d0-075a63ffca73)
