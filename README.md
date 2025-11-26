# Transport Layer Security (TLS/SSL)

> For thread-local storage, which also abbreviates to TLS, see the [Shared-Everything Threads Proposal](https://github.com/WebAssembly/shared-everything-threads).

---

# wasi-tls

A proposed [WebAssembly System Interface](https://github.com/WebAssembly/WASI) API.

### Current Phase

wasi-tls is currently in [Phase 1](https://github.com/WebAssembly/WASI/blob/main/docs/Proposals.md#phase-1---feature-proposal-cg)

### Champions

<!---
Please limit to one champion per company or organization
-->
- [Dave Bakker]([@badeend](https://github.com/badeend))
- [Joel Dice](https://github.com/dicej)
- [James Sturtevant](https://github.com/jsturtevant)

### Portability Criteria

TODO before entering Phase 2.

## Table of Contents [if the explainer is longer than one printed page]

- [Transport Layer Security (TLS/SSL)](#transport-layer-security-tlsssl)
- [wasi-tls](#wasi-tls)
    - [Current Phase](#current-phase)
    - [Champions](#champions)
    - [Portability Criteria](#portability-criteria)
  - [Table of Contents \[if the explainer is longer than one printed page\]](#table-of-contents-if-the-explainer-is-longer-than-one-printed-page)
    - [Introduction](#introduction)
    - [Goals \[or Motivating Use Cases, or Scenarios\]](#goals-or-motivating-use-cases-or-scenarios)
    - [Non-goals](#non-goals)
    - [API walk-through](#api-walk-through)
      - [Use with wasi-sockets to make tls connection](#use-with-wasi-sockets-to-make-tls-connection)
      - [Use to make connection to database](#use-to-make-connection-to-database)
    - [Detailed design discussion](#detailed-design-discussion)
      - [\[Tricky design choice #1\]](#tricky-design-choice-1)
    - [Considered alternatives](#considered-alternatives)
      - [Compile libraries like OpenSSL to Wasm](#compile-libraries-like-openssl-to-wasm)
      - [Use wasi-crypto](#use-wasi-crypto)
    - [Stakeholder Interest \& Feedback](#stakeholder-interest--feedback)
    - [References \& acknowledgements](#references--acknowledgements)

### Introduction

Wasi-tls is aimed at providing a high level api that provides the ability to read and write encrypted data over a stream.  The API is a TLS specific way for clients and servers to configure the connection.  The encryption work is done by the host allowing implementors to re-use hardened solutions and also perform more advance solutions such as hardware offloading and Kernel TLS.

### Goals [or Motivating Use Cases, or Scenarios]

- Use wasi-sockets to open a connection to a web server and then communicate using TLS via wasi-tls
- Enable mTLS connections to databases

### Non-goals

- Provide a fully flushed out implementation of TLS/SSL
- Provide low level TLS primitives

### API walk-through

The full API documentation can be found [in imports](imports.md).

#### Use with wasi-sockets to make tls connection

A simple example in sudo code:

```
// initiate and complete handshake
let handshake =  ClientHandshake::new(DOMAIN, tcp_input, tcp_output);
let (client, tls_input, tls_output) = handshake.finish().await?;

// send data to server and read
tls_output.write("GET / HTTP/1.1\r\nHost: {DOMAIN}\r\n\r\n").await?;
tls_output.read(buffer).await?;

//close the connection
client.close_notify()?
```

#### Use to make connection to database

TODO

### Detailed design discussion

#### [Tricky design choice #1]

TODO

### Considered alternatives

#### Compile libraries like OpenSSL to Wasm

We opted to not go with this option due to no constant time operations in WASM and more advance scenarios like hardware acceleration could not be could be leveraged.  

#### Use wasi-crypto

We opted to not go with this option since [wasi-crypto](https://github.com/WebAssembly/WASI-crypto) is intended for low level use cases and it would be difficult to use correctly and require developers to re-implement libraries that already exist. It might be possible to virtualize wasi-tls using wasi-crypto in the future. 

### Stakeholder Interest & Feedback

TODO before entering Phase 3.

[This should include a list of implementers who have expressed interest in implementing the proposal]

### References & acknowledgements

- [Pre-proposal](https://docs.google.com/presentation/d/1C55ph_fSTRhb4A4Nlpwvp9JGy8HBL6A1MvgY2jrapyQ/edit?usp=sharing)
- [Proposal to WG](https://github.com/WebAssembly/meetings/blob/main/wasi/2025/WASI-01-09.md)
- [Initial GitHub issue and discussion](https://github.com/WebAssembly/wasi-sockets/issues/100)
- [Draft PR, usage examples, and compatibility evaluation](https://github.com/WebAssembly/wasi-sockets/pull/104)
- [.NET guest + Wasmtime host proof-of-concept](https://github.com/dicej/dotnet-wasi-tls)
- [.NET runtime prototype](https://github.com/dotnet/runtime/compare/main...jsturtevant:runtime:wasi-tls-2)

Many thanks for valuable feedback and advice from:

- [Person 1]
- [Person 2]
- [etc.]
