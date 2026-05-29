

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

- printString
  	- fungsi ini akan menerima array of char, sehingga diperlukan variabel untuk index. Disini saya menggunakan i = 0 untuk memulai dari index paling awal.
  	- lakukan perulangan sampai menemukan '\0' pada array.
  	- dalam perulangan lakukan print ke layar dan increment i.
  	- print dilakukan dengan `interrupt(0x10, ax, 0, 0, 0)` dimana 0x10 adalah untuk terhubung ke display dan ax berisi `int ax = (0x0E << 8) | (str[i] & 0xFF)`. bagian depan ax adalah AH dan bagian belakang untuk mendapat AL (yang mau didisplay) perlu bitwise char dengan 0xFF.

- readString
  	- fungsi ini akan menerima array of char (buf), sehingga diperlukan variabel untuk index. Disini saya menggunakan i = 0 untuk memulai dari index paling awal.
  	- pakai perulangan `while(1)` agar terus loop sampai ada break.
  	- di dalam loop, gunakan percabangan untuk cek beberapa hal penting yaitu enter dan backspace.
  	- Ambil input dari keyboard dengan interrupt `int ax = interrupt(0x16, 0x0000, 0, 0, 0)` simpan ke ax. Ambil inputan (AL) dengan membitwise ax dengan 0xFF simpan ke variabel (disini pakai c).
  	- Lakukan percabangan untuk cek c, jika c=='\r' ubah buf[i] menjadi '\0' untuk merubahnya menjadi tanda akhir dari string. Kemudian pakai `interrupt(0x10, (0x0E << 8) | '\r', 0, 0, 0)` untuk menggeser kursor ke kiri layar. 
  	- Jika c=='\b' maka geser kursor ke kiri dengan `interrupt(0x10, (0x0E << 8) | '\b', 0, 0, 0)` displaykan spasi dengan `interrupt(0x10, (0x0E << 8) | ' ', 0, 0, 0)` untuk menimpa huruf sebelumnya, kemudian geser kursor ke kiri lagi.
  	- jika c bukan enter atau backspace maka ubah buf[i] menjadi c untuk menyimpan input, increment i, kemudian displaykan input dengan `interrupt(0x10, (0x0E << 8) | c, 0, 0, 0)`

- clearScreen
  	- Bersihkan layar pakai `interrupt(0x10, (0x06 << 8) | 0, 0x0700, 0, (24 << 8) | 79);` AX sesuai AH dan AL pada soal, BX sesuai BH CX 0 karena CH dan CL 0, DX sesuai DH dan DL.
  	- Pindahkan kursor dengan AH = 0x02 makan jadi `interrupt(0x10, (0x02 << 8) | 0, 0, 0, 0);`
  	- lakukan loop sebanyak 80x25, setiap loop melakukan
  	  ```
  		putInMemory(0xB800, i * 2, ' ');
		putInMemory(0xB800, i * 2 + 1, 0x07);
  	  ```
  	  yaitu mengisi spasi pada memory genap dan mengisi atribut pada posisi ganjil.

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
#include "std_lib.h"
#include "kernel.h"

void parseSegment(char* seg, char* cmd, char* args);
void handleEcho(char* args, char* output);
void handleGrep(char* pattern, char* input, char* output);
void handleWc(char* input, char* output);
void handleCommand(char* buf);
void intToStr(int n, char* str);

void printString(char* str) {
	int i = 0;
	while (str[i] != '\0') {
		int ax = (0x0E << 8) | (str[i] & 0xFF);
		interrupt(0x10, ax, 0, 0, 0);
		i++;
	}
}

void readString(char* buf) {
	int i = 0;
	while (1) {
		int ax = interrupt(0x16, 0x0000, 0, 0, 0);
		char c = ax & 0xFF;
		if (c == '\r') {
			buf[i] = '\0';
			interrupt(0x10, (0x0E << 8) | '\r', 0, 0, 0);
			break;
		}
		else if (c == '\b') {
			if (i > 0) {
				i--;
				interrupt(0x10, (0x0E << 8) | '\b', 0, 0, 0);
				interrupt(0x10, (0x0E << 8) | ' ', 0, 0, 0);
				interrupt(0x10, (0x0E << 8) | '\b', 0, 0, 0);
			}
		}
		else {
			buf[i] = c;
			i++;
			interrupt(0x10, (0x0E << 8) | c, 0, 0, 0);
		}
	}
}

