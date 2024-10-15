# EdgeDetection

# Proyek ini adalah contoh sederhana dari deteksi objek dalam gambar berdasarkan warna tertentu, menggunakan OpenCV di lingkungan Google Colab. Pada contoh ini, kita mendeteksi objek yang berwarna biru menggunakan model warna HSV. Namun, kode ini bisa disesuaikan untuk mendeteksi objek dengan warna lain.

#### Fitur
1. Deteksi objek berdasarkan rentang warna tertentu di gambar.
2. Menggunakan transformasi HSV untuk mengidentifikasi warna.
3. Menampilkan gambar asli dan hasil deteksi objek secara berdampingan menggunakan Matplotlib.

#### Persyaratan
1. Google Colab (karena proyek ini menggunakan files.upload() untuk mengunggah file dari lokal)
2. OpenCV (pre-installed di Colab)
3. Numpy
4. Matplotlib

### Cara Menggunakan

#### 1. Upload Gambar
Untuk mengunggah gambar milik pengguna sendiri, gunakan fungsi files.upload() yang disediakan oleh Google Colab. Kode berikut akan membuka jendela untuk memilih file gambar dari komputer Anda:

```python
from google.colab import files
file = files.upload()
```
Gambar yang dipilih kemudian disimpan dalam variabel file_name, dan akan digunakan untuk proses deteksi berikutnya.

#### 2. Mengganti Gambar
Kode berikut membaca gambar yang diunggah oleh pengguna:

```python
file_name = list(file.keys())[0]  # Mendapatkan nama file
image = cv2.imread(file_name)     # Membaca gambar
```
Pengguna perlu mengunggah gambar milik mereka ketika prompt upload muncul.

#### 3. Mengaburkan dan Mengonversi ke HSV
Sebelum mendeteksi warna, gambar diubah menjadi format HSV, yang lebih sesuai untuk mendeteksi warna berdasarkan rentang tertentu:

```python
blurred_frame = cv2.GaussianBlur(image, (5, 5), 0)  # Mengaburkan gambar
hsv = cv2.cvtColor(blurred_frame, cv2.COLOR_BGR2HSV) # Konversi ke HSV
```

#### 4. Mengatur Rentang Warna HSV
Kode ini menetapkan rentang warna untuk mendeteksi objek. Pada contoh ini, warna biru digunakan. Rentang HSV untuk biru dapat diatur dengan nilai berikut:

```python
lower_blue = np.array([100, 150, 0], dtype=np.uint8)
upper_blue = np.array([140, 255, 255], dtype=np.uint8)
```
Catatan: Pengguna dapat mengganti warna yang ingin dideteksi dengan mengubah nilai lower dan upper. Misalnya, jika ingin mendeteksi warna kuning, ganti dengan:

```python
lower_yellow = np.array([20, 100, 100], dtype=np.uint8)
upper_yellow = np.array([30, 255, 255], dtype=np.uint8)
```

#### Menyesuaikan Rentang Warna
Hue (H) dalam ruang warna HSV biasanya berkisar antara 0 hingga 179 pada OpenCV. Jika Anda menggunakan aplikasi lain yang menunjukkan nilai dari 0 hingga 360, Anda perlu membagi nilai tersebut menjadi 180. Misalnya, jika aplikasi menunjukkan 160 untuk biru, gunakan 160/2 = 80 dalam OpenCV.
Jika objek yang Anda coba deteksi memiliki hue yang berbeda (misalnya, lebih ke arah biru kehijauan), Anda mungkin perlu menyesuaikan batas warna lebih jauh. Anda dapat memperluas rentang warna biru, contohnya:

```python
lower_blue = np.array([90, 100, 100], dtype=np.uint8)  # Batas bawah
upper_blue = np.array([150, 255, 255], dtype=np.uint8)  # Batas atas
```
Untuk menemukan rentang HSV dari warna lain, pengguna bisa menggunakan alat seperti colorizer.org atau meneliti tabel nilai HSV online.

#### 5. Mendeteksi Garis dengan Hough Transform
Setelah mask diterapkan pada gambar, kita dapat mendeteksi garis dalam objek yang sesuai dengan rentang warna:

```python
edges = cv2.Canny(mask, 74, 150)
lines = cv2.HoughLinesP(edges, 1, np.pi / 100, 50, maxLineGap=50)
if lines is not None:
    for line in lines:
        x1, y1, x2, y2 = line[0]
        cv2.line(image, (x1, y1), (x2, y2), (0, 255, 0), 10)
```

#### 6. Menampilkan Hasil
Gambar asli dan hasil deteksi ditampilkan secara berdampingan menggunakan Matplotlib:

```python
fig, ax = plt.subplots(1, 2, figsize=(12, 10))
ax[0].imshow(image_asli)
ax[0].set_title('Gambar Asli')

ax[1].imshow(image_rgb)
ax[1].set_title('Deteksi Objek Biru')

plt.show()
```

#### Menyesuaikan Deteksi Warna
Untuk mengganti gambar: Pastikan untuk mengunggah gambar baru melalui dialog files.upload().
Untuk mengganti warna yang ingin dideteksi: Ubah rentang nilai HSV pada bagian berikut:

```python
lower_color = np.array([H_min, S_min, V_min], dtype=np.uint8)
upper_color = np.array([H_max, S_max, V_max], dtype=np.uint8)
```
Gantilah H_min, S_min, V_min, H_max, S_max, dan V_max dengan nilai yang sesuai dengan warna yang ingin dideteksi.

### Contoh Deteksi Warna Lain
```python
# Kuning:
lower_yellow = np.array([20, 100, 100], dtype=np.uint8)
upper_yellow = np.array([30, 255, 255], dtype=np.uint8)

#Merah:
lower_red = np.array([0, 120, 70], dtype=np.uint8)
upper_red = np.array([10, 255, 255], dtype=np.uint8)
```

## Kesimpulan
Proyek ini membantu mendeteksi objek berdasarkan warna dengan mudah menggunakan OpenCV. Gunakan Google Colab untuk menjalankan kode ini dan pastikan mengganti rentang warna sesuai dengan kebutuhan deteksi Anda. Pastikan gambar yang diunggah memiliki objek dengan warna yang sesuai dengan rentang HSV yang Anda tetapkan.

