**Nama  : Syifa Rahmadina**

**NIM   : H1D021094**

**Cara Kerja Login**

**1. Halaman Login (login.page.html dan login.page.ts)**

![Screenshot (4194)](https://github.com/user-attachments/assets/1f3cf373-2453-458b-a1fc-db6b61b43836)

Disini kita meng-inputkan username dan password, dan jika sudah selesai klik tombol login

Ketika tombol "Login" ditekan, metode login() pada login.page.ts akan dipanggil


**2. Proses dalam Metode login() di login.page.ts**

- Setelah meng-inputkan username dan password fungsi login() pada login.page.ts akan memeriksa apakah salah satu inputannya ada yang kosong atau tidak kosong. Jika salah satu ada yang kosong, maka notifikasi akan ditampilkan menggunakan metode notifikasi() dari AuthenticationService, lalu akan menimbulkan alert bahwa "Username atau Password Tidak Boleh Kosong".

- Jika keduanya terisi, maka akan dibuat data yang berisi username dan password yang telah dimasukkan.

- Lalu setelah objek data telah dibuat fungsi postMethod() dari AuthenticationService akan dipanggil dengan hasil objek data sebagai parameter dan endpoint login.php sebagai argumen kedua.


**3. Koneksi ke Backend dengan postMethod() di AuthenticationService**

- Data login yang diinputkan dikirimkan ke endpoint backend menggunakan metode post yaitu postMethod().

- Jika respons dari backend menunjukkan status login berhasil (res.status_login == "berhasil") maka token dan username pengguna disimpan dengan fungsi saveData(). Lalu username dan password direset menjadi kososng dan pengguna diarahkan ke halaman home (/home) menggunakan this.router.navigateByUrl('/home').

- Jika respons dari backend menunjukan status login gagal, maka fungsi notifikasi() dari AuthenticationService akan dipangggil untuk menampilkan pesan kesalahan.


**4. Menyimpan Data Otentikasi dengan saveData() di AuthenticationService**

- saveData() menyimpan token dan username yang diterima dari respons API ke local storage menggunakan Preferences dari @capacitor/preferences.

- TOKEN_KEY dan USER_KEY digunakan sebagai kunci untuk menyimpan token dan username, sehingga aplikasi dapat mengingat status login pengguna.

- Setelah data tersimpan, isAuthenticated (variabel BehaviorSubject) akan diperbarui menjadi true, yang berarti pengguna sekarang terotentikasi.


**5. Guard untuk Mengamankan Halaman (authGuard dan autoLoginGuard)**

- Fungsi authGuard digunakan untuk mencegah akses ke halaman yang membutuhkan otentikasi jika pengguna belum login.

- Fungsi autoLoginGuard digunakan untuk memeriksa apakah pengguna sudah login dan mencoba mengakses halaman login. Jika pengguna sudah login, guard ini akan otomatis mengarahkan mereka ke halaman home untuk mencegah login ulang.


**6. Fungsi loadData() pada AuthenticationService untuk memuat status otentikasi**

- Ketika aplikasi dimulai, loadData() akan memuat token dan username dari local storage. Jika token ditemukan, berarti pengguna sudah login sebelumnya, dan aplikasi akan mengatur isAuthenticated menjadi true.

- Lalu jika tidak ada token, isAuthenticated tetap false, dan pengguna akan diarahkan ke halaman login jika mencoba mengakses halaman yang membutuhkan otentikasi.


**7. Logout dari Aplikasi (logout() di AuthenticationService dan home.page.ts)**

- Di halaman home (home.page.ts), ada opsi logout. Jika ditekan, fungsi logout() pada AuthenticationService akan dipanggil.

- Fungsi logout() akan menghapus token dan username dari local storage dan memperbarui isAuthenticated menjadi false.

- Setelah melakukan proses logout, pengguna akan diarahkan kembali ke halaman login (/login), memastikan sesi pengguna telah berakhir dan mereka perlu login kembali untuk mengakses halaman yang memerlukan otentikasi.



**SS Halaman Home dan Tombol Logout**

![Screenshot (4195)](https://github.com/user-attachments/assets/70fe497f-b55b-4db4-b52a-3ebd5ba216e0)


**SS Halaman Hasil Logout**

![Screenshot (4196)](https://github.com/user-attachments/assets/8db0b5e6-15a5-45b0-b8b6-b3ff3f92ca20)
