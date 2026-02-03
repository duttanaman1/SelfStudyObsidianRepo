---
tags:
  - linker-exclude
---
Important topic
Need to understand when to use grpc and when to avoid.
Why migrate from REST to grpc.
what improvements does grpc b
### What is gRPC?
gRPC is a modern open-source high-performance Remote Procedure Call (RPC) framework developed by Google. It can efficiently connect services in and across data centers with pluggable support for load balancing, tracing, health checking and authentication.

### Why gRPC over REST?
1. **Protobuf instead of JSON/XML  
    **REST uses JSON for sending and receiving messages. JSON is flexible, text-based and human-readable but it is not fast enough or light-weight enough for transmitting data between systems.  
    gRPC uses Protobuf(protocol buffers) messaging format which is faster and more efficient for data transmission
2. **First-class support for code generation  
    **REST needs third-party tools to generate the code for API calls.  
    gRPC comes with an in-built protoc compiler which provides the code generation features by default.
3. **Built on HTTP 2[^2]  
    **REST is built on HTTP 1.1 which uses a request-response model of communication.  
    gRPC uses HTTP2 so that it supports client-response communication and bidirectional streaming.
4. **Works across languages and platforms  
    **Since the code can be generated for any language, it is easy to create micro-services in any language to interact with each other
![](https://miro.medium.com/v2/resize:fit:286/1*oApAI2FciNHD94_LYQt0xQ.jpeg)

### 4 Types of API in gRPC

![](https://miro.medium.com/v2/resize:fit:563/1*gJzJCGg6aBztnkeJZfrgKQ.jpeg)

Types of API in gRPC

Following is an example of the four types of APIs. You can understand it further when we are implementing the sample.
```
service GreetService {
// Unary  
rpc Greet(GreetRequest) returns (GreetResponse) {};
// ==Streaming== server  
rpc GreetManyTimes(GreetManyTimesRequest) returns (stream GreetManyTimesResponse) {};
// Streaming client  
rpc LongGreet(stream LongGreetRequest) returns (LongGreetResponse) {};
// Bidirectional streaming  
rpc GreetAll(stream GreetAllRequest) returns (stream GreetAllResponse) {};}
```

## 1️⃣ When to use gRPC

**Use gRPC when:**

- **Service-to-service communication** inside microservices
- **Low latency + high throughput** matters
- **Strong API contracts** are needed
- **Streaming** (client, server, bidirectional) is required
- **Polyglot systems** (Java ↔ Go ↔ Python ↔ C++)
**Typical examples**
- Internal microservices communication
- Real-time systems (chat, notifications, market data)
- High-QPS backend calls[^1]
- Data-intensive pipelines
**Why**
- Binary protocol (Protobuf) → faster & smaller
- HTTP/2 multiplexing → fewer connections
- Code generation → fewer contract bugs

---
## 2️⃣ When to avoid gRPC
**Avoid gRPC when:**
- APIs are **public / browser-facing**
- You need **human-readable APIs**
- Clients are **simple or lightweight**
- Infra team lacks **gRPC observability expertise**
- You depend heavily on **caching via HTTP semantics**
**Red flags**
- Direct browser → gRPC (needs gRPC-Web proxy)
- API consumers want curl / Postman simplicity
- Heavy reliance on HTTP status codes & headers
👉 **Rule of thumb:**  
**External APIs → REST/GraphQL**  
**Internal APIs → gRPC**

---
## 3️⃣ What improvements gRPC brings (over REST)
### 🚀 Performance
- Protobuf (binary) vs JSON (text)
- Smaller payloads, faster serialization
- HTTP/2 multiplexing avoids head-of-line blocking
### 📜 Contract-first design
- `.proto` = single source of truth
- Auto-generated clients & servers
- Compile-time safety (huge in large teams)
### 🔄 Streaming
- Client streaming
- Server streaming
- Bi-directional streaming (hard in REST)
### 🔐 Built-in features
- Deadlines & timeouts
- Retries (with interceptors)
- Auth via metadata
- Load balancing support
## 5️⃣ Backpressure compatibility with gRPC
**Yes — gRPC supports backpressure, but differently.**
### How gRPC handles backpressure
- Built on **HTTP/2 flow control**
- Controls:
    - Window size
    - Message buffering
    - Stream readiness
### Java gRPC specifics
- Uses **Flow Control + Async stubs**
- Backpressure is **pull-based**
- Consumer signals readiness before receiving more data

## Interview-ready 1-liners 💡
- **gRPC excels at internal, high-performance service-to-service calls**
- **Avoid gRPC for browser-facing or public APIs**
- **gRPC improves latency, throughput, and contract safety**
- **[[WebFlux]] does not replace gRPC — it complements it**
- **gRPC supports backpressure via HTTP/2 flow control, not Reactive Streams**

[^1]: High-QPS backend calls are service requests optimized for **low latency, high throughput, and predictable performance under heavy concurrent load**.

[^2]: - **HTTP/2 improves performance by multiplexing multiple requests over a single TCP connection.**
	- **It eliminates head-of-line blocking at the application layer present in HTTP/1.1.**
	- **HTTP/2 uses binary framing, making it more efficient than text-based HTTP/1.1.**
	- **Header compression (HPACK) significantly reduces request overhead.**
	- **HTTP/2 enables server push, allowing servers to proactively send resources.**
	- **gRPC is built on top of HTTP/2 to leverage multiplexing and flow control.**
