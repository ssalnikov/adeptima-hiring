
### Post-REST discussion
https://www.tbray.org/ongoing/When/201x/2018/11/18/Post-REST

https://news.ycombinator.com/item?id=18485978

Post-REST discussion, gRPC and Go-micro discussion stacked in the above post ... 

TL;DR 
Don't pitch Go-micro without deep understanding of networking, distributed system, message queueing

Key take-aways:

- REST falls down in transactions

- However, REST for simple enough schemas and transactions involving only one relation is OK

- You could build  a transactional API in Erlang, Haskell, or Go fairly easily, where it is easy and not necessarily unidiomatic to have a thread persist for a long period of time

- Most of our customers end up putting a junior or intermediate engineer familiar with JSON

- For anything low latency, the cost of JSON encoding/decoding starts to become extremely difficult to avoid 

- JSON and in favor of Protobuf or vice-versa depends on which language you're operating in. Most benchmarks I've seen show that Protobuf is faster to encode and decode for Java and Golang while JSON is faster for Python and Javascript

- You POST to get a transaction ID, the thread starts a DB transaction ...

- Modern world is load-balanced, multi-region/zone topology

- Study HTTP/2, HTTP/3 ??? 

- Long polling, WebSockets vs Server-Sent Events compare?

	Using SSE Instead Of WebSockets For Unidirectional Data Flow Over HTTP/2
	https://www.smashingmagazine.com/2018/02/sse-websockets-data-flow-http2/

	How JavaScript works: Deep dive into WebSockets and HTTP/2 with SSE + how to pick the right path
	https://blog.sessionstack.com/how-javascript-works-deep-dive-into-websockets-and-http-2-with-sse-how-to-pick-the-right-path-584e6b8e3bf7

- HTTP calls for front-end, but not back-end to back-end services

- Protobuf has lot of issues study it ... why people building lighter versions?

- QUIC is a new transport which reduces latency compared to that of TCP. On the surface, QUIC is very similar to TCP+TLS+HTTP/2 implemented on UDP. Because TCP is implemented in operating system kernels, and middlebox firmware, making significant changes to TCP is next to impossible. However, since QUIC is built on top of UDP, it suffers from no such limitations.

	https://www.chromium.org/quic
	https://en.wikipedia.org/wiki/QUIC

- RPC (non-CRUD interactions) or streaming updates (e.g. a spinner in a app representing a pending job) or low latency (chat messages / presence status, etc.) don't really fit to HTTP requests so great (requiring e.g. polling) so you should look at alternatives (probably something proxied over a websocket in the case of a browser front-end app).

- Sticky sessions??? 
Sticky sessions are the classical answer to this, but they are very much something you want to avoid these days, because they require that every proxy along the path to your app is aware of the stickiness. If you end up deploying your app to, say, Google Kubernetes Engine, you don't even get a load balancer that supports stickiness.

- When a request comes in, start the thread, open a listener socket (with, say, gRPC), then store the socket address in some reliable shared data store (Etcd, ZooKeeper, Postgres, etc.). Any requests from the same client can then be routed to that socket internally. If you're writing this in Erlang, you can replace "socket" with "process" and be done in half the time. 
That might work okay for latency-sensitive OLTP stuff, but a good pattern I've used before is to model transactions as explicit state. The client uses CRUD actions to build up a "batch" of data (e.g. in a CMS-type app, create a folder, then create a document in the folder), which is stored separate from live data and completely invisible. Once the client is ready to commit, it sends a commit request, and the backend takes the stored state and tries to apply it to the main database.
There are some benefits to this asynchronous, batch-oriented view of transactions: For example, you get audit logging and debuggability "for free", and the backend can be smart about conflict merging and retrying without involving the client. It also allows the client to post incomplete data; it can create a document before the folder exists, even though the document at this point is not yet valid according to business rules, and later in the same transaction it can "patch" the document with the right parent folder. This maps well to the kinds of ETL that apps tends to want to do, where it's nice to write a thin dumb import process that doesn't have to keep lots of local state

- Scaling code is great, but don't pay the price for 10^8 when you're never going to pass 10^3

- FHIR spec as a transaction Bundle:
https://www.hl7.org/fhir/bundle-transaction.json.html

- If REST ultimately results in heavy use of message queues and async URL responses, the question is why not use eg. AMQP or other messaging protocol straight away rather than bother using HTTP facades/services as unecessary SPoFs (Single Point of Failure) at least between backends.

- What's alternatives of NATS? https://nats.io/

- SQS queue