void clearScreen() {
	interrupt(0x10, (0x06 << 8) | 0, 0x0700, 0, (24 << 8) | 79);
	interrupt(0x10, (0x02 << 8) | 0, 0, 0, 0);
	for (int i = 0; i < 2000; i++) {
		putInMemory(0xB800, i * 2, ' ');
		putInMemory(0xB800, i * 2 + 1, 0x07);
	}
}

void intToStr(int n, char* str) {
    int i = 0;
    char tmp[12];
    if (n == 0) {
        str[0] = '0';
        str[1] = '\0';
        return;
    }
    while (n > 0) {
        tmp[i++] = '0' + mod(n, 10);
        n = div(n, 10);
    }
    int j;
    for (j = 0; j < i; j++) {
        str[j] = tmp[i - 1 - j];
    }
    str[i] = '\0';
}

void parseSegment(char* seg, char* cmd, char* args) {
    int i = 0, j = 0;
    while (seg[i] == ' ') i++;
    while (seg[i] != ' ' && seg[i] != '\0') cmd[j++] = seg[i++];
    cmd[j] = '\0';
    j = 0;
    while (seg[i] == ' ') i++;
    while (seg[i] != '\0') args[j++] = seg[i++];
    while (j > 0 && args[j - 1] == ' ') j--;
    args[j] = '\0';
}



void handleEcho(char* args, char* output) {
     /* Insert Function Here */
}

void handleGrep(char* pattern, char* input, char* output) {
     /* Insert Function Here */
}


void handleWc(char* input, char* output) {
     /* Insert Function Here */
}

void handleCommand(char* buf) {
    char segments[3][128];
    int segCount = 0;
    int i = 0, j = 0, seg = 0;
    char cmd[32], args[128], pipeIn[128], pipeOut[128];

    clear((byte*)segments[0], 128);
    clear((byte*)segments[1], 128);
    clear((byte*)segments[2], 128);
    clear((byte*)pipeIn, 128);
    clear((byte*)pipeOut, 128);

    while (buf[i] != '\0' && seg < 3) {
        if (buf[i] == '|') {
            segments[seg][j] = '\0';
            seg++;
            j = 0;
        } else {
            segments[seg][j++] = buf[i];
        }
        i++;
    }
    segments[seg][j] = '\0';
    segCount = seg + 1;

    for (i = 0; i < segCount; i++) {
        clear((byte*)cmd, 32);
        clear((byte*)args, 128);
        clear((byte*)pipeOut, 128);

        parseSegment(segments[i], cmd, args);

        if (strcmp(cmd, "echo")) {
            handleEcho(args, pipeOut);
        } else if (strcmp(cmd, "grep")) {
            handleGrep(args, pipeIn, pipeOut);
        } else if (strcmp(cmd, "wc")) {
            handleWc(pipeIn, pipeOut);
        } else {
            printString("Unknown command: ");
            printString(cmd);
            printString("\n");
            return;
        }

        strcpy(pipeOut, pipeIn);
    }

    if (strlen(pipeIn) > 0) {
        printString(pipeIn);
        printString("\n");
    } else {
        printString("NULL\n");
    }
}

