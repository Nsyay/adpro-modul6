# Commit 1 Reflection notes

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
