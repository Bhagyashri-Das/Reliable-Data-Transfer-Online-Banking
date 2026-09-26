# Reliable Data Transfer System for Online Banking

A Computer Networks Project-Based Learning (PBL) project demonstrating a reliable client-server communication system for online banking using **Cisco Packet Tracer** and **CRC-based error detection**. The project demonstrates networking, routing, connectivity testing, CRC-based error detection, and receiver-side frame acceptance/rejection using Cisco Packet Tracer.

## 📌 Project Overview

This project presents a simplified network model for reliable transmission of banking transaction data between client systems and a bank server.

The network is designed using **Cisco Packet Tracer** and consists of:

- Four client PCs
- Two switches
- Two routers
- A serial connection between the routers
- A bank server
- Three IP networks
- CRC-based error detection
- Receiver-side frame acceptance/rejection logic
- ARQ/retransmission discussed as the recovery mechanism

The project demonstrates how transmission errors can be detected using **Cyclic Redundancy Check (CRC)** and how a receiver can decide whether to accept or reject a received frame.

> **Note:** This is an academic networking model. It is not intended to represent a complete production banking system.

## 🎯 Objectives

The main objectives of this project are to:

1. Design a client-server banking network using Cisco Packet Tracer.
2. Implement CRC-based error detection for transmitted data.
3. Demonstrate receiver-side detection of a corrupted frame.
4. Recalculate and verify the CRC at the receiver.
5. Determine whether a received frame should be accepted or rejected.
6. Explain how retransmission and error-control mechanisms can be used to recover from detected errors.

## 🏗️ Network Architecture

The project uses the following communication path:

```text
Client PCs
    |
    v
  Switch 0
    |
    v
  Router 1
    |
    |  Serial Link
    |
    v
  Router 2
    |
    v
  Switch 1
    |
    v
Bank Server
```

### Packet Flow

```text
Client PC
 ↓
Switch0
 ↓
Router1 Fa0/0
 ↓
Router1 Serial2/0
 ↓
Router2 Serial2/0
 ↓
Router2 Fa0/0
 ↓
Switch1
 ↓
Bank Server
```

## 🌐 IP Addressing

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC0 / Client 1 | FastEthernet0 | `192.168.1.1` | `255.255.255.0` | `192.168.1.5` |
| PC1 / Client 2 | FastEthernet0 | `192.168.1.2` | `255.255.255.0` | `192.168.1.5` |
| PC2 / Client 3 | FastEthernet0 | `192.168.1.3` | `255.255.255.0` | `192.168.1.5` |
| PC3 / Client 4 | FastEthernet0 | `192.168.1.4` | `255.255.255.0` | `192.168.1.5` |
| Router1 | FastEthernet0/0 | `192.168.1.5` | `255.255.255.0` | — |
| Router1 | Serial2/0 | `192.168.2.1` | `255.255.255.252` | — |
| Router2 | Serial2/0 | `192.168.2.2` | `255.255.255.252` | — |
| Router2 | FastEthernet0/0 | `192.168.3.2` | `255.255.255.0` | — |
| Bank Server | FastEthernet0 | `192.168.3.1` | `255.255.255.0` | `192.168.3.2` |

### Networks

| Network | Address | Purpose |
|---|---|---|
| Client LAN | `192.168.1.0/24` | Connects the client PCs to Router1 |
| Router-to-Router Link | `192.168.2.0/30` | Point-to-point connection between Router1 and Router2 |
| Bank Server LAN | `192.168.3.0/24` | Connects Router2 to the bank server |

## 🔐 CRC Error Detection

The project uses the following binary data and generator polynomial:

```text
Data = 1101011011
Generator = 10011
```

Since the generator has a degree of 4, four zeros are appended to the original data:

```text
Original Data = 1101011011
Padded Data   = 11010110110000
```

Modulo-2 division is then performed:

```text
11010110110000 ÷ 10011
```

The resulting CRC is:

```text
CRC = 1110
```

Therefore, the transmitted frame becomes:

```text
Original Data + CRC
1101011011 + 1110
Transmitted Frame = 11010110111110
```

## 📥 Receiver Verification

At the receiver, the complete received frame is divided by the same generator polynomial:

```text
Received Frame = 11010110111110
Generator = 10011
```

For the correct frame:

```text
CRC Remainder = 0000
```

Therefore:

```text
0000 → NO ERROR DETECTED → FRAME ACCEPTED
```

## ⚠️ Error Detection Demonstration

To demonstrate error detection, the project considers a corrupted frame:

```text
Correct Frame:
11010110111110

Corrupted Frame:
01010110111110
```

The corrupted frame produces:

```text
CRC Remainder = 0100
```

Since the remainder is non-zero:

```text
0100 → ERROR DETECTED → FRAME REJECTED
```

### Acceptance/Rejection Rule

| CRC Remainder | Meaning | Decision |
|---|---|---|
| `0000` | No error detected by CRC | **ACCEPT** |
| Non-zero | Transmission error detected | **REJECT / Retransmit** |

