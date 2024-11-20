+++
title = "HTTP Server"
weight = 501
date = 2024-11-20
+++

I wanted to see how difficult it would be to implement
a simple HTTP/1.0 server. I've come accross [rfc1945]<https://www.rfc-editor.org/rfc/rfc1945.html>
in the past, but never actually read it properly. 

I am a self-taught programmer and lower level network
programming is something I've not had much experience with.
I did get my feet wet last year with the [protohackers](https://github.com/poudel/protohacke-rs)
network challenges, but there's a lot I don't know about.

My goal was to avoid reading any tutorials, and just work
off of the RFC and the socket library. There's nothing wrong
with following tutorials, I just wanted to challenge myself.

Here's what I ended up with after a couple of hours. 

{% folded(title="http.py") %}

```python
#!/usr/bin/env python

# HTTP/1.0 implementation from RFC1945
# https://datatracker.ietf.org/doc/html/rfc1945

import dataclasses
import socket
from datetime import datetime


@dataclasses.dataclass
class Request:
    method: str
    path: str
    protocol: str
    headers: dict[str, str]
    body: str

    @property
    def uri(self) -> str:
        return "http://" + self.headers["HOST"] + self.path

    @classmethod
    def make(cls, raw: bytes) -> "Request":
        decoded = raw.decode()
        # todo: validate line
        headers = {}
        body = None

        splitted = decoded.split("\n")

        # todo: validate
        method, path, protocol = splitted[0].strip().split(" ")

        headers_done = False
        done = False

        for item in splitted[1:]:
            if item == "" and headers_done is False:
                headers_done = True
                body = item
            elif item == "" and done is False:
                done = True
            else:
                splitted_headers = item.split(":")
                if len(splitted_headers) < 2:
                    print("NOOOO", splitted_headers)
                    continue

                key, *value = splitted_headers
                headers[key.upper()] = ":".join(value).strip()

        return cls(
            method=method, 
            path=path, 
            protocol=protocol, 
            headers=headers, 
            body=body
        )

@dataclasses.dataclass
class Response:
    request: Request
    status: int
    headers: dict[str, str]
    body: bytes

    @classmethod
    def make(cls, request: Request, status: int,  body: bytes | None, **headers) -> "Response":
        # Date: Tue, 15 Nov 1994 08:12:31 GMT
        datefmt = "%a, %d %b %Y %H:%M:%S GMT"
        datestr = datetime.now().strftime(datefmt)

        headers.update({
            "Location": request.uri,
            "Server": "keshab/0.1",
            "Allow": "GET",
            "Content-Type": "text/html",
            "Content-Length": len(body),
            "Date": datestr,
            "Last-Modified": datestr
        })
        return Response(request=request, status=status, headers=headers, body=body)

    def encode(self):
        headerstr = "\n".join([
            f"{key}: {value}" for key, value in self.headers.items()
        ])
        resp = f"{self.request.protocol} {self.status}\n{headerstr}\n"
        return resp.encode() + self.body


def handle_request(data: bytes) -> Response:
    print("\nSTART\n", data.decode(), "\nEND")
    request = Request.make(data)
    datestr = datetime.now().isoformat()

    response_body = f"""
<html><body>
    <h1>Hello world</h1>
    <h2>Now: {datestr}</h2>
</body>
</html>
""".encode()

    response = Response.make(
        request=request,
        status=200,
        body=response_body,
    )
    return response


def runserver(host: str, port: int):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind((host, port))
    print(f"Listening at http://{host}:{port}")

    s.listen(1)

    while True:
        conn, addr = s.accept()

        while True:
            data = conn.recv(1024)
            if not data:
                break

            try:
                response = handle_request(data)
                encoded = response.encode()
                conn.sendall(encoded)
            except Exception as err:
                print(f"Error while handling request: {err}")

            break

        try:
            conn.close()
        except Exception:
            pass

    try:
        s.close()
    except Exception:
        pass



if __name__ == "__main__":
    runserver(host="127.0.0.1", port=6111)

```
{% end %}

and running it is just 

```shell
./http.py
```

The RFC was actually really easy to follow. I didn't actually
read everything though, and I was aware of the major concepts
already. But I did learn about the `From` header for the 
first time.

The code snippet above is too long for what it actually does.
This same functionality could be compressed into a far fewer
lines but I'm too lazy to refactor that now.

I want to move onto handling multiple requests and use `asyncio` 
for an extra bit of challenge.


## with asyncio

The `asyncio` library seems slightly more [ergonomic](https://docs.python.org/3/library/asyncio-stream.html#tcp-echo-server-using-streams)
compared to `socket`.

{% folded(title="http2.py") %}
```python
#!/usr/bin/env python
# https://datatracker.ietf.org/doc/html/rfc1945
import socket
import asyncio
import socket
from datetime import datetime


async def handle_request(reader, writer):
    data = await reader.read(1024)
    message = data.decode()
    addr = writer.get_extra_info("peername")

    datefmt = "%a, %d %b %Y %H:%M:%S GMT"
    datestr = datetime.now().strftime(datefmt)

    body = b"<html><body><h1>Hello</h1></body></html>"
    resp = (
        f"HTTP/1.1 200 OK\n"
        "Location: http://localhost:6111/\n"
        "Server: keshab/0.2\n"
        "Allow: GET\n"
        "Content-Type: text/html\n"
        f"Content-Length: {len(body)}\n"
        f"Date: {datestr}\n"
    ).encode()

    writer.write(resp + b"\n" + body)
    await writer.drain()

    writer.close()
    await writer.wait_closed()


async def runserver():
    server = await asyncio.start_server(
        handle_request,
        "127.0.0.1", 
        6111,
        reuse_address=True,
        reuse_port=True,
    )
    print(f"Listening on 6111")
    async with server:
        await server.serve_forever()


if __name__ == "__main__":
    asyncio.run(runserver())
```
{% end %}
