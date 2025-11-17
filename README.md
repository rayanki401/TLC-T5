**Bidirectional Networking Protocol Project**

This project implements a **custom bidirectional networking protocol** between a **client** and **server**, supporting handshake, message exchange, heartbeats, and PCAP capture for troubleshooting and analysis.

🚀 Features

✅ 1. Custom Protocol Implementation

* Handshake mechanism (client/server)
* Structured message types
* Reliable message parsing
* Heartbeat mechanism

✅ 2. Client & Server Applications

* Python-based client & server
* TCP socket communication
* Shared protocol module

✅ 3. Packet Capture (PCAP)

* Capture traffic from inside the server container
* Analyze using Wireshark

✅ 4. Docker Support

* Separate Dockerfiles for server and client
* Can be deployed locally or containerized

---

📁 **Project Structure**

```
BD/
│
├── client/
│   ├── client.py
│   └── protocol/
│       ├── protocol.py
│       ├── message.py
│       └── __init__.py
│
├── server/
│   ├── server.py
│   └── protocol/
│       ├── protocol.py
│       ├── message.py
│       └── __init__.py
│
├── captures/
│   └── protocol1.pcap
│
├── Dockerfile.client
├── Dockerfile.server
└── README.md
```

⚙️ **How the Protocol Works**

🔹 Step 1 — Client connects to Server
Using standard TCP.

🔹 Step 2 — 3-Way Application-Level Handshake
Client initiates, server responds, connection established.

🔹 Step 3 — Message Exchange
Uses `Message` and `ProtocolHandler` classes.

🔹 Step 4 — Heartbeat
Monitors connection liveness.


▶️ **Run Locally**
Start Server
```bash
python server/server.py
```

Start Client
```bash
python client/client.py
```

🐳 **Docker Usage**

Build Images
```bash
docker build -f Dockerfile.server -t bd-server .
docker build -f Dockerfile.client -t bd-client .
```

Create Network
```bash
docker network create <net-name>
```

Run Server
```bash
docker run -d --name <name> --net=<net-name> -p 9000:9000 bd-server
```

Run Client
```bash
docker run --rm --name <name> --net=<net-name> -e SERVER_HOST=<server-container-name> bd-client
```

📡 **Generate PCAP File Inside Server Container**

1️⃣ Enter the Server Container

Replace `<id>` with your server container ID:

```bash
docker exec -it <id> /bin/bash
```

2️⃣ Install tcpdump & procps

Inside the container:

```bash
apt update
apt install -y tcpdump procps
```

3️⃣ Start Capturing Packets

Capture packets on the container’s `eth0` interface:

```bash
tcpdump -i eth0 -w /captures/protocol1.pcap
```

4️⃣ Stop the Server Process (Important)

To ensure the PCAP file is saved correctly:

```bash
pkill -f server.py
```

---

5️⃣ Exit the Container

```bash
exit
```


6️⃣ Copy the PCAP File to Your Host Machine

From your host:

```bash
docker cp <id>:/captures/protocol1.pcap .
```


👥 **Contributors**

| Name         | Contribution                                                                  |
| ------------ | ----------------------------------------------------------------------------- |
| Priyank  | Filtered & analyzed PCAP files using Wireshark                                |
| Deepith  | Developed major parts of client & server logic                                |
| Ravalitha | Created protocol design + message structure (`message.py`, protocol workflow) |
| Siddesh  | Dockerized server & client, created environment, generated PCAP for analysis  |
| Manisha   | Helped containerize applications and generate traffic for PCAP capture        |

---

🛠️**Future Enhancements**

* TLS encryption
* Authentication layer
* Multi-client support
* Dashboard/monitoring UI

---

📜License

For academic and research use only.