CRC detects transmission errors but does not itself correct the corrupted data. The project describes an ARQ/retransmission mechanism as a possible recovery method.

## 🧪 Connectivity Testing

End-to-end connectivity can be tested from Client 1 to the Bank Server.

### Command

```text
ping 192.168.3.1
```

Successful ICMP replies demonstrate routed IP connectivity between the client and bank server.

The Packet Tracer Simulation Mode can also be used to observe the packet travelling through:

```text
Client → Switch → Router1 → Serial Link → Router2 → Switch → Bank Server
```

## 📊 Results

| Test Condition | CRC Result | Decision |
|---|---:|---|
| Correct frame `11010110111110` | `0000` | **ACCEPT** |
| Corrupted frame `01010110111110` | `0100` | **REJECT** |

### Summary

- The client LAN uses `192.168.1.0/24`.
- The bank server LAN uses `192.168.3.0/24`.
- Router1 and Router2 communicate through the `192.168.2.0/30` serial network.
- The calculated CRC for `1101011011` using generator `10011` is `1110`.
- The transmitted frame is `11010110111110`.
- A correct frame produces a zero CRC remainder and is accepted.
- The demonstrated corrupted frame produces a non-zero remainder and is rejected.

## 📸 Project Screenshots

The repository includes screenshots taken directly from the Cisco Packet Tracer project.

Recommended screenshots:

### Network Topology

```text
screenshots/topology.png
```

### Connectivity Test

Screenshot showing the successful ping from Clients to the Bank Server:

```text
screenshots/successful-ping.png
```

### Simulation Mode

```text
screenshots/simulation-mode.png
```

## 🛠️ Tools and Technologies

- **Cisco Packet Tracer** — network topology design and simulation
- **Computer Networks concepts**
- **CRC (Cyclic Redundancy Check)** — transmission error detection
- **IP addressing and subnetting**
- **Routing**
- **ICMP / Ping** — connectivity verification
- **ARQ / Retransmission concepts** — discussed as an error recovery mechanism

## 📂 Repository Contents

### README.md
Contains an overview of the project, network architecture, CRC calculation, testing procedure, results, limitations, and future scope.

### Online Banking.pkt
Contains the Cisco Packet Tracer network project.

### PBL REPORT.pdf
Contains the detailed project report, including the network design, IP addressing, CRC calculation, receiver verification, results, limitations, and conclusion.

### CN-PBL-PPT.pptx
Contains the project presentation.

## ▶️ How to Run the Project

### 1. Install Cisco Packet Tracer

Install a compatible version of Cisco Packet Tracer.

### 2. Open the Packet Tracer File

Open:

```text
project/Online Banking.pkt
```

### 3. Inspect the Network

Verify the following:

- Four client PCs
- Client-side switch
- Router1
- Router2
- Serial connection between routers
- Server-side switch
- Bank Server

### 4. Verify IP Configuration

Check that the devices use the IP addressing scheme documented in this README and the project report.

### 5. Test Connectivity

From Client 1, test the Bank Server:

```text
ping 192.168.3.1
```

### 6. Use Simulation Mode

Packet Tracer's Simulation Mode can be used to observe the packet path from the client through the routers to the bank server.

## ⚠️ Limitations

This project is a simplified academic demonstration and does not represent a complete production banking architecture.

The current model:

- Does not implement a complete banking application.
- Does not implement encryption.
- Does not implement user authentication.
- Uses CRC for error detection rather than error correction.
- Describes ARQ/retransmission as a recovery mechanism rather than implementing it as a complete application protocol.
- Uses ICMP ping to verify network connectivity rather than performing an actual banking transaction such as a fund transfer or balance update.

## 🚀 Future Scope

Possible extensions to the project include:

- Implementing ARQ-based automatic retransmission.
- Comparing CRC with checksum and Hamming code.
- Adding encryption and authentication.
- Developing real-time banking functions such as balance inquiry and fund transfer.
- Measuring packet loss, delay, bandwidth, and error rates.
- Expanding the network to multiple branches and redundant links.
- Exploring cloud-based deployment and high-availability designs.

## 👥 Project Team

**Brainware University**  
**Department of Computer Science & Engineering – Cyber Security & Data Science**  
**Course:** Computer Networks  
**Course Code:** BTD50112

| Member | Student Code |
|---|---|
| Bhagyashree Das | `BWU/BTD/24/063` |
| Debraj Mandal | `BWU/BTD/24/139` |
| Krishanu Das | `BWU/BTD/24/137` |
| Sayantan Paul | `BWU/BTD/24/144` |

**Supervisor:**  
Dr. Arnab Kundu  
Assistant Professor, CSE-CS & DS

## 📚 References

1. Cisco Networking Academy — Cisco Packet Tracer.
2. Forouzan, B. A. — *Data Communications and Networking*.
3. Kurose, J. F. & Ross, K. W. — *Computer Networking: A Top-Down Approach*.
4. Peterson, L. L. & Davie, B. S. — *Computer Networks: A Systems Approach*.
5. Project-related reference videos listed in the project documentation.
