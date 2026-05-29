# Perubahan pada Proyek Kamar Tidur 3D (OpenGL)

## 1. Apa yang Ditambahkan

### 🖥️ Objek TV (`void tv()`)

Sebuah objek **televisi (TV)** telah ditambahkan ke dalam scene kamar tidur 3D. TV ini terdiri dari beberapa bagian:

| Bagian | Deskripsi | Warna |
|---|---|---|
| **TV Stand** | Meja/rak tempat TV berdiri | Cokelat tua |
| **TV Body (Frame)** | Bingkai luar TV berwarna hitam | Hitam gelap |
| **TV Screen** | Layar TV berwarna biru gelap | Biru gelap (seperti layar mati) |
| **TV Neck** | Leher/penyangga antara layar dan base | Abu-abu gelap |
| **TV Base Kiri** | Kaki penyangga bagian kiri | Abu-abu gelap |
| **TV Base Kanan** | Kaki penyangga bagian kanan | Abu-abu gelap |

### 📍 Posisi TV
TV diletakkan menghadap ke arah tempat tidur, menempel pada dinding depan kamar, kira-kira di koordinat:
- X: `2.0` (tengah ruangan, sisi kanan)
- Y: `0.5` (setinggi pandangan mata saat duduk)
- Z: `4.58` (dekat dinding depan)

### 📞 Pemanggilan di `display()`
Fungsi `tv()` dipanggil di dalam fungsi `display()` setelah `sphericalObject()`.

---

## 2. Bagaimana Cara Membuatnya

### Langkah 1 – Buat fungsi `tv()`

Tambahkan fungsi baru sebelum fungsi `sphericalObject()`:

```cpp
void tv()
{
    // ... bagian-bagian TV
}
```

### Langkah 2 – Buat TV Stand (meja TV)

TV stand dibuat menggunakan `drawCube1` yang di-scale menjadi bentuk persegi panjang pipih dan lebar:

```cpp
// tv stand / meja tv
glPushMatrix();
glTranslatef(2.0, -0.5, 4.55);
glScalef(0.6, 0.17, 0.13);
drawCube1(0.3, 0.15, 0.05, 0.15, 0.075, 0.025);
glPopMatrix();
```

- `glTranslatef` → memindahkan posisi ke tempat yang diinginkan
- `glScalef` → mengubah ukuran kubus menjadi bentuk meja tipis dan lebar
- `drawCube1(r, g, b, ambR, ambG, ambB)` → menggambar dengan warna cokelat

### Langkah 3 – Buat Bingkai TV (Frame Hitam)

Frame TV dibuat dengan kubus yang sangat tipis (scale Z sangat kecil = `0.0001`):

```cpp
// tv body (frame hitam)
glPushMatrix();
glTranslatef(2.0, 0.5, 4.58);
glScalef(0.6, 0.4, 0.0001);
drawCube1(0.05, 0.05, 0.05, 0.025, 0.025, 0.025);
glPopMatrix();
```

> **Trik:** Nilai `glScalef` pada sumbu Z dibuat sangat kecil (`0.0001`) agar objek tampak seperti **permukaan 2D** yang menempel di dinding.

### Langkah 4 – Buat Layar TV

Layar dibuat sedikit lebih kecil dari frame dan diposisikan sedikit lebih maju (Z lebih besar) agar tampak di atas frame:

```cpp
// tv screen (layar biru gelap)
glPushMatrix();
glTranslatef(2.05, 0.55, 4.585);
glScalef(0.55, 0.35, 0.0001);
drawCube1(0.05, 0.1, 0.2, 0.025, 0.05, 0.1, 80);
glPopMatrix();
```

- Warna `(0.05, 0.1, 0.2)` → biru gelap (simulasi layar mati)
- `shine = 80` → layar sedikit mengkilap

### Langkah 5 – Buat Leher dan Kaki TV

Leher (neck) menghubungkan layar dengan kaki:

```cpp
// tv neck
glPushMatrix();
glTranslatef(2.25, 0.0, 4.6);
glScalef(0.1, 0.17, 0.05);
drawCube1(0.1, 0.1, 0.1, 0.05, 0.05, 0.05);
glPopMatrix();
```

Dua kaki TV (kiri dan kanan):

```cpp
// tv base kiri
glPushMatrix();
glTranslatef(2.05, -0.52, 4.58);
glScalef(0.12, 0.02, 0.1);
drawCube1(0.1, 0.1, 0.1, 0.05, 0.05, 0.05);
glPopMatrix();

// tv base kanan
glPushMatrix();
glTranslatef(2.43, -0.52, 4.58);
glScalef(0.12, 0.02, 0.1);
drawCube1(0.1, 0.1, 0.1, 0.05, 0.05, 0.05);
glPopMatrix();
```

### Langkah 6 – Panggil di `display()`

Tambahkan pemanggilan `tv()` di dalam fungsi `display()`:

```cpp
void display(void)
{
    // ...
    sphericalObject();
    tv();          // <-- tambahkan ini
    lightBulb1();
    // ...
}
```

---

## 3. Ringkasan Konsep yang Digunakan

| Konsep | Penjelasan |
|---|---|
| `glPushMatrix()` / `glPopMatrix()` | Menyimpan & memulihkan matrix transformasi agar setiap bagian tidak mempengaruhi bagian lain |
| `glTranslatef(x, y, z)` | Memindahkan posisi objek di ruang 3D |
| `glScalef(x, y, z)` | Mengubah ukuran objek (scale sangat kecil = objek flat/pipih) |
| `drawCube1(dif, amb, shine)` | Fungsi helper yang sudah ada untuk menggambar kubus dengan material OpenGL |
| **Layering Z** | Layar diletakkan sedikit lebih maju dari frame (`Z + 0.005`) agar tidak terjadi *z-fighting* |

---

## 4. Struktur Komponen TV

```
TV
├── TV Stand (meja)
├── TV Body (frame hitam)
│   └── TV Screen (layar biru)
└── TV Neck (leher)
    ├── TV Base Kiri (kaki)
    └── TV Base Kanan (kaki)
```
