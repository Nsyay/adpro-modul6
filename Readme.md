### Commit 1 Reflection notes

Secara umum, handle_connection method adalah method yang membaca HTTP request dan me-return output berupa keterangan dan informasi HTTP request yang dibaca.
```
let buf_reader = BufReader::new(&mut stream);
```
code ini membuat 'BufReader' yang berfungsi untuk membaca dan menyimpan data
```
let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();
```
`http_request` berupa vector yang membaca lines dari `buf_reader` dengan menggunakan `line()` method. Kemudian `.map(|result| result.unwrap())` berfungsi untuk membuka yang dihasilkan `lines()` dan menangani kesalahan yang mungkin terjadi selama pembacaan.
`take_while()` berfungsi mengumpulkan line yang dibaca hingga line kosong. `collect()` mengumpulkan semua line yang di read dan dimasukkan ke vector `http_request`
```
println!("Request: {:#?}", http_request);
```
Terakhir, code ini digunakan untuk mengembalikan output `http_request` dengan `{:#?}` untuk menambah indentasi agar lebih rapih dan mudah dibaca.


### Commit 2 Reflection notes
![Commit 2 screen capture](/assets/images/hello_rust.png)

Perubahan pada method handle_connection kali ini yaitu bagaimana caranya agar web server yang kita buka me-return file HTML.

pertama, kita menambahkan modul `fs` yang berfungsi untuk menyediakan fungsi pada Rust dalam berinteraksi dengan suatu file.
kemudian `let status_line = "HTTP/1.1 200 OK";` adalah menginisiasi respons HTTP dengan kode status 200 yaitu menyatakan bahwa respons berhasil.
`let contents = fs::read_to_string("hello.html").unwrap();` line ini akan membaca file html hello dan dimasukkan ke contents. dibuat juga variabel length `let length = contents.len();` yang menyimpan panjang contents. Setelah itu digabungkan menjadi satu response html pada variabel response `let response = format!("{status_line}\r\nContent-Length:{length}\r\n\r\n{contents}");`.

Terakhir, `stream.write_all(response.as_bytes()).unwrap();` line ini mengirimkan kembali respons yang telah diubah menjadi urutan byte.

### Commit 3 Reflection notes
![Commit 3 screen capture](/assets/images/bad_rust.png)

Perubahan pada method handle_connection kali ini yaitu bagaimana caranya agar web server yang kita buka dapat me-return page error apabila tidak ditemukan page yang sesuai.

Pertama, code ini akan mengecek apakah `request_line` berisi GET request. apabila iya, maka akan masuk ke kondisi satu, dimana HTML file ditemukan, sesuai dan akan me-return contents dari file HTML tersebut. 
Pada kasus ini, ketika kode status adalah 200, maka akan dibaca file HTML hello dan dikembalikan ke client.

Kemudian, apabila `request_line` tidak berisi GET request, maka akan masuk ke kondisi else, dimana kita akan menginisiasi `let status_line = "HTTP/1.1 404 NOT FOUND";` dan membaca file HTML 404 dengan code `let contents = fs::read_to_string("404.html").unwrap();`. Hal ini sama dengan cara membaca dan me-return file HTML hello sebelumnya

Dengan ditambahkannya conditional ini, apabila client membuka halaman yang tidak valid atau tidak tersedia, maka akan dialihkan ke halaman error HTML melalui `404.html`

Jika diperhatikan, pada code yang sudah saya terapkan, terdapat duplikasi code. Oleh karena itu saya akan melakukan refactor untuk menghilangkan duplikasi tersebut
![Refactor Commit 3 screen capture](/assets/images/refactor_commit3.png)