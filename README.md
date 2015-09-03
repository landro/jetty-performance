footer: Bekk fagdag, 4. september 2015
slidenumbers: true

# Jetty Performance
### Stefan Magnus Landrø
---

## Jetty 

- Java HTTP server 
- Java Servlet container
- Free/libre open source 
- Embeddable
  - Configure in code

---

## Traditional Web Server Performance Metrics

- Requests per second
- Average Response times (std dev) 
- (web framework performance?)

---

# C10k problem

- Optimising network sockets to handle a large number of clients at the same time
- C10k = ten thousand concurrent connections
  - Chrome 45 : 6 connections per hostname
  - IE 11 : 13 connections per hostname

---

## Modern Web Server Performance Metrics

- Requests per second
- Response times (percentiles)
- Nb of concurrent connections

---

## Jetty 6, 7 & 8

-  SocketConnector 
  - Blocking I/O - one thread per connection
-  SelectChannelConnector 
  - Non blocking - threads are allocated to connections with requests

---

## Jetty 9 
- ServerConnector
  - Selector-based non blocking I/O connector

---

# Threaddump 

```
"qtp1296064247-19-selector-ServerConnectorManager@359cc65/1":
  at sun.nio.ch.KQueueArrayWrapper.kevent0(Native Method)
  ...
  at sun.nio.ch.SelectorImpl.select()
  at org.eclipse.jetty.io.SelectorManager$ManagedSelector.select()
  at org.eclipse.jetty.io.SelectorManager$ManagedSelector.run()

```

--- 


```java 
/*
 * KQueueArrayWrapper.java (Open JDK)
 * Implementation of Selector using FreeBSD / Mac OS X kqueues
 * Derived from Sun's DevPollArrayWrapper
 */

package sun.nio.ch;

...

class KQueueArrayWrapper {
...
}
``` 

---

# kqueue (BSD) / epoll (Linux)

Scalable event notification interface in modern operating systems

- Efficient I/O event pipelines between kernel and userland
- Extremely efficient when polling for events on a large number of file descriptors (e.g. sockets)

---
# Conclusion:

## Jetty performance =~ OS performance


