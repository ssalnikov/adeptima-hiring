
## Golang Developer Check List
Here’s that we discuss during the interview.

- Have you pass Go Tour, https://tour.golang.org/welcome/1
- Have you read Effective Go, https://golang.org/doc/effective_go.html
- Have you tried to reinvent MVC frameworks patterns in Go?
    * Which Go framework is the best from your perspective and why?
    * Did you use any tool from [Gorilla Toolkit](http://www.gorillatoolkit.org/)?
    * What’s your opinion on Echo? [Echo - High performance, minimalist Go web framework](https://echo.labstack.com/)
- What did you use for dependency management?
- What package did you use for logging: log, logrus, zap etc.?
- Did you use panic/recover and why?
- Did you know what is virtual methods table and does it present in the Go?
- What is Go good for? Concurrency / Networking? What else?
- Golang and its features
    * How to handle Context for logging and monitor system resources?
    * What’s the potential source for Race condition / Data race / Deadlocks in Go?
    * Do you understand Async programming patterns in Go?
    * Would you avoid using ORM? If - yes, why?
    * When not to use Reflect?
    * Do you understand Interfaces implementation in Go?
    * Is Go a OOP (Object Oriented Programming) language, and why?
    * When should you use and not dynamic array args?
    * Assert or not Assert during testing? [Testing in Golfing](https://medium.com/@thejasbabu/testing-in-golang-c378b351002d)
- What is the difference and use cases for WebSocket, http/2 SSE and [gRPC](https://grpc.io/)
    * [Will WebSocket survive HTTP/2?](https://www.infoq.com/articles/websocket-and-http2-coexist)
- What would be criteria for choosing Messaging, In-Memory solution in your project?
    * [Kafka vs. Redis: Log Aggregation Capabilities and Performance](https://logz.io/blog/kafka-vs-redis/)
- What’s your level of confidence with SQL, JSON, Stored Procedures in Postgres?
- How indexes works (BTree, GIN/GIST).
- When should you use popular DevOps tools and why? Ex. CI, Docker, Etcd, Kubernetes
- How would you handle security in Go?
    * Check [Top 10 2017-Top 10 - OWASP](https://www.owasp.org/index.php/Top_10_2017-Top_10) if not sure
- Who is the most impressive person in Go community?
    * Rob Pike Twitter: [@rob_pike](https://twitter.com/rob_pike) [github.com/robpike](https://github.com/robpike)
    * Russ Cox Twitter: [@_rsc](https://twitter.com/_rsc) [github.com/rsc](https://github.com/rsc)
    * Dave Cheney Blog: dave.cheney.net [@davecheney](https://twitter.comdavecheney) [github.com/davecheney](https://github.com/davecheney)
    * [Подкаст GolangShow](https://golangshow.com/)
    * https://golangnews.com
    

### Unsorted
* https://github.com/quii/learn-go-with-tests
* https://github.com/hoanhan101/ultimate-go
* https://github.com/Alikhll/golang-developer-roadmap
* https://github.com/ardanlabs/gotraining/blob/master/topics/go/README.md#design-guidelines
* http://sijinjoseph.com/programmer-competency-matrix/


#### Community Leaders & Trend-setter
- https://www.ardanlabs.com/ << copy business approach?
- https://dave.cheney.net/  <<< steal topics?
- https://blog.golang.org/ <<< know all?
- https://golangweekly.com/issues
- https://golangnews.com/
- https://github.com/trending/go?since=monthly
- https://www.reddit.com/r/golang/
- https://github.com/avelino/awesome-go

### Go-micro
We are considering to use Go Micro to abstract away the details of distributed systems. Here are the main features:

-   **Service Discovery**  - Automatic service registration and name resolution
-   **Load Balancing**  - Client side load balancing built on discovery
-   **Message Encoding**  - Dynamic encoding based on content-type with protobuf and json support
-   **Sync Streaming**  - RPC based communication with support for bidirectional streaming
-   **Async Messaging**  - Native PubSub messaging built in for event driven architectures

Go Micro supports both the Service and Function programming models. Read on to learn more.

#### Go-micro Enterprise
-   secure by default: tls enabled
-   authentication: rbac and service-to-service
-   central control plane
-   circuit breaking
-   rate limiting
-   distributed tracing
-   intelligent routing
-   metric instrumentation
-   performance tuned
-   config reloading
-   dynamic plugin loading

#### Go-micro Links
-   https://github.com/micro/go-micro
-   https://github.com/micro/enterprise
-   https://micro.mu/pricing/
-   https://backy.io/features/
