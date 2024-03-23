# Module 6

Nama: Emir Mohamad Fathan <br>
NPM: 2206081982 <br>
Kelas: A <br>

<hr>

1. **Commit 1 Reflection**

Agar rust dapat menangani koneksi ke server, yaitu menangani koneksi TCP, dibuatlah sebuah fungsi baru `handle_connection`. Fungsi ini membuat BufReader yang membaca dari parameter TcpStream. Hal ini berfungsi untuk membaca data dari stream dan menyimpannya. Setelah itu terdapat variabel http_request yang meyimpan request yang dikirimkan browser ke server. Dengan begitu, fungsi ini membaca permintaan HTTP browser dari stream, mengelolanya, lalu meng-printnya.

2. **Commit 2 Reflection**

![Commit 2 screen capture](/assets/images/commit2.png)

Dari commit sebelumnya, ada tambahan code yaitu ada variabel status line yang menunjukkan untuk status requestnya. Lalu ada juga variabel yang berfungsi membaca hello.html yaitu contents. Lalu variabel contents tersebut diproses dengan dicari lengthnya, responsena. Lalu response tersebut akan direturn dari server ke browser.

3. **Commit 3 Reflection**

![Commit 3 screen capture](/assets/images/commit3.png)

Ada penambahan code pada commit ini yang mana agar fungsi dapat mengembalikan html error saat url yang dimasukkan NOT FOUND.

Menambahkan variabel request_line untuk mengambil request string dari BufReader:
```rust
let request_line = buf_reader.lines().next().unwrap().unwrap();
```

Melakukan pengecekan terhadap request_line:

```rust
    let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "error.html")
    };
```

Jika ternyata NOT FOUND, maka variabel filename akan menjadi error.html. Jadi, pada variabel contents, file yang diambil bukan lagi hello.html melainkan error.html.

4. **Commit 4 Reflection**

Ada penambahan code pada commit ini, yaitu menambahkan endpoint baru berupa /sleep yang berfungsi meng-sleep server selama 10 detik. Dapat diperhatikan, bahwa program rust yang sekarang dibuat adalah single-threaded yang mana saat suatu client menjalankan endpoint /sleep, jika ada client lain yang ingin mengakses server ini, maka client tersebut harus menunggu bersamaan dengan client yang menjalankan endpoint /sleep. 

5. **Commit 5 Reflection**

Agar code menjadi multithreaded, dibutuhkan pembuatan ThreadPool, yaitu sekumpulan thread yang akan bekerja ketika sebuah tugas atau request masuk. Pada threadpool terdapat yang disebut Worker. Nantinya, Worker akan diberikan Job tertentu dari sebuah request yanag masuk. Sementara satu Worker bekerja, Worker yang lain menunggu request yang lain. Maka dari itu, server ini dikerjakan secara paralel pada tiap request baru yang masuk.

Penggunaan multithreaded ada pada file lib.rs. Pada kode ini terdapat potong kode berikut:
```rs
impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv().unwrap();
            job();
        });

        Worker {
            id,
            thread: Some(thread),
        }
    }
}
```
Pada potongan kode ini, program akan membuat thread worker baru saat sebuah request masuk.

Setelah membuat lib.rs, kita akan memanggil lib.rs tersebut di main.rs lalu menjalankan threadpoolnya
```rs
    let pool = lib::ThreadPool::new(4);

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        pool.execute(|| {
            handle_connection(stream);
        });
    }
```