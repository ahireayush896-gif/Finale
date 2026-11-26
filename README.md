# Finale: Autonomous Robotics Navigation Gateway

Finale is an advanced Autonomous Robotics Navigation Gateway engineered to manage high-precision sensor pinouts, real-time kinematic telemetry protocols, and deterministic hardware synchronization for autonomous mobile robots (AMRs). Built with low-latency drivers and robust ROS2/CAN-bus bridging, Finale delivers lightning-fast multi-sensor fusion, actuator control, and fail-safe navigation routing.

---

## 🚀 Key Features

- **Blazing Fast In-Memory Storage**: Optimized memory layout and lock-free concurrent hash maps delivering sub-millisecond read/write latencies.
- **Automatic Distributed Sharding**: Consistent hashing ring (`MurmurHash3`) distributes keys dynamically across cluster nodes, minimizing rebalancing overhead during cluster scaling.
- **Replication & Fault Tolerance**: Configurable synchronous/asynchronous master-replica replication with automated leader election and heartbeat node health monitoring.
- **Adaptive Eviction Policies**: Supports LRU (Least Recently Used), LFU (Least Frequently Used), and TTL-based expiration algorithms.
- **Python Client & Async API**: Fully asynchronous Python client supporting connection pooling, pipelining, and automatic failover.

---

## 🏗️ System Architecture

Finale operates as a decentralized peer-to-peer (or master-replica) distributed cluster. Each node runs an independent query execution engine and memory manager.

```
                  +-----------------------------------+
                  |         Python Client App         |
                  |  (Async Pool / Consistent Hash)   |
                  +-----------------+-----------------+
                                    |
                    TCP / Binary Protocol & Pipelining
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

1. **Consistent Hashing Ring**: The key space is mapped onto a $2^{32}-1$ virtual ring. Nodes are assigned multiple virtual nodes (vnodes) to guarantee uniform distribution and prevent hotspots.
2. **Memory Engine & Eviction**: Custom arena allocator and slab memory management reduce garbage collection pauses. Background sweeper threads reclaim expired keys based on TTL.
3. **Transport Layer**: High-performance asynchronous socket server using non-blocking I/O multiplexing and a compact binary serialization format.

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

Start a standalone or clustered cache node using the CLI runner:

```bash
# Start Node 1 on port 6379 with clustering enabled
python -m finale.server --host 127.0.0.1 --port 6379 --cluster-seed 127.0.0.1:6379
```

### 3. Python Client Integration

Connect to your Finale cluster and perform distributed caching operations using the asynchronous Python client:

```python
import asyncio
from finale.client import FinaleClient

async def main():
    # Initialize the client connected to seed nodes
    client = FinaleClient(nodes=["127.0.0.1:6379", "127.0.0.1:6380"])
    await client.connect()

    # Set a key with a 60-second TTL
    await client.set("user:1001", {"name": "Alice", "role": "engineer"}, ttl=60)

    # Retrieve the value
    user = await client.get("user:1001")
    print("Retrieved User:", user)

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
