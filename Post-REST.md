
### Post-REST, gRPC

- Protobuf
- Study HTTP/2 vs HTTP/3
- Long polling, WebSockets vs Server-Sent Events compare?

	Using SSE Instead Of WebSockets For Unidirectional Data Flow Over HTTP/2
	https://www.smashingmagazine.com/2018/02/sse-websockets-data-flow-http2/

	How JavaScript works: Deep dive into WebSockets and HTTP/2 with SSE + how to pick the right path
	https://blog.sessionstack.com/how-javascript-works-deep-dive-into-websockets-and-http-2-with-sse-how-to-pick-the-right-path-584e6b8e3bf7

- QUIC is a new transport which reduces latency compared to that of TCP. On the surface, QUIC is very similar to TCP+TLS+HTTP/2 implemented on UDP. Because TCP is implemented in operating system kernels, and middlebox firmware, making significant changes to TCP is next to impossible. However, since QUIC is built on top of UDP, it suffers from no such limitations.

	https://www.chromium.org/quic
	https://en.wikipedia.org/wiki/QUIC

