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

a. Code lengkap :

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


b. Code lengkap :
```bash
#!/bin/bash

ACCESS_LOG="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/access.log"
PEMINJAMAN_CSV="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/peminjaman_computer.csv"
BACKUP_DIR="/home/aji-zaenul-musthofa/backup"

convert_date() {
    input_date="$1"
    month=$(echo "$input_date" | cut -d'/' -f1)
    day=$(echo "$input_date" | cut -d'/' -f2)
    year=$(echo "$input_date" | cut -d'/' -f3)

    months=(Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec)
    month_name=${months[$((month - 1))]}

    echo "$day/$month_name/$year"
}

read -p "Masukkan tanggal (MM/DD/YYYY): " INPUT_DATE
read -p "Masukkan IP Address (192.168.1.X): " INPUT_IP

LOG_DATE=$(convert_date "$INPUT_DATE")

PC_NUMBER=$(echo "$INPUT_IP" | awk -F'.' '{print $4}')

USER_NAME=$(awk -F',' -v date="$INPUT_DATE" -v pc="$PC_NUMBER" '$1 == date && $2 == pc {print $3}' "$PEMINJAMAN_CSV")

if [[ -z "$USER_NAME" ]]; then
    echo "Data yang kamu cari tidak ada."
    exit 1
fi

echo "Pengguna saat itu adalah $USER_NAME"

LOG_RESULT=$(grep "$LOG_DATE" "$ACCESS_LOG" | grep "$INPUT_IP")

if [[ -z "$LOG_RESULT" ]]; then
    echo "Tidak ada aktivitas yang ditemukan untuk pengguna ini."
    exit 1
fi

FORMATTED_LOG=$(echo "$LOG_RESULT" | awk '{
    split($4, waktu, ":");
    tanggal = substr(waktu[1], 2);
    jam = waktu[2];
    menit = waktu[3];
    detik = waktu[4];

    method = substr($6, 2);
    endpoint = $7;
    status = $9;

    printf "[%s:%s:%s:%s]: %s - %s - %s\n", tanggal, jam, menit, detik, method, endpoint, status;
}')

mkdir -p "$BACKUP_DIR"

TIMESTAMP=$(date +"%H%M%S")
BACKUP_FILE="$BACKUP_DIR/${USER_NAME}_$(echo $INPUT_DATE | tr -d '/')_${TIMESTAMP}.log"

echo "$FORMATTED_LOG" > "$BACKUP_FILE"

echo "Log Aktivitas $USER_NAME"
```
Penjelasan :
```bash
#!/bin/bash
```
Baris pertama pada kode ini adalah shebang yang berfungsi sebagai tanda yang menunjukkan interpreter apa yang akan digunakan untuk menjalankan script ini. Dalam hal ini, script akan dijalankan menggunakan Bash, sebuah shell di sistem Linux/Unix.

```bash
ACCESS_LOG="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/access.log"
```
Mendefinisikan variabel ACCESS_LOG yang berisi path lengkap menuju file access.log.
```bash
PEMINJAMAN_CSV="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/peminjaman_computer.csv"
```
Mendefinisikan variabel PEMINJAMAN_CSV yang berisi path ke file peminjaman_computer.csv.
```bash
BACKUP_DIR="/home/aji-zaenul-musthofa/backup"
```
Mendefinisikan variabel BACKUP_DIR yang menyimpan path ke direktori tempat file backup log akan disimpan.

```bash
convert_date() {
    input_date="$1"
    month=$(echo "$input_date" | cut -d'/' -f1)
    day=$(echo "$input_date" | cut -d'/' -f2)
    year=$(echo "$input_date" | cut -d'/' -f3)

    months=(Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec)
    month_name=${months[$((month - 1))]}

    echo "$day/$month_name/$year"
}
```
Fungsi convert_date bertugas mengonversi format tanggal dari MM/DD/YYYY (misalnya "01/17/2025") menjadi format dd/Mon/yyyy (misalnya "17/Jan/2025"). Berikut adalah penjelasannya :
- `input_date="$1"` berfungsi mengambil argumen pertama yang diberikan ke fungsi dan menyimpannya ke variabel input_date.
  Catatan: Tanda $1 menunjukkan argumen pertama.
