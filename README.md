

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

- Lengkapi semua bagian sesuai command
- Prepare : buat direktory dengan `mkdir -p` agar tidak eror saat direktori sudah ada. pada dd, if untuk input dan of untuk lokasi yaitu bin/floppy.img. bs sesuai command dan count sesuai jumlah block.
- bootloader : ikuti command pakai nasm -f untuk flat, kemudian yg akan diassemble -o output yang diminta. pada dd, if untuk yg akan ditulis atau input of untuk lokasi yaitu bin/floppy.img. count sesuai block yaitu 1 dan conv sesuai command.
- stdlib : gunakan `bcc -ansi -c` sesuai command, untuk flag isi input -o output sesuai command yaitu `src/std_lib.c -o bin/std_lib.o`.
- kernel : bagian awal mirip stdlib. bagian kedua mirip bootloader dengan format berbeda yaitu as86 sehingga menjadi `nasm -f as86 src/kernel.asm -o bin/kernel_asm.o`.
- link : ikuti command, pakai ld86 -d. flag selanjutnya adalah -o keluaran diikuti semua masukan sehingga menjadi `ld86 -d -o bin/kernel.bin bin/kernel.o bin/kernel_asm.o bin/std_lib.o`. gunakan dd sesuai command dengan seek=1 conv=notrunc.
- run : jalankan `bochs -q -f bochs.txt` -q untuk quiet mode.


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
# =========================================================
# Task E — Complete every target below.
# All compiled outputs must go into the bin/ directory.
# =========================================================

prepare:
	# TODO: Create the bin/ directory if it does not exist.
	mkdir -p bin
	# TODO: Generate a blank 1.44 MB floppy image at bin/floppy.img.
	#       Use dd: input /dev/zero, 512-byte blocks, 2880 blocks total.
	dd if=/dev/zero of=bin/floppy.img bs=512 count=2880

bootloader:
	# TODO: Assemble src/bootloader.asm → bin/bootloader.bin  (nasm, flat binary format).
	nasm -f bin src/bootloader.asm -o bin/bootloader.bin
	# TODO: Write bin/bootloader.bin to sector 0 of bin/floppy.img
	#       (dd: 1 block, conv=notrunc so the rest of the image is preserved).
	dd if=bin/bootloader.bin of=bin/floppy.img bs=512 count=1 conv=notrunc

stdlib:
	# TODO: Compile src/std_lib.c → bin/std_lib.o  (bcc, -ansi -c flags).
	bcc -ansi -c src/std_lib.c -o bin/std_lib.o

kernel:
	# TODO: Compile src/kernel.c   → bin/kernel.o     (bcc, -ansi -c flags).
	bcc -ansi -c src/kernel.c -o bin/kernel.o
	# TODO: Assemble src/kernel.asm → bin/kernel_asm.o (nasm, as86 format).
	nasm -f as86 src/kernel.asm -o bin/kernel_asm.o

link:
	# TODO: Link bin/kernel.o + bin/kernel_asm.o + bin/std_lib.o → bin/kernel.bin
	#       (ld86 with -d flag for no-standard-library linking).
	ld86 -d -o bin/kernel.bin bin/kernel.o bin/kernel_asm.o bin/std_lib.o
	# TODO: Write bin/kernel.bin to bin/floppy.img starting at sector 1
	#       (dd: seek=1, conv=notrunc).
	dd if=bin/kernel.bin of=bin/floppy.img bs=512 seek=1 conv=notrunc

build: prepare bootloader stdlib kernel link

run:
	# TODO: Launch Bochs using bochsrc.txt in quiet mode (-q flag).
	bochs -q -f bochsrc.txt
```
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
