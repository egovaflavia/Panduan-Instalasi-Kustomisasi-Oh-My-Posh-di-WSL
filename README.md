# Panduan Instalasi & Kustomisasi Oh My Posh di WSL

Dokumentasi pribadi untuk menginstal, mengonfigurasi Nerd Font, hingga membuat tema kustom **Oh My Posh** pada lingkungan Windows Subsystem for Linux (WSL).

---

## 1. Instalasi Prasyarat di WSL

Pastikan paket `curl` dan `unzip` sudah terpasang di distribusi Linux WSL (contoh: Ubuntu) sebelum menjalankan skrip instalasi.

```bash
sudo apt update && sudo apt install curl unzip -y
```

---

## 2. Instalasi Oh My Posh

Unduh dan jalankan skrip instalasi resmi untuk Linux:

```bash
curl -s https://ohmyposh.dev | bash -s
```

Jika perintah `oh-my-posh` belum dikenali oleh terminal, pastikan direktori `~/.local/bin` sudah terdaftar di dalam variabel `PATH` pada file `~/.bashrc`:

```bash
export PATH=\$PATH:~/.local/bin
```

---

## 3. Pengaturan Nerd Font di Windows Terminal

Agar semua ikon dan simbol (*glyph*) tampil sempurna dan tidak berubah menjadi kotak-kotak, pasang **Nerd Font** pada sistem operasi **Windows** (bukan di dalam WSL):

1. Unduh font pilihan (contoh: *MesloLGM NF* atau *FiraCode Nerd Font*) dari [Nerd Fonts Downloads](https://nerdfonts.com).
2. Ekstrak file `.zip`, blok semua file font, klik kanan, lalu pilih **Install**.
3. Buka **Windows Terminal** dan tekan `Ctrl + ,` untuk membuka **Settings**.
4. Pilih profil WSL/Ubuntu pada panel sebelah kiri, lalu buka tab **Appearance**.
5. Ubah opsi **Font face** ke Nerd Font yang baru saja diinstal, lalu klik **Save**.

---

## 4. Manajemen Tema

### A. Melihat Pratinjau Tema Bawaan di WSL
Perintah `oh-my-posh get themes` hanya tersedia di Windows PowerShell. Untuk melihat pratinjau semua tema langsung di terminal WSL, unduh paket tema resmi terlebih dahulu:

```bash
# Unduh dan ekstrak paket tema
mkdir -p ~/.poshthemes && curl -sL https://github.com -o ~/.poshthemes/themes.zip && unzip -q ~/.poshthemes/themes.zip -d ~/.poshthemes && rm ~/.poshthemes/themes.zip

# Tampilkan pratinjau seluruh tema di layar
for file in ~/.poshthemes/*.omp.json; do echo -e "\n\033[1;32mTema: (basename "file" .omp.json)\033[0m"; oh-my-posh print primary --config "\$file"; done
```

*(Alternatif: Lihat galeri visual tema secara online di [Oh My Posh Themes](https://ohmyposh.dev)).*

### B. Mengambil Tema Kustom dari Windows ke WSL
1. Cek lokasi tema yang aktif di **PowerShell Windows**:
   ```powershell
   notepad \$PROFILE
   ```
   *(Cari alamat file `.omp.json` di dalam parameter `--config`).*
2. Salin file dari drive `C:\` Windows ke direktori home WSL:
   ```bash
   cp /mnt/c/Users/<NAMA_USER_WINDOWS>/<LOKASI_FILE>/tema.omp.json ~/.mytheme.omp.json
   ```

---

## 5. Membuat & Mengedit Tema Kustom

Buat atau buka file konfigurasi tema menggunakan editor teks (`nano` atau `code`):

```bash
nano ~/.mytheme.omp.json
```

### Contoh Konfigurasi Tema Minimalis (Tiga Panah)
Konfigurasi berikut menggunakan skema **Oh My Posh v4** untuk menampilkan prompt tiga panah minimalis dengan warna merah, oranye, dan hijau:

```json
{
  "\$schema": "https://githubusercontent.com",
  "blocks": [
    {
      "type": "prompt",
      "alignment": "left",
      "segments": [
        {
          "type": "text",
          "style": "plain",
          "foreground": "#ffffff",
          "template": "<#FF5555>❯</><#FFB86C>❯</><#50FA7B>❯</> "
        }
      ]
    }
  ],
  "version": 4
}
```

### Contoh Modifikasi: Menambahkan Nama Folder & Status Git
Jika ingin menampilkan nama direktori aktif dan *branch* Git sebelum tiga panah:

```json
{
  "\$schema": "https://githubusercontent.com",
  "blocks": [
    {
      "type": "prompt",
      "alignment": "left",
      "segments": [
        {
          "type": "path",
          "style": "plain",
          "foreground": "#8BE9FD",
          "options": {
            "style": "folder"
          },
          "template": "{{ .Path }} "
        },
        {
          "type": "git",
          "style": "plain",
          "foreground": "#BD93F9",
          "template": "on  {{ .HEAD }} "
        },
        {
          "type": "text",
          "style": "plain",
          "foreground": "#ffffff",
          "template": "<#FF5555>❯</><#FFB86C>❯</><#50FA7B>❯</> "
        }
      ]
    }
  ],
  "version": 4
}
```

> **Catatan Kustomisasi:**
> - Cari kode ikon tambahan melalui [Nerd Fonts Cheat Sheet](https://nerdfonts.com).
> - Ubah warna teks menggunakan format `<#HEX_COLOR>teks</>` di dalam properti `template`.
> - Setiap perubahan pada file `.omp.json` akan langsung terlihat setelah menekan `Enter` di terminal.

---

## 6. Aktivasi Permanen di Bash (`~/.bashrc`)

Tambahkan perintah inisialisasi berikut di baris paling bawah file `~/.bashrc`:

```bash
nano ~/.bashrc
```

Tempelkan konfigurasi berikut:

```bash
export PATH=\$PATH:~/.local/bin
eval "\$(oh-my-posh init bash --config ~/.mytheme.omp.json)"
```

Simpan file (`Ctrl + O`, `Enter`, `Ctrl + X`), lalu terapkan perubahan:

```bash
source ~/.bashrc
```