- `month=$(echo "$input_date" | cut -d'/' -f1)
day=$(echo "$input_date" | cut -d'/' -f2)
year=$(echo "$input_date" | cut -d'/' -f3)
` mempunyai fungsi masing-masing, yaitu :
  - `echo "$input_date"`untuk menampilkan nilai dari `input_date`.
  - Pipa `|` untuk mengalirkan output dari `echo` ke perintah berikutnya.
  - `cut -d'/' -f1` :
    - `cut` perintah untuk memotong (extract) bagian dari string.
    - `-d'/'` menentukan bahwa pemisah (delimiter) adalah karakter garis miring (`/`).
    - `-f1` untuk mengambil field pertama (yaitu bulan).
  - `-f2` dan `-f3` sama seperti `-f1`, namun mengambil field kedua (hari) dan ketiga (tahun).

```bash
    months=(Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec)
    month_name=${months[$((month - 1))]}
```
- `months=(...)` untuk mendefinisikan array bernama months yang berisi singkatan nama bulan dari Januari hingga Desember.
- `month_name=${months[$((month - 1))]}`
  - `$((month - 1))` melakukan perhitungan aritmatika untuk mendapatkan indeks array. Karena indeks array dimulai dari 0, maka nilai bulan (misalnya 1 untuk Januari) harus dikurangi 1.
  - `${months[...]}` untuk mengambil elemen dari array months pada indeks yang sudah dihitung.
  - Tanda `$` dan `{}` :
    - `$` untuk mengakses nilai variabel.
    - `{}` berguna agar Bash mengetahui batas variabel yang diakses.
    - `[...]` mengindikasikan akses ke elemen array pada indeks tertentu.
- `echo "$day/$month_name/$year"` berfungsi untuk mencetak hasil tanggal dalam format dd/Mon/yyyy dan tanda double quote (`""`) untuk menjaga agar spasi atau karakter khusus tidak dipisah dan agar variabel dapat dievaluasi (nilai variabel akan digantikan dengan isinya).

```bash
read -p "Masukkan tanggal (MM/DD/YYYY): " INPUT_DATE
```
Bagian ini berfungsi sebagai pengambilan input dari pengguna. Berikut adalah penjelasannya :
- `read` berfungsi membaca input dari pengguna.
- Opsi `-p` berfungsi menampilkan prompt (teks pesan) kepada pengguna.
- Teks dalam tanda kutip (`"Masukkan tanggal (MM/DD/YYYY): "`) berfungsi sebagai teks promt yang akan dilihat oleh pengguna.
- `INPUT_DATE` adalah variabel yang akan menyimpan input pengguna.
```bash
read -p "Masukkan IP Address (192.168.1.X): " INPUT_IP
```
Sama seperti di atas, namun meminta IP Address dari pengguna dan menyimpannya di variabel `INPUT_IP`.

```bash
LOG_DATE=$(convert_date "$INPUT_DATE")
```
Bagian ini berfungsi memanggil fungsi `convert_date` dengan `INPUT_DATE` sebagai argumen, dan menyimpan hasil konversi (tanggal dalam format dd/Mon/yyyy) ke variabel `LOG_DATE`. Bagian `$(...)` untuk substitusi perintah yang menjalankan perintah di dalamnya dan mengembalikan outputnya.

```bash
PC_NUMBER=$(echo "$INPUT_IP" | awk -F'.' '{print $4}')
```
Bagian ini berfungsi mengekstrak nomor komputer dari IP Address. Berikut penjelasan per bagian :
- `echo "$INPUT_IP"` untuk mencetak IP Address yang diberikan pengguna.
- Pipa `|` untuk mengalirkan output dari echo ke perintah awk.
- `awk -F'.' '{print $4}'` :
  - `-F'.'` untuk menetapkan pemisah (delimiter) sebagai titik (`.`) sehingga IP Address dipisahkan menjadi empat bagian.
  - `'{print $4}'` perintah untuk mencetak bagian keempat (misalnya, jika IP adalah "192.168.1.2", maka outputnya adalah "2").
- Hasilnya akan disimpan pada variabel `PC_NUMBER`.


