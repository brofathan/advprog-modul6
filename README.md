# Module 6

Nama: Emir Mohamad Fathan <br>
NPM: 2206081982 <br>
Kelas: A <br>

<hr>

1. Commit 1 Reflection

Agar rust dapat menangani koneksi ke server, yaitu menangani koneksi TCP, dibuatlah sebuah fungsi baru `handle_connection`. Fungsi ini membuat BufReader yang membaca dari parameter TcpStream. Hal ini berfungsi untuk membaca data dari stream dan menyimpannya. Setelah itu terdapat variabel http_request yang meyimpan request yang dikirimkan browser ke server. Dengan begitu, fungsi ini membaca permintaan HTTP browser dari stream, mengelolanya, lalu meng-printnya.

2. Commit 2 Reflection

![Commit 2 screen capture](/assets/images/commit2.png)

Dari commit sebelumnya, ada tambahan code yaitu ada variabel status line yang menunjukkan untuk status requestnya. Lalu ada juga variabel yang berfungsi membaca hello.html yaitu contents. Lalu variabel contents tersebut diproses dengan dicari lengthnya, responsena. Lalu response tersebut akan direturn dari server ke browser.
