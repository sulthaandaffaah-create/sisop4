

|    NRP     |           Nama             |
| :--------: |       :------------:       |
| xxxxxxxxxx | xxxxxxx                    |
| 5025251103 | Rafifah Nabil Rahmadian    |
| xxxxxxxxxx | xxxxxxx                    |

# Praktikum Modul 4 _(Module 4 Lab Work)_

</div>

### Daftar Soal _(Task List)_

- [Task 1 - MirrorFS](/task-1/)

- [Task 2 - The Plagiarist's Trap](/task-2/)

- [Task 3 - SumbulOS — The Piping Gauntlet](/task-3/)

---

### Laporan Resmi Praktikum Modul 4 _(Module 4 Lab Work Report)_

Tulis laporan di template berikut!

_Write your lab work on the template here!_

---

# Task 1 - MirrorFS

## A. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

---

# Task 2 - The Plagiarist's Trap

## A. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Masuk ke mode superuser untuk menambah user
2. Gunakan ``` useradd -m -s /bin/bash CodeCopier``` untuk menambah user CodeCopier
3. Gunakan ```passwd CodeCopier``` untuk menambah password
4. Masukkan password
5. Ulangi hal yg sama untuk user lainnya
- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="418" height="198" alt="Screenshot 2026-05-28 at 11 39 01" src="https://github.com/user-attachments/assets/d867b26c-e757-47cd-8a33-d731904b0f89" />


- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
ubuntu@ubuntu:~/sisop-modul-4-a03/task-2$ sudo su
[sudo] password for ubuntu: 
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# useradd -m -s /bin/bash CodeCopier
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# passwd CodeCopier
New password: 
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary word
Retype new password: 
passwd: password updated successfully
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# useradd -m -s /bin/bash AlgoAnna
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# passwd AlgoAnna
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# useradd -m -s /bin/bash BinaryBen
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# passwd BinaryBen
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# grep -E "CodeCopier|AlgoAnna|BinaryBen" /etc/passwd
CodeCopier:x:1001:1001::/home/CodeCopier:/bin/bash
AlgoAnna:x:1002:1002::/home/AlgoAnna:/bin/bash
BinaryBen:x:1003:1003::/home/BinaryBen:/bin/bash
root@ubuntu:/home/ubuntu/sisop-modul-4-a03/task-2# exit
exit
```

- 

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Perbaiki kode troll_skeleton.c pada fungsi ```static int troll_readdir```
2. Tambahkan ```filler(buf, "submit.txt", NULL, 0);``` supaya file submit.txt terlihat
3. Compile ulang program dengan ```gcc -Wall troll_skeleton.c -o troll `pkg-config fuse --cflags --libs` ```
4. Buat direktori mount point menggunakan ```sudo mkdir -p /mnt/honeypot```
5. Jalankan fuse filesystem dengan ```sudo ./troll /mnt/honeypot``
6. Cek keberadaan file submit.txt dengan ls -la /mnt/honeypot

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="607" height="139" alt="Screenshot 2026-05-28 at 11 47 56" src="https://github.com/user-attachments/assets/cafb70fb-cab0-4942-972c-7b1f52982626" />


- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
static int troll_readdir(const char *path, void *buf, fuse_fill_dir_t filler,
                         off_t offset, struct fuse_file_info *fi) {
    if (strcmp(path, "/") != 0)
        return -ENOENT;

    filler(buf, ".",  NULL, 0);
    filler(buf, "..", NULL, 0);

    /* BUG (PART B): something is missing here. */
    filler(buf, "secret_solution.txt", NULL, 0);
filler(buf, "submit.txt", NULL, 0); /*fixed part B*/
    return 0;
}
```

- 

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Modifikasi fungsi troll_read
2. Cek apakah file yang diakses dan user yang mengakses sudah sesuai
3. Jika user sesuai (CodeCopier), maka output yang dikeluarkan adalah ```Algorithm walkthrough: Use dynamic programming with memoization on overlapping subproblems```
4. Jika tidak sesuai (user lain yang mengakses) maka outputnya adalah ```CodeCopier's plagiarized assignments archive!!.txt```
5. Compile ulang program

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="631" height="177" alt="Screenshot 2026-05-28 at 12 22 13" src="https://github.com/user-attachments/assets/73304eac-a792-49ea-b97b-be604e89ba73" />


- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
static int troll_read(const char *path, char *buf, size_t size,
                      off_t offset, struct fuse_file_info *fi) {
        struct fuse_context *ctx = fuse_get_context();
    uid_t uid = ctx->uid;

    struct passwd *pw = getpwuid(uid);
    const char *user = (pw && pw->pw_name) ? pw->pw_name : "unknown";

const char *data = "";

    /* TODO: PART E - log this READ. */

    //const char *data = "";

    /* TODO: PART D - Post-trigger behavior goes here. */
if (trap_triggered){ 
data = "ACCESS DENIED: Plagiarism detected! Your activity has been logged.\n";
}
    /* TODO: PART C - Pre-trigger content for secret_solution.txt. */
if(strcmp(path, file1) == 0) {
if(strcmp(user, target_user) == 0){
data = "Algorithm walkthrough: Use dynamic programming with memoization on overlapping subproblems>
} else {
data = "CodeCopier's plagiarized assignments archive!!.txt\n";
}
}
    size_t len = strlen(data);
    if (offset < (off_t)len) {
        if (offset + size > len) size = len - offset;
        memcpy(buf, data + offset, size);
    } else {
        size = 0;
    }
    return size;
}

```

- 

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Tambahkan library ```<syslog.h>```
2. Tambahkan variabel trap_triggered untuk memberi tanda apakah jebakan sudah aktif
3. Set nilainya menjadi 0
4. ```if (strcmp(path, file2) == 0)``` untuk mengecek apakah file yang diakses adalah submit.txt
5. ```if (strcmp(user, target_user) == 0)``` mengecek user, jika user merupakan CodeCopier maka baris if-else akan dijalankan
6. Set nilai variabel trap_tiggered menjadi 1
7. Buat file yang berisi peringatan bahwa jebakan sudah aktif (sesuai dengan ```static const char *flag_path   = "/var/tmp/honeypot_trigger.flag"``` )
8. Tambahkan post-trigger behaviour sesuai petunjuk pada file
9. Jika trap_triggered bernilai 1, maka output ascii art akan dicetak
10. Compile ulang program

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="478" height="131" alt="Screenshot 2026-05-29 at 16 23 09" src="https://github.com/user-attachments/assets/2f3cfdc8-9379-4adf-9869-91e376fdcbd5" />


- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
troll_read:
/* TODO: PART D - Post-trigger behavior goes here. */
    /* TODO: PART C - Pre-trigger content for secret_solution.txt. */
if(trap_triggered){
data = ascii_art;
}else if(strcmp(path, file1) == 0) {
if(strcmp(user, target_user) == 0){
data = "Algorithm walkthrough: Use dynamic programming with memoization on overlapping subproblems\n";
} else {
data = "CodeCopier's plagiarized assignments archive!!.txt\n";
}
}

troll_write:
if (strcmp(path, file2) == 0) {
if (strcmp(user, target_user) == 0){
trap_triggered = 1;

syslog(LOG_WARNING, "%s", ascii_art);

FILE *flag = fopen(flag_path, "w");
if(flag){
fprintf(flag, "Triggered by %s at %ld\n", user, time(NULL));
fclose(flag);
}
}
```

- 

## E. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Modifikasi fungsi log_operation
2. Buka file honeypot_trigger.flag dengan menggunakan variabel log_path
3. Gunakan mode append supaya catatan log sebelumnya tidak hilang
4. Buat variabel now dengan tipe data time_t untuk mencatat waktu akses
5. Cetak sesuai format yang diinginkan
6. Panggil fungsi log_operation pada fungsi troll_read dan troll_write

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
static void log_operation(const char *username,
                          const char *operation,
                          const char *filename) {
FILE *log = fopen(log_path, "a");
if(log){ 
time_t now = time(NULL);
struct tm *tm = localtime(&now);
fprintf(log, "[%04d-%02d-%02d %02d:%02d:%02d] %s %s %s\n",
                tm->tm_year+1900, tm->tm_mon+1, tm->tm_mday,
                tm->tm_hour, tm->tm_min, tm->tm_sec,
                username, operation, filename);
fclose(log);
}
}
```

- 

---

# Task 3 - SumbulOS — The Piping Gauntlet

## A. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## E. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

---

### Revisi Praktikum Modul 4 _(Module 4 Lab Work Revision)_

Tuliskan hanya bagian yang direvisi pada template berikut.  
_Only write the revised sections using the template below._

Gunakan format yang sama seperti pada laporan utama.  
_Use the same format as in the main report._

---

# Task [] - []

## []. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

...