```bash
USER_NAME=$(awk -F',' -v date="$INPUT_DATE" -v pc="$PC_NUMBER" '$1 == date && $2 == pc {print $3}' "$PEMINJAMAN_CSV")
```
Bagian ini berfungsi untuk mencari dan mengambil nama pengguna dari file CSV. Berikut ini adalah penjelasan per bagian :
- `awk -F','` untuk menetapkan pemisah field (delimiter) sebagai koma, sesuai format CSV.
- `-v date="$INPUT_DATE"` mendefinisikan variabel `date` di dalam program awk dengan nilai dari `INPUT_DATE`.
- `-v pc="$PC_NUMBER"` mendefinisikan variabel `pc` di dalam awk dengan nilai dari `PC_NUMBER`.
- `'$1 == date && $2 == pc {print $3}'` :
  - `$1` field pertama dari CSV.
  - `$2` field pertama dari CSV.
  - `$3` field pertama dari CSV.
  - Jika field pertama sama dengan date dan field kedua sama dengan pc, maka perintah awk akan mencetak field ketiga.
- `"$PEMINJAMAN_CSV"` menunjukkan file CSV yang akan diproses.
Kemudian hasilnya disimpan di variabel `USER_NAME`.

```bash
if [[ -z "$USER_NAME" ]]; then
    echo "Data yang kamu cari tidak ada."
    exit 1
fi
```
Bagian ini berfungsi untuk mengecek data pengguna ada atau tidak. Berikut adalah penjelasan per bagian :
- `if [[ -z "$USER_NAME" ]]` :
  - `[[ ... ]]` digunakan untuk evaluasi kondisi.
  - `-z "$USER_NAME"` mengecek apakah string dalam variabel `USER_NAME` kosong (tidak ada data).
- `then` menandai awal blok perintah yang akan dijalankan jika kondisi benar.
- `echo "Data yang kamu cari tidak ada."` mencetak pesan error bahwa data tidak ditemukan.
- `exit 1` menghentikan skrip dengan kode status 1 (menandakan adanya error).
- `fi` digunakan untuk menutup blok dari `if`


```bash
echo "Pengguna saat itu adalah $USER_NAME"
```
Bagian ini berungsi untuk mencetak pesan ke terminal yang memberitahu siapa pengguna yang ditemukan, dengan menampilkan nilai dari variabel `USER_NAME`.


```bash
LOG_RESULT=$(grep "$LOG_DATE" "$ACCESS_LOG" | grep "$INPUT_IP")
```
Bagian ini berfungsi mencari baris pada file `access.log` yang mengandung tanggal (dalam format yang telah dikonversi) dan IP Address yang dimasukkan. Berikut adalah penjelasan per bagian :
- `grep "$LOG_DATE" "$ACCESS_LOG"` mencari dan menampilkan baris-baris dalam file `ACCESS_LOG` yang memuat string `LOG_DATE`.
- Pipa `|` mengalirkan output dari perintah `grep` pertama sebagai input untuk perintah `grep` kedua.
- `grep "$INPUT_IP"` menyaring hasil agar hanya baris yang juga mengandung IP Address yang diberikan.
- Hasilnya disimpan di variabel `LOG_RESULT`.

```bash
if [[ -z "$LOG_RESULT" ]]; then
    echo "Tidak ada aktivitas yang ditemukan untuk pengguna ini."
    exit 1
fi
```
Bagian ini berfungsi untuk mengecek apakah variabel LOG_RESULT kosong. Jika kosong maka akan menampilkan bahwa tidak ada aktivitas log yang ditemukan, kemudian keluar dari skrip dengan `exit 1`.

```bash
FORMATTED_LOG=$(echo "$LOG_RESULT" | awk '{
    split($4, waktu, ":");
    tanggal = substr(waktu[1], 2);
    jam = waktu[2];
    menit = waktu[3];
    detik = waktu[4];

    method = substr($6, 2);
    endpoint = $7;
    status = $9;

    printf "[%s:%s:%s:%s]: %s - %s - %s\n", tanggal, jam, menit, detik, method, endpoint, status;
}')
```

