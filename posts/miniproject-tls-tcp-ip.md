---
title: "Internet data flow in small scale"
date: 2025-04-28
layout: default
---

## Internet
A network of networks where computers communicate through a set of standardised protocols.

## How the data communication works
All the communications on the internet boils down to the transmission of binary code.
There are several layers involved in the communication process.
To grasp the basic idea, I made a small project: [Secure TCP Chat App in GO](https://github.com/snkzt/secure-chat-app).

## Learnings
### Basic data flow
#### On the internet
Sending data from computer A to B:
```
A: IP address 1.2.3.4           B: IP address 5.6.7.8

Application 			        Application	
↓ TCP			                ↑ TCP
↓ IP			                ↑ IP
Hardware	→	Internet	→	Hardware
```
#### In the app
Client sends messages and server replies to them:
```
Client: IP address localhost   Server: IP address localhost

User input: "hi"
↓ Application: Go client programme reads input and sends via conn.Write()                    Application: Writes back via conn.Write()
↓ TLS: crypto/tls encrypts the message                                                       ↑ TLS: Decrypts and validates info
↓ TCP: Adds segment headers, segments data as required                                       ↑ TCP: Reads and strips header, checks data
IP: Adds packet header  →  Routing: Loops back into network stack via loopback interface  →  IP: Reads and strips header
```

### TCP and TLS
TCP (Transmission Control Protocol) ensures data:
  - Arrives in order
  - Is not lost
  - Is checked for errors
  - Is retransmitted if necessary


TLS (Transport Layer Security) adds:
  - Encryption
  - Authentication
  - Integrity

  In `server.go`
  ```
  // Load the server's TLS certificate and private key
  cert, err := tls.LoadX509KeyPair("./certs/server.crt", "./certs/server.key")

  // Configure TLS with the certificate
  config := &tls.Config{Certificates: []tls.Certificate{cert}}

  // Start listening on TCP port 8080 with TLS encryption
  listener, err := tls.Listen("tcp", ":8080", config)

  ```
  In `client.go`
  ```
  // Connect to the server securely using TLS over TCP
  conn, err := tls.Dial("tcp", "localhost:8080", &tls.Config{
	  InsecureSkipVerify: true, // (skips cert verification for testing)
  })

  ```

### What's achieved
- Sends and receives encrypted data
- Uses the same encryption mechanism as HTTPS
- Handles multiple clients via goroutines
- Models real-world network protocols and encryption