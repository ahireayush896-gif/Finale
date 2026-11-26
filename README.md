# Finale: Autonomous Quantum Encryption Gateway

Finale is an advanced, ultra-secure Autonomous Quantum Encryption Gateway designed for high-throughput microservice architectures and real-time data pipelines. Built with cutting-edge post-quantum cryptography, automated key rotation protocols, and comprehensive security audit trails, Finale provides military-grade in-memory key-value encryption, secure sharding, replication, and tamper-proof fault tolerance.

---

## 🚀 Key Features

- **Autonomous Quantum Encryption**: Post-quantum cryptographic algorithms securing data at rest and in transit against quantum computing threats.
- **Automated Key Rotation Protocols**: Dynamic, zero-downtime cryptographic key rotation backed by cryptographic hardware security modules (HSMs) and policy-driven schedulers.
- **Comprehensive Security Audits**: Real-time telemetry, tamper-evident audit logging, and automated compliance verification checks.
- **Automatic Distributed Sharding**: Consistent hashing ring (`MurmurHash3`) distributes keys dynamically across cluster nodes, minimizing rebalancing overhead during cluster scaling.
- **Python Client & Async API**: Fully asynchronous Python client supporting connection pooling, secure pipelining, and automatic failover.

---

## 🏗️ System Architecture

Finale operates as a decentralized peer-to-peer quantum encryption gateway cluster. Each node runs an independent query execution engine, cryptographic processor, and memory manager.

```
                  +-----------------------------------+
                  |         Python Client App         |
                  |  (Async Pool / Consistent Hash)   |
                  +-----------------+-----------------+
                                    |
                    Secure TLS 1.3 & Quantum-Safe Tunnel
                                    |
         +--------------------------+--------------------------+
         |                          |                          |
         v                          v                          v
+-----------------+        +-----------------+        +-----------------+
|  Finale Node 1  | <----> |  Finale Node 2  | <----> |  Finale Node 3  |
|  (Shard A - Master)     |  (Shard B - Master)     |  (Shard C - Master)     |
|   + Replica B   |        |   + Replica C   |        |   + Replica A   |
+-----------------+        +-----------------+        +-----------------+
         ^                          ^                          ^
         +--------------------------+--------------------------+
                       Gossip Protocol / Heartbeat
```

### Core Architectural Components

1. **Quantum Encryption Engine**: Implements lattice-based and hash-based post-quantum cryptographic primitives alongside automated key rotation schedulers.
2. **Consistent Hashing Ring**: The key space is mapped onto a $2^{32}-1$ virtual ring. Nodes are assigned multiple virtual nodes (vnodes) to guarantee uniform distribution and prevent hotspots.
3. **Transport & Audit Layer**: High-performance asynchronous socket server with end-to-end TLS 1.3 encryption and immutable audit trail generation.

---

## 🛠️ Setup & Installation

### Prerequisites
- Python 3.9+
- `pip` or Poetry

### 1. Installation

Clone the repository and install dependencies in your virtual environment:

```bash
git clone https://github.com/assimilate-automation/finale.git
cd finale
pip install -r requirements.txt
```

### 2. Running a Finale Node

Start a standalone or clustered encryption gateway node using the CLI runner:

```bash
# Start Node 1 on port 6379 with quantum clustering & key rotation enabled
python -m finale.server --host 127.0.0.1 --port 6379 --cluster-seed 127.0.0.1:6379 --enable-quantum-encryption
```

### 3. Python Client Integration

Connect to your Finale cluster and perform secure encryption & caching operations using the asynchronous Python client:

```python
import asyncio
from finale.client import FinaleClient

async def main():
    # Initialize the client connected to seed nodes with quantum security
    client = FinaleClient(nodes=["127.0.0.1:6379", "127.0.0.1:6380"], quantum_secure=True)
    await client.connect()

    # Set an encrypted key with a 60-second TTL
    await client.set("user:1001", {"name": "Alice", "role": "engineer"}, ttl=60)

    # Retrieve the value
    user = await client.get("user:1001")
    print("Retrieved Secure User:", user)

    # Delete a key
    await client.delete("user:1001")
    
    await client.close()

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 🧪 Running Tests

To verify the installation and run the unit/integration test suite, use `pytest`:

```bash
pytest tests/
```

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