Bagian ini berfungsi untuk mengambil hasil log (`LOG_RESULT`), memprosesnya dengan `awk` untuk memformat setiap baris log sesuai format yang diinginkan. Berikut adalah penjelasan lebih lanjut :
- `echo "$LOG_RESULT"` mencetak log yang ditemukan.
- Pipa `|` ke `awk` output dari `echo` dikirim ke `awk` untuk diproses.
- Blok `awk` :
  - `split($4, waktu, ":");` memecah kolom keempat (yang berisi waktu) menjadi array bernama waktu dengan delimiter titik dua (:).
  - `tanggal = substr(waktu[1], 2);` mengambil substring dari elemen pertama array `waktu`. `substr(waktu[1], 2)` menghilangkan karakter pertama (biasanya tanda kurung `[`).
  - `jam = waktu[2]; menit = waktu[3]; detik = waktu[4];` menyimpan komponen waktu ke variabel masing-masing.
  - `method = substr($6, 2);` mengambil HTTP method dari kolom keenam. `substr($6, 2)` menghilangkan karakter pembuka (misalnya tanda kutip atau tanda kurung).
  - `endpoint = $7;` menyimpan nilai endpoint dari kolom ketujuh.
  - `status = $9;` menyimpan status code dari kolom kesembilan.
  - `printf "[%s:%s:%s:%s]: %s - %s - %s\n", ...` Format string untuk mencetak hasil dalam format: `[tanggal:jam:menit:detik]: method - endpoint - status\n` memastikan setiap log berada di baris baru.
- Substitusi `$(...)` Output dari seluruh perintah `awk` disimpan ke variabel `FORMATTED_LOG`.

```bash
mkdir -p "$BACKUP_DIR"
```
Bagian ini berfungsi membuat direktori backup jika belum ada. Berikut penjelasannya :
- `mkdir`: Perintah untuk membuat direktori.
- Opsi `-p`: Membuat parent directories jika belum ada dan tidak mengeluarkan error jika direktori sudah ada.
- `"$BACKUP_DIR"`: Variabel yang berisi path direktori backup; tanda kutip memastikan spasi dalam nama path tertangani dengan benar.


```bash
TIMESTAMP=$(date +"%H%M%S")
```
Bagian ini berfungsi untuk mengambil waktu saat ini dengan format jam, menit, dan detik (misalnya "142536" untuk jam 14:25:36). Berikut adalah penjelasannya :
- `date +"%H%M%S"` perintah date dengan format string untuk menghasilkan waktu dalam format 24-jam.
- Substitusi `$(...)` untuk menyimpan output ke variabel `TIMESTAMP`.

```bash
BACKUP_FILE="$BACKUP_DIR/${USER_NAME}_$(echo $INPUT_DATE | tr -d '/')_${TIMESTAMP}.log"
```

Bagian ini berfungsi untuk menentukan nama lengkap file backup yang akan disimpan. Berikut adalah penjelasan per bagian :
- `$BACKUP_DIR/`: Path direktori backup.
- `${USER_NAME}_`: Nama pengguna diikuti oleh garis bawah.
  - `${USER_NAME}`: Menggunakan `{}` untuk memastikan bash mengakses variabel dengan benar.
- `$(echo $INPUT_DATE | tr -d '/')`: Mengambil input tanggal dan menghapus semua karakter garis miring (/) sehingga format tanggal menjadi misalnya "01032025".
  - `tr -d '/'`: Perintah untuk menghapus (`-d`) karakter `/`.
- `_${TIMESTAMP}`: Menambahkan timestamp ke nama file.
- `.log` adalah ekstensi file log.
- Hasil akhir akan disimpan dengan format nama file seperti `Caca_01022025_213130.log`



```bash
echo "$FORMATTED_LOG" > "$BACKUP_FILE"
```
Bagian ini berfungsi mencetak (menulis) hasil log yang telah diformat ke file backup. Berikut adalah penjelasannya :
- `echo "$FORMATTED_LOG"`: Mencetak output variabel `FORMATTED_LOG`.
- `>` adalah operator redirection yang mengarahkan output ke file.
- `"$BACKUP_FILE"`: Nama file tempat output akan disimpan.

```bash
echo "Log Aktivitas $USER_NAME"
```
Bagian ini berfungsi menampilkan pesan ke layar bahwa log aktivitas untuk pengguna yang bersangkutan telah diproses dan disimpan.


