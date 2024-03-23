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

### Commit 2 Reflection
#### What have I learned about the new `handle_connection` code?

I added modifications on the `main.rs` program so that it will be able to return a HTML page when I access the server through my browser. Here is the modifications: 
```
let status_line = "HTTP/1.1 200 OK";
let contents = fs::read_to_string("hello.html").unwrap();
let length = contents.len();

let response = 
format!("{status_line}\r\nContent-Length:
{length}\r\n\r\n{contents}");

stream.write_all(response.as_bytes()).unwrap();
```
According to the Rust documentation (on Chapter 20), the `fs` statement is used to bring the standard library's filesystem module so that it can read the contents in our HTML file into string. We can also set the size of `hello.html` contents which will be shown on our browser by using `Content-Length` to ensure a valid HTML response by our web server. If the response is invalid, the program will stop as stream will unwrap it. 

Here is the example of my browser display when I access the web server:

![Commit 2 screen capture](/assets/images/Commit%202%20Tutorial%206%20Adpro.png)