# HDFS Overview

## 🔍 What is HDFS?

HDFS (Hadoop Distributed File System) is the primary storage system used by Hadoop. It is designed to store massive amounts of data reliably and to stream it at high bandwidth to user applications. HDFS runs across multiple machines and uses a master/slave architecture.

---

## 🧱 Core Components

### 1. **Client**

- The user or application that interacts with HDFS.
- Sends commands like read/write to the NameNode.
- Transfers actual data to/from DataNodes.

### 2. **NameNode (Master)**

- Stores **metadata** about the file system: file names, directory structure, block locations, permissions.
- Does **not** store the actual data.
- Maintains the **namespace** (hierarchical file system structure).

### 3. **DataNodes (Workers)**

- Store the actual **data blocks**.
- Serve read/write requests from clients.
- Periodically send **heartbeats** to the NameNode.

---

## 📦 File Storage in HDFS

When a file is written to HDFS:

1. The file is split into blocks (default size: 128MB).
2. Each block is stored on multiple DataNodes (default replication: 3).
3. The NameNode tracks which DataNodes hold each block.

---

## 🧠 NameNode Metadata Management

### 📁 **Namespace**

- The hierarchical directory structure maintained in memory by the NameNode.

### 📷 **FsImage (File System Image)**

- A **snapshot** of the entire HDFS namespace stored on disk.
- Used to quickly restore metadata during NameNode startup.

### ✏️ **EditLog**

- A **log file** that records every change (write, delete, rename, etc.) to the namespace after the last FsImage.
- Stored on disk to ensure durability.

### 🔄 **Checkpointing**

- Combines the current FsImage and EditLog into a new FsImage.
- Clears the EditLog.
- Often handled by a **Secondary NameNode** or **Checkpoint Node**.

---

## 🛡️ Crash Recovery

If the NameNode crashes:

1. On restart, it loads the last FsImage from disk.
2. Replays the EditLog to restore the latest state.
3. Result: Complete recovery of namespace metadata.

---

## 📋 Summary Table

| Component | Role                      | Stores What                 |
| --------- | ------------------------- | --------------------------- |
| Client    | Sends requests            | Nothing                     |
| NameNode  | Manages metadata          | Namespace, FsImage, EditLog |
| DataNode  | Stores actual data blocks | File blocks                 |
| FsImage   | Snapshot of metadata      | On NameNode disk            |
| EditLog   | Log of recent changes     | On NameNode disk            |

---

## 📌 Notes

- HDFS is optimized for **large files** and **write-once-read-many** use cases.
- Designed for **fault-tolerance** and **high throughput**, not low-latency access.
- Data is replicated across multiple nodes for durability.

---

> HDFS is the foundation of the Hadoop ecosystem, ensuring reliable, distributed storage at scale.