### Foto Hasil Output
![image](https://github.com/user-attachments/assets/c10f2c19-8f8a-4a52-9ab1-39964cf3922e)

### Hasil File yang disimpan dalam direktori backup
![image](https://github.com/user-attachments/assets/4eb8ac90-ebb6-4707-961b-2d96bcd4f281)

### Isi File dari direktori backup
![image](https://github.com/user-attachments/assets/8cc04bba-7730-4238-bfc7-d85eb425ccf2)




c. Code lengkap :
```bash
#!/bin/bash

LOG_FILE="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/access.log"
CSV_FILE="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/peminjaman_computer.csv"

awk '$9 == 500 {print $1}' "$LOG_FILE" | sort | uniq -c > temp_500.log

declare -A error_count


while read jumlah ip; do
    komputer=$(echo "$ip" | awk -F '.' '{print $4}')
    pengguna=$(awk -v komputer="$komputer" -F, '$2 == komputer {print $3}' "$CSV_FILE")
    if [ -n "$pengguna" ]; then
        ((error_count[$pengguna]+=$jumlah)) 
    fi
done < temp_500.log

rm temp_500.log 

pemenang=$(for pengguna in "${!error_count[@]}"; do
    echo "$pengguna ${error_count[$pengguna]}"
done | sort -k2 -nr | head -n 1)

nama=$(echo "$pemenang" | awk '{print $1}')
jumlah=$(echo "$pemenang" | awk '{print $2}')

if [ -n "$nama" ]; then
    echo "Pemenang adalah $nama dengan total $jumlah error 500."
else
    echo "Tidak ada pengguna yang menghasilkan error 500."
fi
```

Penjelasan :
```bash
#!/bin/bash
```
Baris pertama pada kode ini adalah shebang yang berfungsi sebagai tanda yang menunjukkan interpreter apa yang akan digunakan untuk menjalankan script ini. Dalam hal ini, script akan dijalankan menggunakan Bash, sebuah shell di sistem Linux/Unix.

```bash
LOG_FILE="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/access.log"
```
Bagian ini berfungsi sebagai variabel LOG_FILE menyimpan path atau lokasi file log access.log.

```bash
CSV_FILE="/home/aji-zaenul-musthofa/praktikum-modul-1-d10/task-2/peminjaman_computer.csv"
```

Ini juga merupakan deklarasi variabel berfungsi menyimpan path dari file CSV peminjaman_computer.csv yang akan digunakan untuk mencari nama pengguna berdasarkan nomor komputer.

```bash
awk '$9 == 500 {print $1}' "$LOG_FILE" | sort | uniq -c > temp_500.log
```
Bagian ini berfungsi untuk membaca file log dan menampilkan IP address dari baris-baris yang memiliki status code 500 (yang terletak di kolom ke-9). Berikut adalah penjelasan lebih lanjut :
- `awk`: Sebuah perintah yang digunakan untuk memproses dan memfilter teks atau data berdasarkan pola tertentu.
- `''`: Digunakan untuk menyusun pola dan blok perintah yang diberikan kepada awk.
- `$9` merujuk pada kolom ke-9 di setiap baris file log.
- `{print $1}` berarti mencetak nilai dari kolom pertama (yaitu IP address) dari baris yang memenuhi syarat `$9 == 500`.
- `"$LOG_FILE"` digunakan untuk memanggil variabel LOG_FILE, yang berisi path dari file log.

- `| sort` berfungsi mengurutkan IP yang dihasilkan dari perintah awk secara alfabetis.
  - `|`: Pipe. Ini digunakan untuk mengarahkan output dari satu perintah ke perintah lain.
  - `sort`: Perintah untuk mengurutkan output secara alfabetis atau numeris.
- `uniq -c`berfungsi menghapus duplikasi IP dan menghitung jumlah kemunculan masing-masing IP. Opsi `-c` digunakan untuk untuk menampilkan jumlah duplikat di sebelah kiri setiap IP.
- `> temp_500.log` berfungsi untuk mengarahkan output yang dihasilkan oleh perintah sebelumnya ke file `temp_500.log`. Tanda `v` redirection operator ini digunakan untuk menulis output ke sebuah file, menggantikan isinya jika file tersebut sudah ada.

```bash
declare -A error_count
```
Bagian ini mendeklarasikan sebuah array asosiatif bernama `error_count`. Array asosiatif memungkinkan penggunaan string sebagai indeks (kunci). Berikut adalah penjelasan lebih lanjut :
- `declare`: Digunakan untuk mendeklarasikan variabel Bash dengan opsi tambahan.
- `-A`: Opsi untuk mendeklarasikan array asosiatif.
- `error_count`: Nama array asosiatif yang akan digunakan untuk menyimpan jumlah error dari masing-masing pengguna.

```bash
while read jumlah ip; do
    komputer=$(echo "$ip" | awk -F '.' '{print $4}')
    pengguna=$(awk -v komputer="$komputer" -F, '$2 == komputer {print $3}' "$CSV_FILE")
    if [ -n "$pengguna" ]; then
        ((error_count[$pengguna]+=$jumlah)) 
    fi
done < temp_500.log
```
Loop ini membaca file `temp_500.log` dan memecah setiap baris menjadi dua variabel: `jumlah` dan `ip`. Kemudian, berdasarkan IP address, loop mencari nama pengguna komputer di file CSV dan menghitung jumlah error 500 yang ditemukan oleh pengguna tersebut. Berikut penjelasan per bagian :
- `while read jumlah ip; do` membaca setiap baris file temp_500.log.
- `read jumlah ip`:
  - `jumlah`: Menyimpan jumlah status code 500 yang ditemukan oleh IP tersebut.
  - `ip`: Menyimpan alamat IP dari baris yang sedang diproses.

```bash
komputer=$(echo "$ip" | awk -F '.' '{print $4}')
```
Bagian ini berfungsi mengekstrak angka terakhir dari IP address. Berikut penjelasan per bagian:
- `echo "$ip"`: Menampilkan nilai variabel ip.
- `|`: Pipe, untuk mengarahkan output dari echo ke awk.
- `awk -F '.' '{print $4}'`:
  - `-F '.'`: Menentukan pemisah kolom berupa tanda titik (.) pada IP address.
  - `$4`: Mengacu pada kolom ke-4 (angka terakhir dari IP address).
  - `{print $4}`: Mencetak angka keempat dari IP tersebut.


 ```bash
pengguna=$(awk -v komputer="$komputer" -F, '$2 == komputer {print $3}' "$CSV_FILE")
```
Bagian ini berfungsi untuk mencari nama pengguna berdasarkan nomor komputer di file CSV. Berikut adalah penjelasan per bagian:
- `-v komputer="$komputer"`: Membuat variabel `komputer` di dalam `awk` yang nilainya sama dengan nilai variabel Bash `komputer`.
- `-F,`: Menentukan pemisah kolom berupa tanda koma (,) di file CSV.
- `$2 == komputer`: Membandingkan nilai kolom kedua (nomor komputer) dengan variabel `komputer`.
- `{print $3}`: Mencetak kolom ketiga, yaitu nama pengguna komputer.

```bash
    if [ -n "$pengguna" ]; then
        ((error_count[$pengguna]+=$jumlah)) 
    fi
```
Blok ini mengecek apakah variabel pengguna tidak kosong :
- `-n "$pengguna"`: Mengecek apakah string $pengguna tidak kosong.
Jika pengguna ditemukan:
- `((error_count[$pengguna]+=$jumlah))`:
  - Menambahkan jumlah error 500 yang ditemukan oleh pengguna tersebut ke dalam array `error_count`.

Fungsi dari `done < temp_500.log` adalah memberikan input ke loop `while` dari file `temp_500.log`. Dengan kata lain, file `temp_500.log` dibaca baris demi baris oleh loop `while` dan diproses satu per satu.
Tanpa `done < temp_500.log`, loop `while` tidak akan menerima input dan tidak akan melakukan apapun.

```bash
rm temp_500.log
```
Perintah `rm temp_500.log` berfungsi untuk menghapus file log sementara yang telah digunakan dalam loop `while`.

```bash
pemenang=$(for pengguna in "${!error_count[@]}"; do
    echo "$pengguna ${error_count[$pengguna]}"
done | sort -k2 -nr | head -n 1)
```
Bagian kode ini digunakan untuk menentukan pengguna komputer yang menghasilkan jumlah status code 500 terbanyak dan menyimpannya ke dalam variabel `pemenang`. Berikut adalah penjelasan lebih lanjut :
- pemenang=$() Fungsi ini digunakan untuk menyimpan output dari sebuah perintah (dalam hal ini, hasil dari loop dan sorting) ke dalam variabel pemenang. Substitution $() menjalankan semua perintah di dalamnya, kemudian hasil akhirnya disimpan ke variabel pemenang.
- `for loop` digunakan untuk iterasi terhadap elemen dalam array associative `error_count`.
- `${!error_count[@]}`:
  - Bagian ini akan mengakses semua indeks (key) dari array associative `error_count`, yaitu daftar nama pengguna komputer.


```bash
echo "$pengguna ${error_count[$pengguna]}"
```
- Untuk setiap pengguna dalam array, perintah ini akan mencetak nama pengguna dan jumlah status code 500 yang ditemukan oleh pengguna tersebut.
- `${error_count[$pengguna]}`: Mengakses nilai dari array associative `error_count` yang sesuai dengan key `$pengguna`.

```bash
done | sort -k2 -nr
```
- Pipe `|`: Mengarahkan output dari loop `for` (hasil `echo`) ke perintah `sort`.
- `sort`: Perintah ini digunakan untuk mengurutkan hasil output berdasarkan jumlah error.
  - `-k2`: Mengurutkan berdasarkan kolom ke-2 (jumlah error).
  - `-n`: Melakukan sorting numerik.
  - `-r`: Mengurutkan secara descending (dari nilai terbesar ke terkecil).

```bash
| head -n 1
```
`head -n 1` digunakan untuk mengambil baris pertama dari hasil sorting, yaitu pengguna dengan jumlah status code 500 terbanyak.
Setelah seluruh proses selesai, hasilnya akan disimpan ke dalam variabel `pemenang`.

```bash
nama=$(echo "$pemenang" | awk '{print $1}')
jumlah=$(echo "$pemenang" | awk '{print $2}')
```
Bagian ini digunakan untuk memisahkan nama pengguna dan jumlah error 500 dari variabel `pemenang` dan menyimpannya masing-masing ke dalam variabel `nama` dan `jumlah`. Penjelasan detailnya:
- `nama=$( ... )`:
  - Menggunakan Command Substitution untuk menyimpan hasil eksekusi perintah di dalam kurung `$()` ke variabel `nama`.
- `echo "$pemenang"` perintah ini akan mencetak nilai dari variabel pemenang ke terminal.
- `|` (Pipe) digunakan untuk mengalihkan output dari perintah sebelumnya ke perintah berikutnya.

- `awk '{print $1}'` Perintah ini menggunakan`awk`, yaitu tool pemrosesan teks untuk memproses data baris demi baris.
  - `{print $1}` berarti mencetak kolom pertama dari input.
- Hasil dari `awk` kemudian disimpan ke variabel nama.

- `jumlah=$(echo "$pemenang" | awk '{print $2}')` bagian ini sama seperti sebelumnya, menggunakan Command Substitution untuk menyimpan hasil dari perintah di dalam $() ke variabel jumlah.

```bash
if [ -n "$nama" ]; then
    echo "Pemenang adalah $nama dengan total $jumlah error 500."
else
    echo "Tidak ada pengguna yang menghasilkan error 500."
fi
```
Bagian ini adalah sebuah struktur kontrol `if-else` yang digunakan untuk mengecek apakah variabel `nama` memiliki nilai (tidak kosong) dan memberikan output berdasarkan hasil pengecekan tersebut. Berikut penjelasan lebih detail :
- `if [ -n "$nama" ]; then`:
  - `-n` adalah opsi dalam perintah `test` (di dalam `[ ]`) yang berarti "cek apakah string memiliki panjang lebih dari 0".
  - `"$nama"` adalah string yang dicek. Jika `nama` tidak kosong, perintah dalam blok `then` akan dieksekusi.
```bash
echo "Pemenang adalah $nama dengan total $jumlah error 500."
```
Jika pengecekan bernilai true (variabel `nama` tidak kosong), baris ini akan mencetak nama pemenang beserta jumlah error 500 yang dihasilkan.
- `else` bagian ini akan dieksekusi jika `if` menghasilkan false (variabel `nama` kosong).
```bash
echo "Tidak ada pengguna yang menghasilkan error 500."
```
Jika tidak ada pemenang (karena tidak ada pengguna dengan error 500), pesan ini akan dicetak.
- `fi`menandakan akhir dari blok `if-else`.


### Foto Hasil Output
![image](https://github.com/user-attachments/assets/2bcc499d-a6cb-4fbd-9bba-87284f9d6195)
