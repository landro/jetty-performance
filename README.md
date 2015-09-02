# Jetty Performance

## C10k problem
The C10k problem is the problem of optimising network sockets to handle a large number of clients at the same time. The name C10k is a numeronym for concurrently handling ten thousand connections. Note that concurrent connections are not the same as requests per second, though they are similar: handling many requests per second requires high throughput (processing them quickly), while high number of concurrent connections requires efficient scheduling of connections.

## Connectors

### Jetty 7 & 8

#### SelectChannelConnector
This connector uses efficient NIO buffers with a non-blocking threading model. Jetty uses Direct NIO buffers, and allocates threads only to connections with requests. Synchronization simulates blocking for the servlet API, and any unflushed content at the end of request handling is written asynchronously. See

#### SocketConnector
This connector implements a traditional blocking IO and threading model. Jetty uses Normal JRE sockets and allocates a thread per connection. Jetty allocates large buffers to active connections only. You should use this Connector only if NIO is not available.

### Jetty 9 

####  ServerConnector
Jetty 9 has only a selector-based non blocking I/O connector