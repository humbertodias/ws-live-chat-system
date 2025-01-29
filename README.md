# WebSocket Load Balancer with HAProxy

## Running the Application

1. Start the services using Docker Compose:
   ```sh
   docker-compose up
   ```
   
2. Open your browser console and run the following commands to create a WebSocket connection:
   ```js
   let ws = new WebSocket("ws://localhost:8080");
   ws.onmessage = message => console.log(`Received: ${message.data}`);
   ws.send("Hello! I'm a client");
   ```
   
3. Open multiple console windows to simulate multiple clients and observe how messages are handled.

## Architecture Diagram

```mermaid
graph TD
    subgraph Load Balancer
        LB[HAProxy]
    end

    subgraph WebSocket Servers
        WS1[WebSocket Server 1]
        WS2[WebSocket Server 2]
        WS3[WebSocket Server 3]
        WS4[WebSocket Server 4]
    end

    subgraph Redis
        PUB[Publisher livechat]
        SUB1[Subscriber livechat]
        SUB2[Subscriber livechat]
        SUB3[Subscriber livechat]
        SUB4[Subscriber livechat]
    end

    Client1[Client] -->|WebSocket Connection| LB
    Client2[Client] -->|WebSocket Connection| LB
    LB -->|Routes Traffic| WS1
    LB -->|Routes Traffic| WS2
    LB -->|Routes Traffic| WS3
    LB -->|Routes Traffic| WS4

    WS1 -->|Publish Message| PUB
    WS2 -->|Publish Message| PUB
    WS3 -->|Publish Message| PUB
    WS4 -->|Publish Message| PUB

    PUB -->|Broadcast Message| SUB1
    PUB -->|Broadcast Message| SUB2
    PUB -->|Broadcast Message| SUB3
    PUB -->|Broadcast Message| SUB4

    SUB1 -->|Send to Clients| WS1
    SUB2 -->|Send to Clients| WS2
    SUB3 -->|Send to Clients| WS3
    SUB4 -->|Send to Clients| WS4
```

Enjoy testing your WebSocket load balancing setup!

### Ref

* [Scaling Websockets with Redis, HAProxy and Node JS - High-availability Group Chat Application](https://www.youtube.com/watch?v=gzIcGhJC8hA)