int main() {
    char buf[128];

    clearScreen();
    printString("SumbulOS - [[PUT YOUR TEAM CODE HERE]]\n");

    while (true) {
        printString("$> ");
        readString(buf);
        printString("\n");

        if (strlen(buf) > 0) {
		/* InsrtFunction Here */
		handleCommand(buf);
        }
    }
}
```

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- div
  	- buat variabel untuk simpan nilai absolut dari a dan b
  	- pembagian adalah pengurangan berulang, buat variabel untuk simpan jumlah perulangan yaitu d.
  	- pada pembagian berlaku akan menjadi mines jika kedua tanda bilangan berbeda, buat variabel untuk tanda.
  	- cegah eror ketika pembagian dengan 0, cek b dengan if, jika 0 maka langsung return 0.
  	- cek tanda dengan 2 percaangan. 1 jika a min dan b positif tanda jadi min (-1). 2 jika a positif dan b min tanda jadi min juga. jika tidak maka tanda = 1 (saat deklarasi tanda = 1).
  	- simpan nilai absolut a dan b dengan percabangan. Jika a positif maka absolutnya -a, jika tidak maka absolutnya b. Lakukan juga untuk absolut b.
  	- lakukan pengurangan berulang yang berhenti ketika absolut a kurang dari absolut b. Dalam setiap perulangan increment d untuk menghitung jumlah loop.
  	- keluarkan nilai -d jika tanda = -1, dan d jika tanda = 1.
- mod
  	- mod adalah mencari sisa bagi, maka perlu hasil pembagian dan sisa. buat 2 variabel itu.
  	- isi hasil bagi dengan memakai fungsi div.
  	- sisa adalah bilangan yang dibagi dikurang pembagi kali hasil bagi. Maka `sisa = a - (hasil_bagi * b);`.
  	- keluarkan hasil dengan `return sisa`;
- memcpy
  	- lakukan loop sebanyak size dengan `for (i = 0; i < size; i++)`.
  	- Setiap loop akan mengisi destinasi dengan nilai yang sama dengan sumber dengan `dst[i] = src[i]`
- strlen
  	- strlen menghitung panjang dengan menghitung jumlah char sampai bertemu '\o'. sehingga panjang adalah jumlah index.
  	- buat variabel panjang = 0;
  	- lakukan loop sampai menemukan '\0' dengan `while (str[panjang] != '\0')`.
  	- setiap loop akan incremen panjang untuk ke index selanjutnya. setelah loop selesai, return panjang.
- strcmp
  	- fungsi ini membandingkan setiap index yang sama anatara 2 string.
  	- Buat variabel untuk index.
  	- Lakukan loop selama index ke i dari kedua string sama `while (str1[i] == str2[i])`.
  	- dalam loop, cek Jika index ke i berisi '\0'. jika iya maka return true. jika tidak lanjutkan loop.
  	- jika loop selesai berarti ada index yang tidak saman maka return false.
- strcpy
  	- fungsi ini menyalin isi src ke destinasi (dst).
  	- Buat variabel untuk index yaitu i.
  	- Lakukan loop sampai bertemu '\0' dengan `while (src[i] != '\0')` karena jika bertemu null berarti string sudah selesai.
  	- Setiap loop lakukan `dst[i] = src[i]`. Kemudian incremen i untuk pindah ke index selanjutnya.
  	- Setelah keluar dari loop, tambahkan '\0' pada akhir dst (dst[i]) karena loop berhenti sebelum menyalin '\0'.
- clear
  	- Fungsi ini mengisi memory dengan 0.
  	- Lakukan loop sebanyak size dengan `for (i = 0; i < size; i++)`.
  	- Setiap loop mengisi buf index ke i dengan 0 `buf[i] = 0;`.

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
#include "std_lib.h"


int div(int a, int b) {
	unsigned int ab_a = 0;
	unsigned int ab_b = 0;
	unsigned int d = 0;
	int tanda = 1;

	if (b == 0) {
		return 0;
	}
	if (a < 0 && b > 0) {
		tanda = -1;
	}
	if (a > 0 && b < 0) {
		tanda = -1;
	}

	if (a < 0) {
		ab_a = -a;
	}
	else {
		ab_a = a;
	}
	if (b < 0) {
		ab_b = -b;
	}
	else {
		ab_b = b;
	}

	while (ab_a >= ab_b) {
		ab_a = ab_a - ab_b;
		q + q + 1;
	}
	if (tanda = -1) {
		return -(int)d;
	}
	else {
		return (int)d;
	}
}

int mod(int a, int b) {
	int hasil_bagi = 0;
	int sisa = 0;
	if (b == 0) {
		return 0;
	}
	hasil_bagi = div(a,b);
	sisa = a - (hasil_bagi * b);
	return sisa;
}

void memcpy(byte* src, byte* dst, unsigned int size) {
	unsigned int i = 0;
	for (i = 0; i < size; i++){
		dst[i] = src[i];
	}
}

unsigned int strlen(char* str) {
	unsigned int panjang = 0;
	while (str[panjang] != '\0') {
		panjang = panjang + 1;
	}
	return panjang;
}

bool strcmp(char* str1, char* str2) {
	int i = 0;
	while (str1[i] == str2[i]) {
		if (str1[i] == '\0') {
			return true;
		}
		i = i + 1;
	}
	return false;
}

void strcpy(char* src, char* dst) {
	int i = 0;
	while (src[i] != '\0') {
		dst[i] = src[i];
		i = i + 1;
	}
	dst[i] = '\0';
}

void clear(byte* buf, unsigned int size) {
	unsigned int i = 0;
	for (i = 0; i < size; i++) {
		buf[i] = 0;
	}
}
```

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- handleEcho
  	- Command echo akan mencetak ulang apa yang diinputkan.
  	- Maka copy input ke output menggunakan strcpy yang sudah dibuat pada soal sebelumnya
