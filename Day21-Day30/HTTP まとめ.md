## HTTP
- HTTP is a protocol to exchange **any kind of** data between client and server **based on TCP/IP.**
- ~Its features are~ **It achieves some of REST's important features:** stateless, unifined interface **and cache.**
- It has only 8 methods: GET, PUT**(expected to be idempotent)**, POST, DELETE, ~UPDATE~ **HEAD（same as GET but only retunrs header）, OPTIONS(to know the supported methods for the resource or required header fields), TRACE(get the same response as request for debug), CONNECT(ask proxy server to tunnel request) (sometimes PATCH is used to update the part of existing resource)**.
- First client side send a request, server receives it and return response.
- A request and reposnse have header fields and body. 
- Here are some examples from request header fields: origin, Cache-control, referer, ~requestUrl,~ Content-type **(specify media type), Accept (for content negotiation)**
- Here are some examples from response header fields: ~status,~ Content-type, **Access-Control-Allow-Origin**
- HTTP status 1XX: **processing** , 2XX: success, 3XX:redirect , 4XX: client issues, 500: server issues.

### 1.0
- In 1.0, it cannot re-use the connection. So everytime interaction happens, it establish connection and close it. Of course, TCP have to do three-way handshake every single time.

### 1.1 
- ~In 2.0 it is enabled to recycle the conection. So once it is established, it is kept until user close the browser.~ **(This is introduced in HTTP 1.1. called Keep-alive)**
- In 1.1, **only one data can be downloaded at once in one connection and second request should wait for the first requests' completion. **This is called head of line blocking.** So it establish connection multipully at the same time, which is a heavy load for server.**
- **Also, since it is stateless, in most cases, information are added to header and sometimes it size becomes too heavy and overhead becomes large**

### 2.0
- **In 2.0, it can handle multiple data in one connection. It creates multiple "streams", virtual connections and exhange data in a unit called "frame". It reduce the load for server and reduce the number of time to do three-way handshake or TLS handshake.**
- **Also, it becomes binary base from text base. Header and body will be send in different frame.**
- **Furthermore, header can be compressed with HPACK protocol. If there is no diff in header between several request, it will not re-send the header.**
- **In addition, it enables client to specify the priority of resource using 31 bit values.**
- Though 2.0 resolves several issues in 1.1, still it has problem. For example, **TCP** Head of line blocking. **(There are two HOL)** When one of the packets are lost, TCP automatically re-send the packet. However, application cannot start processing the packets until re-sent packet has arrived though packets for another process have arrived fully because it have to re-organize the packets using sequence numbers.

[Reference](https://qiita.com/uutarou10/items/7698ee3336c70a482843)

### 3.0
- To resolve issues in HTTP 2.0, the proposal of HTTP 3.0 has been formulated. One of the biggest changes would be QUIC. QUIC is the **new** communication protocol **in transport layer** based on UDP, not TCP. However, it behaves like TCP. It re-send the packet if it is lost on the way but it can proceed the process if the data for that process are all arrived.
- **It is cipher native. TLS/1.3 provide algorhythm and actual ciphering is done by QUIC. Until 1.2, it takes 3RTT(Round Trip Time) to establish encryption channel but from 1.3, it only takes 0 to 1.** 

[Reference](https://http3-explained.haxx.se/en/quic-connections.html)
