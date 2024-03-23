# Module 6

Nama: Emir Mohamad Fathan
NPM: 2206081982
Kelas: A

1. Commit 1 Reflection

Agar rust dapat menangani koneksi ke server, yaitu menangani koneksi TCP, dibuatlah sebuah fungsi baru `handle_connection`. Fungsi ini membuat BufReader yang membaca dari parameter TcpStream. Hal ini berfungsi untuk membaca data dari stream dan menyimpannya. Setelah itu terdapat variabel http_request yang meyimpan request yang dikirimkan browser ke server. Dengan begitu, fungsi ini membaca permintaan HTTP browser dari stream, mengelolanya, lalu meng-printnya.