- handleGrep
  	- Command grep akan melakukan cetak ulang jika pattern yang diminta ada di inputan.
  	- Fungsi ini akan menerima input, pattern, dan output.
  	- Buat variabel untuk indexing (i dan j) dan untuk cek apakah pattern ditemukan (bool ketemu).
  	- Lakukan perulangan (loop besar) sampai input habis atau pattern ditemukan dengan `while (input[i] != '\0' && ketemu == false)`. kalau salah satu tidak terpenuhi loop akan berhenti. Kemudian dalam loop lakukan :
  	- Index i digunakan untuk awal dari input, dan j untuk indexing dari i sampai akhir pattern dan untuk indexing pattern.
  	- cek dari awal dengan gunakan loop ketika input ke i+j == pattern ke j dan keduanya bukan null. Jika sesuai maka geser j (j++). Jika tidak sesuai maka keluar dari loop dan geser i(i++) ubah j menjadi 0 lagi sebelum masuk ke loop ini lagi (diubah di atas while).
  	- Jika pattern ke j == null artinya loop selesai karena pattern ditemukan, maka ubah ketemu menjadi true.
  	- Setelah keluar dari loop besar, gunakan percabangan untuj jika pattern ditemukan dan tidak. jika ketemu == true maka salin input ke output dengan strpy. Jika tidak maka output = null.

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```
#include "std_lib.h"
#include "kernel.h"

void parseSegment(char* seg, char* cmd, char* args);
void handleEcho(char* args, char* output);
void handleGrep(char* pattern, char* input, char* output);
void handleWc(char* input, char* output);
void handleCommand(char* buf);
void intToStr(int n, char* str);

void printString(char* str) {
	int i = 0;
	while (str[i] != '\0') {
		int ax = (0x0E << 8) | (str[i] & 0xFF);
		interrupt(0x10, ax, 0, 0, 0);
		i++;
	}
}

void readString(char* buf) {
	int i = 0;
	while (1) {
		int ax = interrupt(0x16, 0x0000, 0, 0, 0);
		char c = ax & 0xFF;
		if (c == '\r') {
			buf[i] = '\0';
			interrupt(0x10, (0x0E << 8) | '\r', 0, 0, 0);
			break;
		}
		else if (c == '\b') {
			if (i > 0) {
				i--;
				interrupt(0x10, (0x0E << 8) | '\b', 0, 0, 0);
				interrupt(0x10, (0x0E << 8) | ' ', 0, 0, 0);
				interrupt(0x10, (0x0E << 8) | '\b', 0, 0, 0);
			}
		}
		else {
			buf[i] = c;
			i++;
			interrupt(0x10, (0x0E << 8) | c, 0, 0, 0);
		}
	}
}

void clearScreen() {
	interrupt(0x10, (0x06 << 8) | 0, 0x0700, 0, (24 << 8) | 79);
	interrupt(0x10, (0x02 << 8) | 0, 0, 0, 0);
	for (int i = 0; i < 2000; i++) {
		putInMemory(0xB800, i * 2, ' ');
		putInMemory(0xB800, i * 2 + 1, 0x07);
	}
}

void intToStr(int n, char* str) {
    int i = 0;
    char tmp[12];
    if (n == 0) {
        str[0] = '0';
        str[1] = '\0';
        return;
    }
    while (n > 0) {
        tmp[i++] = '0' + mod(n, 10);
        n = div(n, 10);
    }
    int j;
    for (j = 0; j < i; j++) {
        str[j] = tmp[i - 1 - j];
    }
    str[i] = '\0';
}

void parseSegment(char* seg, char* cmd, char* args) {
    int i = 0, j = 0;
    while (seg[i] == ' ') i++;
    while (seg[i] != ' ' && seg[i] != '\0') cmd[j++] = seg[i++];
    cmd[j] = '\0';
    j = 0;
    while (seg[i] == ' ') i++;
    while (seg[i] != '\0') args[j++] = seg[i++];
    while (j > 0 && args[j - 1] == ' ') j--;
    args[j] = '\0';
}



void handleEcho(char* args, char* output) {
	strcpy(args, output);
}

void handleGrep(char* pattern, char* input, char* output) {
	int i = 0;
	int j = 0;
	bool ketemu = false;
	while (input[i] != '\0' && ketemu == false){
		j = 0;
		while (input[i + j] == pattern[j] && pattern[j] != '\0' && input[i + j] != '\0') {
			j = j + 1;
		}
		if (pattern[j] == '\0') {
			ketemu = true;
		}
	i = i + 1;
	}
	if (ketemu == true) {
		strcpy(input, output);
	}
	else {
		output[0] = '\0';
	}
}


void handleWc(char* input, char* output) {
     /* Insert Function Here */
}

void handleCommand(char* buf) {
    char segments[3][128];
    int segCount = 0;
    int i = 0, j = 0, seg = 0;
    char cmd[32], args[128], pipeIn[128], pipeOut[128];

    clear((byte*)segments[0], 128);
    clear((byte*)segments[1], 128);
    clear((byte*)segments[2], 128);
    clear((byte*)pipeIn, 128);
    clear((byte*)pipeOut, 128);

    while (buf[i] != '\0' && seg < 3) {
        if (buf[i] == '|') {
            segments[seg][j] = '\0';
            seg++;
            j = 0;
        } else {
            segments[seg][j++] = buf[i];
        }
        i++;
    }
    segments[seg][j] = '\0';
    segCount = seg + 1;

    for (i = 0; i < segCount; i++) {
        clear((byte*)cmd, 32);
        clear((byte*)args, 128);
        clear((byte*)pipeOut, 128);

        parseSegment(segments[i], cmd, args);

        if (strcmp(cmd, "echo")) {
            handleEcho(args, pipeOut);
        } else if (strcmp(cmd, "grep")) {
            handleGrep(args, pipeIn, pipeOut);
        } else if (strcmp(cmd, "wc")) {
            handleWc(pipeIn, pipeOut);
        } else {
            printString("Unknown command: ");
            printString(cmd);
            printString("\n");
            return;
        }

        strcpy(pipeOut, pipeIn);
    }

    if (strlen(pipeIn) > 0) {
        printString(pipeIn);
        printString("\n");
    } else {
        printString("NULL\n");
    }
}

int main() {
    char buf[128];

    clearScreen();
    printString("SumbulOS - [[PUT YOUR TEAM CODE HERE]]\n");

    while (true) {
        printString("$> ");
        readString(buf);
        printString("\n");

        if (strlen(buf) > 0) {
		/* InsrtFunction Here */
		handleCommand(buf);
        }
    }
}
```

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
