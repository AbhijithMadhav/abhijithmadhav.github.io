---
title: Web Sockets
tags: [chat-application]
---

## Chat applications
* Use of HTTP has two disadvantages
    * Opening of a connection(which is costly) for every message
    * Server needs to initiate a connection when sending a message to the recipient. A client typically uses HTTP to initiate a single request-response style communication with the client
* Web sockets allow for bidirectional communication between client and server once a connection has been established