👤 Project Overview & Team
Topic: Implement a Non-blocking HTTP Server and Hybrid Chat Application.

Team Members: Vo Minh Quan, Vo Trung Thanh, Nguyen Viet Le Manh Quan, and Nguyen Hieu.

Project Repository: https://github.com/quanmanhle/Computer_Network-HCMUT-PO.git

🏗️ 1. Multi-Tier System Architecture
The application is cleanly divided into four distinct components to handle client-server traffic, data processing, and user interactions:

Proxy Server: Acts as the entry point for all web requests. It intercepts traffic, manages client sessions, enforces authentication guards, and performs load-balancing (e.g., Round-Robin) to forward tasks to the backend cluster.

Backend Server Cluster: A dedicated processing tier that executes business logic, handles database interactions, and acts as a central signaling server to orchestrate user connections.

Web Application: A responsive interface that users interact with. It establishes asynchronous pipelines to request data and listen for incoming messages without freezing the web UI.

Chat Engine Database: Stores user profiles, authentication salts, session state maps, and persistent chat logs.

🔄 2. Core Technical Mechanics
🔹 Non-Blocking I/O Architecture
Unlike traditional servers that spawn a new blocking thread for every connection, this system utilizes Python’s low-level selectors module (leveraging OS-level epoll or select mechanisms under the hood).

EVENT_READ Event Loop: The server registers its listening socket to an ongoing event loop. When a new connection arrives or data is ready to be pulled from an existing client socket, an event is fired asynchronously.

High Concurrency: This allows a single execution thread to handle hundreds of concurrent user requests smoothly, preventing slow network clients from tying up server resources.

🍪 Session & Security Management
Cookie-based Access Control: Follows RFC 6265 standards. Upon a successful login attempt, the backend returns a unique Set-Cookie session token with an assigned Time-To-Live (TTL).

State Verification: Every subsequent HTTP request intercepted by the Proxy scans for a valid cookie string. If missing or expired, the proxy issues an immediate redirect back to the login page.

💬 3. Hybrid Peer-to-Peer (P2P) Chat Protocol
The messaging suite uses a hybrid design combining central orchestration with distributed client networking to ensure real-time transmission:

Signaling Phase (Client-Server): When a user wants to initiate a chat or join a channel, they first talk to the Backend/Signaling server via a persistent socket to query peer network locations and update availability maps.

Peer Discovery: The backend handles channel management, broadcasting online states, and exchanging network coordinates (IP and assigned ports) between clients.

Direct P2P Data Channels (Peer-to-Peer): Once peers know where each other are, they open direct TCP/UDP sockets to one another. Messages fly directly between user devices, bypassing the server completely. This heavily reduces central bandwidth and latency.

📂 How would you like to proceed with this file?
Since you are likely compiling a README for GitHub, drafting CV content, or preparing for your project defense/viva, I can help you structure it:

Option A: Write a structured README.md description detailing this non-blocking architecture, complete with a features list and layout.

Option B: Draft a highly professional CV Project Experience bullet-point list highlighting your software engineering contributions.

Option C: Generate potential Q&A defense questions your lecturer might ask regarding non-blocking I/O vs multi-threading, socket handling, or P2P data flow.
