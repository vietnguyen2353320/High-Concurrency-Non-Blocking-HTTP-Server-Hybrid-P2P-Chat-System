# High-Concurrency Non-Blocking HTTP Server & Hybrid P2P Chat System

An end-to-end distributed network ecosystem featuring a custom-built, event-driven **Non-Blocking HTTP Web Server**, a stateful **Reverse Proxy Layer** with integrated load balancing, and a decentralized **Hybrid Peer-to-Peer (P2P) Messaging Protocol**. 

Built entirely from scratch using low-level socket programming interfaces to demonstrate high-concurrency systems architecture without relying on heavy application frameworks.

---

## 🏗️ Core System Architecture

The ecosystem is decoupled into four individual architectural tiers to separate routing concerns, business logic, user interaction, and decentralized communications:

1. **Reverse Proxy Server:** The centralized edge gateway. It intercepts all incoming client HTTP requests, handles load distribution across the cluster using a Round-Robin algorithm, enforces session-validation guards, and mitigates single-point bottlenecks.
2. **Backend Server Cluster:** The centralized application state machine. It processes backend business logic, handles database interactions, processes user authentications, and serves as the **Signaling Registry** for direct peer discovery.
3. **Asynchronous Web Application:** A responsive client web interface designed with non-blocking polling loops, ensuring user interface rendering never freezes while waiting on background I/O payload streams.
4. **Chat Engine Database Layer:** A transactional data store responsible for persistent user records, credentials authentication tracking, active session tokens, and durable fallback chat histories.

---

## 🚀 Key Technical Features

### ⚡ Event-Driven Non-Blocking I/O
Instead of allocating a resource-heavy OS thread per network connection, the core HTTP server utilizes an asynchronous event loop abstraction via Python's low-level `selectors` module (utilizing OS-level `epoll` or `select` mechanisms).
* Monitors hundreds of file descriptors (sockets) concurrently within a single thread loop.
* Binds discrete callbacks to explicit `EVENT_READ` and `EVENT_WRITE` triggers.
* Eliminates heavy CPU context-switching overhead caused by slow network clients.

### 🛡️ Stateful Security & Session Management (RFC 6265)
* Implements robust, cookie-based session verification protocols directly inside the Reverse Proxy middleware.
* Successful login handshakes prompt the cluster to issue unique `Set-Cookie` tracking tokens assigned with precise Time-To-Live (TTL) tracking parameters.
* The Proxy intercepts every stateless downstream request, scanning headers for valid, unexpired credentials. Unauthorized packets undergo immediate gateway-level redirection.

### 💬 Hybrid Peer-to-Peer (P2P) Messaging Protocol
Combines centralized structural control with localized transport mesh performance to scale real-time message streams:
* **Signaling & Coordination Phase:** Clients communicate with the central Backend registry via a dedicated socket pool to request active channels, broadcast presence states, and query peer
