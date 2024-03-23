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

### Commit 3 Reflection
#### How to split between response?
The responses are differ by their endpoints. In this part of my code, 
```
let request_line = 
    buf_reader.lines().next().unwrap().unwrap();

let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
    ("HTTP/1.1 200 OK", "hello.html")
} else {
    ("HTTP/1.1 404 NOT FOUND", "404.html")
};
```
`request_line` checks whether the request line of a GET request equals to the / path. If it does, it returns the contents of our `hello.html` file. If it does not, it returns the contents of our `404.html` file, which indicates that the path that we accessed did not available. In this case, I tried accessing `http://127.0.0.1:7878/bad` on my browser and it returned the `404.html` one.

Here is what happened when I tried it:

![Commit 3 screen capture](/assets/images/commit3.png)

#### Why the refactoring is needed?
In this case, refactoring is needed because there are too many duplicated/repeated if-else statements that do the same action, reading files and writing the contents of the files to the stream. The only differences are the status line and the filename. As a result, it is better to put those differences into separated (and smaller)if-else lines to make the code more concise.