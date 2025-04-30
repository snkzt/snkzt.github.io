---
title: "Protocol Performance: A Local Benchmark of TCP vs UDP"
date: 2025-04-30
layout: default
---

When building network applications, choosing the right protocol can significantly impact performance.
Two of the most common transport protocols, TCP and UDP, offer different trade-offs in reliability, speed, and overhead.

In this post, I compared TCP and UDP round-trip times on a local machine.


### What are TCP and UDP?
- TCP (Transmission Control Protocol) is connection-oriented, ensuring reliable, ordered delivery of packets. It's widely used for web traffic, email, and file transfers.
- UDP (User Datagram Protocol) is connectionless, with minimal overhead, making it suitable for real-time applications, such as video streaming and online games.

But how much faster is UDP in practice?


### Benchmark Setup
- Language: Go
- Envoironment: macOS, localhost
- Messages: 10,000 round-trip messages
- Message size: minimal (small ping–pong messages)

Each message's round-trip time (RTT) was measured using Go's `time` package.
Both protocols were implemented using native Go `net` packages.


### Result Summary
[Code](https://github.com/snkzt/tcp-udp-comparison)
TCP:
```
Average round-trip time: 11.275957ms
Dropped messages: 0
Errors: 0
```

UDP:
```
Average round-trip time: 11.22507mss
Dropped messages: 0
Errors: 0
```


### Interpretation
In this test with a simulated 10ms network delay, the results show TCP and UDP performing nearly identically in average round-trip time.
This may seem surprising given UDP's lightweight reputation, but the closeness of results can be attributed to several factors:
- Artificial delay dominates: The added sleep of 10ms in both servers overshadows protocol differences.
```
// Simulate network by pausing the server for 10 milliseconds after receiving data
time.Sleep(time.Millisecond * 10)
```
- Reliable loopback interface: On localhost, with no packet loss or congestion, the benefits of UDP's minimalism are reduced.
- Efficient TCP implementation: TCP benefits from connection reuse and system-level optimisations.

The result reinforces the idea that real-world conditions are key to revealing true protocol behaviour differences.


### Key Takeaways
- Real-world UDP gains may not appear in local tests. The difference becomes clearer on real networks with latency, jitter, and potential packet loss.
- Use the right tool. TCP is great when reliability matters.


<[Home](https://snkzt.github.io/)>