# Module 6: Concurrency
## Tutorial Advanced Programming 2023/2024 Genap

* Nama  : Tengku Laras Malahayati
* NPM   : 2206081641
* Kelas : A

## Reflection
### Commit 1 Reflection
#### What is inside the `handle_connection` method?

Inside the `handle_connection` method, we utilize `BufReader` to facilitate efficient and repetitive read operations on the same file or network connection, specifically for `TcpStream` in this context. Prior to our modifications, the program only offered messages indicating whether the browser successfully attempted our request which lacked a method to read the response. Therefore, we introduced `BufReader` to swiftly and repeatedly read the response message by encapsulating `TcpStream`.

Moreover, there is also 
```
let http_request: Vec<_> = buf_reader 
    .lines() 
    .map(|result| result.unwrap())
    .take_while(|line| !line.is_empty()) 
    .collect();
    
println!("Request: {:#?}", http_request);
```
which is used to read and divide every line then wrap them into String, perform each iteration until empty String is found, then collect the result into a Vector.