# 📚 CHAPTER 3: TRANSPORT LAYER - COMPLETE STUDY GUIDE

**Exam Focus: Conceptual Understanding | Sections: 3.1-3.7 (excluding 3.6)**

---

## 🎯 3.1 TRANSPORT LAYER SERVICES

### **Definition (Exam):**
> The Transport Layer provides logical communication between application processes running on different hosts, extending the network layer's host-to-host delivery to process-to-process delivery through multiplexing and demultiplexing.

### **Conceptual Explanation (Roman English):**

**Transport Layer ka Role:**

Network Layer aur Application Layer ke beech bridge hai!

**Analogy:**
```
Apartment Building:

Network Layer = Delivers mail to building (host)
Transport Layer = Delivers to specific apartment (process)

Building address = IP address
Apartment number = Port number
```

---

### **Key Concepts:**

#### **1. Logical Communication:**

**Concept:**
From application's perspective, processes directly communicate, but physically:
```
Process A → Transport → Network → Link → Physical
                ↓
         (travels through network)
                ↓
Physical → Link → Network → Transport → Process B
```

Application thinks: "Direct connection!"
Reality: Multiple layers involved

---

#### **2. Process-to-Process Delivery:**

**Network Layer:**
- Delivers between **hosts** (computers)
- IP address identifies host

**Transport Layer:**
- Delivers between **processes** (applications)
- Port number identifies process

**Example:**
```
Computer A (IP: 192.168.1.10)
  - Web browser (Port 54321)
  - Email client (Port 54322)
  
Computer B (IP: 192.168.1.20)
  - Web server (Port 80)
  - Email server (Port 25)

Transport layer ensures:
Browser → Web server
Email client → Email server
```

---

#### **3. Segments:**

**Definition (Exam):**
> A segment is the Protocol Data Unit (PDU) at the transport layer, consisting of application data encapsulated with a transport layer header.

**Encapsulation:**
```
Application Layer: Message
       ↓
Transport Layer: Segment = [Header | Message]
       ↓
Network Layer: Datagram = [IP Header | Segment]
       ↓
Link Layer: Frame = [Frame Header | Datagram | Trailer]
```

---

#### **4. Transport Layer Functions:**

**Fundamental Responsibility:**
- **Multiplexing** (sender side)
- **Demultiplexing** (receiver side)

**Additional (TCP only):**
- Reliable data transfer
- Flow control
- Congestion control

---

### **IP Service Model:**

**Important:** IP is **unreliable** (best-effort)!

**IP Characteristics:**
- No guarantee of delivery
- No guarantee of order
- No guarantee of data integrity
- Packets may be lost, duplicated, or corrupted

**Transport Layer adds reliability (TCP only)!**

---

### **📊 TCP vs UDP - FUNDAMENTAL DIFFERENCES:**

**Analogy for Memory:**

**TCP = Car A (Cautious Approach):**
```
🚗 with protective shield
- Stops frequently to check ball
- Ensures ball safe and in position
- Slow but RELIABLE
- Ball reaches destination intact! ✓
```

**UDP = Car B (Reckless Approach):**
```
🚗 no shield, high speed
- Doesn't check ball
- Drives fast, no stops
- Ball may fly out!
- Fast but UNRELIABLE
- Ball might be lost ✗
```

---

### **COMPARISON TABLE:**

| Feature | UDP | TCP |
|---------|-----|-----|
| **Service Model** | Unreliable, Connectionless | Reliable, Connection-oriented |
| **Terminology** | Datagram | Segment |
| **Handshake** | No | Yes (3-way) |
| **Reliability** | No guarantees | Guaranteed delivery |
| **Ordering** | No order guarantee | In-order delivery |
| **Error Checking** | Checksum only (minimal) | Checksum + retransmission |
| **Flow Control** | No | Yes (prevent overflow) |
| **Congestion Control** | No | Yes (prevent network overload) |
| **Speed** | Fast | Slower (due to overhead) |
| **Header Size** | 8 bytes | 20 bytes (minimum) |
| **Use Cases** | DNS, Streaming, Gaming | HTTP, Email, File Transfer |
| **When to Use** | Speed > Reliability | Reliability > Speed |

---

## 🔀 3.2 MULTIPLEXING AND DEMULTIPLEXING

### **Definition (Exam):**
> Multiplexing is gathering data from multiple sockets, encapsulating with headers to create segments, and passing to network layer. Demultiplexing is delivering received segments to the correct socket based on port numbers.

### **Conceptual Explanation:**

**Real-Life Analogy:**

**Household Mail:**
```
Mail Carrier (Network Layer):
Delivers to house address (IP)

Family Members (Processes):
Each has name (port number)

Demultiplexing:
Sorting mail to correct family member

Multiplexing:
Collecting letters from family to send out
```

---

### **How It Works:**

#### **DEMULTIPLEXING (Receiver Side):**

```
Step 1: Segment arrives at host
Step 2: Examine destination port number in header
Step 3: Deliver to socket with that port
Step 4: Process reads data from socket
```

**Visual:**
```
Network Layer
     ↓
[Segment: Port 80]  [Segment: Port 25]  [Segment: Port 443]
     ↓                    ↓                    ↓
Socket (80)         Socket (25)         Socket (443)
     ↓                    ↓                    ↓
Web Server          Email Server        HTTPS Server
```

---

#### **MULTIPLEXING (Sender Side):**

```
Step 1: Process writes data to socket
Step 2: Transport layer reads from socket
Step 3: Add source port and destination port
Step 4: Create segment
Step 5: Pass to network layer
```

**Visual:**
```
Browser      Email       Chat
   ↓           ↓          ↓
Socket 1    Socket 2   Socket 3
   ↓           ↓          ↓
   └───────────┴──────────┘
              ↓
      Transport Layer
       (adds ports)
              ↓
       Network Layer
```

---

### **Port Numbers:**

**Definition (Exam):**
> Port numbers are 16-bit integers (0-65535) used to identify specific processes or services on a host for transport layer multiplexing and demultiplexing.

**Structure:**
- **Size:** 16 bits
- **Range:** 0 to 65,535

**Categories:**

**1. Well-Known Ports (0-1023):**
```
Reserved for standard services:
- Port 20/21: FTP
- Port 22: SSH
- Port 23: Telnet
- Port 25: SMTP (Email)
- Port 53: DNS
- Port 80: HTTP
- Port 110: POP3
- Port 143: IMAP
- Port 443: HTTPS
```

**2. Registered Ports (1024-49151):**
```
Registered for specific applications
- Port 3306: MySQL
- Port 5432: PostgreSQL
- Port 8080: Alternative HTTP
```

**3. Dynamic/Private Ports (49152-65535):**
```
Used by client applications
Automatically assigned by OS
Temporary ports
```

---

### **Socket Identifier:**

**Format of Segment:**
```
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|    Source Port Number (16b)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Destination Port Number (16b) |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Other Fields          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Data               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

## 📮 3.3 CONNECTIONLESS MULTIPLEXING/DEMULTIPLEXING (UDP)

### **UDP Socket Identification:**

**Definition (Exam):**
> A UDP socket is uniquely identified by a two-tuple: (destination IP address, destination port number).

**Key Point:** UDP socket identified by **destination** only!

---

### **How UDP Demultiplexing Works:**

**Process:**
```
1. Segment arrives
2. Extract destination IP and destination port
3. Find socket with that (IP, Port)
4. Deliver to that socket
```

**Important Behavior:**
```
If segments from DIFFERENT sources have:
- Same destination IP
- Same destination port

→ Delivered to SAME socket!
```

---

### **Example:**

**Server:**
```python
# UDP Server
serverSocket = socket(AF_INET, SOCK_DGRAM)
serverSocket.bind(('', 9999))  # Port 9999

# Server has ONE socket listening on port 9999
```

**Clients:**
```
Client A (IP: 192.168.1.10, Port: 54321)
Client B (IP: 192.168.1.20, Port: 54322)
Client C (IP: 192.168.1.30, Port: 54323)

All send to Server (IP: 192.168.1.100, Port: 9999)
```

**Result:**
```
All messages delivered to SAME server socket!

Server cannot distinguish clients by socket
Must check source IP and source port in segments
```

---

### **Source Port as "Return Address":**

**Concept:**

**Segment A → B:**
```
Source Port: 54321 (Client A)
Dest Port: 9999 (Server)
```

**Segment B → A (Response):**
```
Source Port: 9999 (Server)
Dest Port: 54321 (Client A)  ← Uses A's source port!
```

**Why?**
Server knows where to send response!

---

### **Binding UDP Socket:**

**Automatic Assignment:**
```python
clientSocket = socket(AF_INET, SOCK_DGRAM)
# OS automatically assigns available port (e.g., 54321)
```

**Manual Assignment:**
```python
clientSocket = socket(AF_INET, SOCK_DGRAM)
clientSocket.bind(('', 19157))  # Specific port
```

---

## 🔗 3.4 CONNECTION-ORIENTED MULTIPLEXING/DEMULTIPLEXING (TCP)

### **TCP Socket Identification:**

**Definition (Exam):**
> A TCP socket is uniquely identified by a four-tuple: (source IP, source port, destination IP, destination port).

**Key Difference from UDP:**
TCP uses **both source and destination** for identification!

---

### **Why Four-Tuple?**

**Scenario:**
```
Web Server (192.168.1.100:80)

Client A (192.168.1.10:54321) connects
Client B (192.168.1.20:54322) connects

Two DIFFERENT sockets created:
Socket 1: (192.168.1.10, 54321, 192.168.1.100, 80)
Socket 2: (192.168.1.20, 54322, 192.168.1.100, 80)

Each connection has dedicated socket!
```

---

### **TCP Connection Process:**

**Step 1: Client Initiates Connection**
```
Client sends SYN:
Source: (Client IP, Random Port)
Dest: (Server IP, Well-Known Port like 80)
```

**Step 2: Server Creates Socket**
```
"Welcoming socket" listens on port 80
For each new connection:
  Create NEW "connection socket"
  Four-tuple identifies socket
```

**Step 3: Data Exchange**
```
All segments for that connection
→ Delivered to that specific socket
```

---

### **Visual Example:**

```
Server: 192.168.1.100

Listening Socket: Port 80 (waits for connections)

Connection 1: Client A (192.168.1.10:54321)
  ↓
Connection Socket 1: (10:54321, 100:80)

Connection 2: Client B (192.168.1.20:54322)
  ↓
Connection Socket 2: (20:54322, 100:80)

Connection 3: Client A again (192.168.1.10:54330)
  ↓
Connection Socket 3: (10:54330, 100:80)
```

**Each connection = Separate socket!**

---

### **Web Server Example:**

**Multi-Threaded Server:**
```
Main Thread:
  - Welcoming socket on port 80
  - Accepts new connections
  - For each connection:
      Create new thread
      Pass connection socket to thread
      
Worker Threads:
  - Handle individual clients
  - Each has own socket
  - Serve HTTP requests/responses
```

**High-Performance Servers:**
```
One process, multiple threads (lightweight)

Benefits:
✓ Efficient resource usage
✓ Handle thousands of connections
✓ Each connection isolated
```

---

### **Persistent vs Non-Persistent HTTP:**

**Persistent HTTP:**
```
1 TCP connection = Multiple requests/responses

Client connects
Request 1 → Response 1 (same socket)
Request 2 → Response 2 (same socket)
Request 3 → Response 3 (same socket)
Connection closes

Efficient! Reuses TCP connection
```

**Non-Persistent HTTP:**
```
Each request = New TCP connection

Request 1 → New connection → Response 1 → Close
Request 2 → New connection → Response 2 → Close
Request 3 → New connection → Response 3 → Close

Overhead of creating/closing sockets frequently!
```

---

## 📦 3.5 UDP (USER DATAGRAM PROTOCOL)

### **Definition (Exam):**
> UDP is a minimalist, connectionless transport protocol that provides multiplexing/demultiplexing and basic error checking without reliability guarantees, flow control, or congestion control.

### **Conceptual Explanation:**

**UDP Philosophy:**
```
"Bare minimum transport service"

Takes message from application
Adds source/dest ports
Adds checksum
Sends to network layer
DONE!

No handshake, no reliability, no fuss!
```

---

### **UDP Characteristics:**

#### **1. Connectionless:**
```
No handshaking
No connection state
Just send and hope!
```

#### **2. Unreliable:**
```
No delivery guarantee
No ordering guarantee
Packets may:
  - Be lost
  - Arrive out of order
  - Be duplicated
```

#### **3. Minimal Overhead:**
```
Header: Only 8 bytes!
Processing: Very fast
Delay: Minimal
```

---

### **Why Use UDP?**

**When UDP is Better:**

**1. Finer Control over Sending:**
```
Application decides when to send
No TCP buffering/delays
Real-time applications need this!
```

**2. No Connection Establishment:**
```
TCP: 3-way handshake (latency!)
UDP: Immediate sending

Example:
DNS query needs quick response
TCP handshake = wasteful overhead
```

**3. No Connection State:**
```
Server doesn't maintain connection info
Can support MORE clients
Lower memory usage
```

**4. Small Header Overhead:**
```
UDP: 8 bytes
TCP: 20-60 bytes

For small messages, huge difference!
```

---

### **Applications Using UDP:**

**1. DNS (Domain Name System):**
```
Query needs quick response
Typically small (fits in 1 packet)
If no response, simply retry
```

**2. Live Streaming:**
```
Video/audio streaming
Lost frames acceptable (skip them)
Retransmission causes delays
Prefer smooth playback over perfection
```

**3. Online Gaming:**
```
Real-time, low latency critical
Player position updates
Some loss tolerable
Delay = bad gameplay!
```

**4. VoIP (Voice over IP):**
```
Voice calls (Skype, WhatsApp calls)
Slight quality loss okay
Delays = awkward conversations!
```

**5. SNMP (Network Management):**
```
Monitoring queries
Simple request-response
Low overhead important
```

---

### **UDP Segment Structure:**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                          Data / Payload                       |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Total Header: 8 bytes (4 fields × 2 bytes)**

---

### **UDP Header Fields:**

**1. Source Port (16 bits):**
- Sending process's port
- Return address
- Optional (can be 0 if no response needed)

**2. Destination Port (16 bits):**
- Receiving process's port
- Required for demultiplexing

**3. Length (16 bits):**
- Total segment length (header + data)
- Minimum: 8 bytes (header only)
- Maximum: 65,535 bytes

**4. Checksum (16 bits):**
- Error detection
- Optional in IPv4 (but used in practice)
- Mandatory in IPv6

---

### **UDP CHECKSUM:**

**Definition (Exam):**
> UDP checksum is calculated using 1's complement arithmetic on all 16-bit words in the segment (including pseudo-header) to detect bit errors in transmission.

#### **How Checksum Works:**

**Sender Side:**

**Step 1: Treat segment as sequence of 16-bit integers**
```
Example data:
0110011001100000
0101010101010101
1000111100001100
```

**Step 2: Add all 16-bit words**
```
  0110011001100000
+ 0101010101010101
------------------
  1011101110110101

+ 1000111100001100
------------------
 10100101010111001
 ↑
Overflow (carry)
```

**Step 3: Wrap around overflow**
```
 0100101010111001
+                1  (carry)
------------------
 0100101010111010
```

**Step 4: Take 1's complement (flip all bits)**
```
Sum:       0100101010111010
Checksum:  1011010101000101  ← This goes in checksum field
```

---

**Receiver Side:**

**Step 1: Add all words (including checksum)**
```
  0110011001100000
+ 0101010101010101
+ 1000111100001100
+ 1011010101000101  (checksum)
------------------
  1111111111111111
```

**Step 2: Check result**
```
If sum = 1111111111111111 (all 1s):
  → No error detected ✓
  
If sum ≠ 1111111111111111:
  → Error detected! ✗
  → Discard segment or pass with warning
```

---

### **Why Error Detection in UDP?**

**Question:** If UDP is unreliable anyway, why checksum?

**Answer:**
1. **Link layer** may not always check errors
2. **In-memory errors** possible (bit flips in RAM)
3. **Best-effort detection** better than nothing
4. **Safety measure** across protocol layers

**Important:**
```
UDP detects errors but does NOT correct them!

Options on error:
1. Discard segment silently
2. Pass to application with warning

Application decides what to do!
```

---

### **UDP with Application-Level Reliability:**

**QUIC Protocol Example:**

UDP itself unreliable, but applications can add:
- Acknowledgments
- Retransmission
- Flow control
- Congestion control

**Built directly into application!**

**Benefits:**
- Flexibility (customize for needs)
- Avoid TCP's head-of-line blocking
- Faster than TCP (no OS overhead)
- Still get reliability where needed

---

## 🔧 3.7 TCP (TRANSMISSION CONTROL PROTOCOL)

### **Definition (Exam):**
> TCP is a connection-oriented, reliable transport protocol that provides full-duplex, point-to-point communication with flow control, congestion control, and in-order byte-stream delivery.

### **Key TCP Characteristics:**

#### **1. Connection-Oriented:**
```
Handshake required before data transfer
3-way handshake establishes connection
Connection state maintained
```

#### **2. Reliable:**
```
Guaranteed delivery (retransmit if lost)
In-order delivery (reorder if needed)
Error-free (checksum + retransmission)
```

#### **3. Full-Duplex:**
```
Data flows in BOTH directions simultaneously

A → B (data)
A ← B (data)

Same connection!
```

#### **4. Point-to-Point:**
```
One sender, one receiver
No multicast (1-to-many)
No broadcast (1-to-all)
```

---

### **TCP Data Transfer Mechanism:**

**Buffers:**

```
Sender Side:
Application → Send Buffer → TCP → Network

Receiver Side:
Network → TCP → Receive Buffer → Application
```

**Send Buffer:**
- Holds data waiting to be sent
- TCP controls when to actually transmit
- Based on: window size, congestion, MSS

**Receive Buffer:**
- Holds received data
- Application reads at its own pace
- TCP manages buffer to prevent overflow

---

### **Maximum Segment Size (MSS):**

**Definition (Exam):**
> MSS is the maximum amount of application-layer data in a TCP segment, excluding TCP and IP headers.

**Typical Calculation:**
```
Ethernet MTU = 1500 bytes

IP Header = 20 bytes
TCP Header = 20 bytes
Total Headers = 40 bytes

MSS = MTU - Headers
MSS = 1500 - 40 = 1460 bytes
```

**Why MSS Important?**
```
Fit in single link-layer frame
Avoid IP fragmentation
Fragmentation = inefficiency!
```

**Example:**
```
Application sends 5,000 bytes

Segmentation:
Segment 1: 1,460 bytes
Segment 2: 1,460 bytes
Segment 3: 1,460 bytes
Segment 4: 620 bytes
Total: 4 segments
```

---

### **TCP Segment Structure:**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Data  |       |C|E|U|A|P|R|S|F|                               |
|Offset | Rsrvd |W|C|R|C|S|S|Y|I|         Window Size           |
|       |       |R|E|G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if any)                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Minimum Header: 20 bytes**

---

### **TCP Header Fields:**

**1. Source Port & Destination Port (16 bits each):**
- Multiplexing/demultiplexing
- Same as UDP

**2. Sequence Number (32 bits):**

**Definition (Exam):**
> Sequence number identifies the position of the first byte of data in this segment within the sending byte stream.

**Important:** Sequence number is for **BYTES**, not segments!

**Example:**
```
File: 500,000 bytes
MSS: 1,000 bytes

Segment 1: Seq = 0 (bytes 0-999)
Segment 2: Seq = 1000 (bytes 1000-1999)
Segment 3: Seq = 2000 (bytes 2000-2999)
...
```

**3. Acknowledgment Number (32 bits):**

**Definition (Exam):**
> Acknowledgment number is the sequence number of the next byte the receiver expects to receive, implementing cumulative acknowledgment.

**Cumulative ACK:**
```
ACK = 1000 means:
"I received all bytes up to 999"
"Next, I expect byte 1000"
```

**Example:**
```
Sender sends:
Segment 1: Seq = 0, Data = 1000 bytes

Receiver sends:
ACK = 1000 ("Got 0-999, expecting 1000 next")
```

**4. Header Length / Data Offset (4 bits):**
- Header length in 32-bit words
- Similar to IP IHL
- Typical: 5 (20 bytes)

**5. Flags (6 bits):**

**SYN (Synchronize):**
- Connection setup
- First packet in 3-way handshake

**ACK (Acknowledgment):**
- ACK field valid
- Set in almost all segments (except first SYN)

**FIN (Finish):**
- Connection termination
- No more data from sender

**RST (Reset):**
- Abort connection
- Error condition

**PSH (Push):**
- Push data immediately to application

**URG (Urgent):**
- Urgent data present

**6. Receive Window / rwnd (16 bits):**
- Flow control
- Available buffer space at receiver
- Tells sender: "Don't send more than this!"

**7. Checksum (16 bits):**
- Error detection
- Mandatory (unlike UDP)
- Same algorithm as UDP

**8. Urgent Pointer (16 bits):**
- Used with URG flag
- Indicates urgent data location

**9. Options (Variable):**
- MSS negotiation
- Window scaling
- Timestamps
- SACK (Selective ACK)

---

### **Sequence and ACK Numbers Example:**

**Scenario: Client sends data to Server**

```
Initial:
Client Seq = 42
Server Seq = 79

Step 1: Client → Server
Segment:
  SYN = 1
  Seq = 42
  (No data, connection setup)

Step 2: Server → Client
Segment:
  SYN = 1, ACK = 1
  Seq = 79
  ACK = 43 ("Expecting byte 43")
  
Step 3: Client → Server (connection established)
Segment:
  ACK = 1
  Seq = 43
  ACK = 80
  Data: "GET /index.html" (8 bytes)

Step 4: Server → Client
Segment:
  ACK = 1
  Seq = 80
  ACK = 51 ("Got bytes 43-50, expecting 51")
  Data: "<html>..." (500 bytes)

Step 5: Client → Server
Segment:
  ACK = 1
  Seq = 51
  ACK = 580 ("Got bytes 80-579, expecting 580")
```

**Key Points:**
- Seq number = position of YOUR data
- ACK number = next byte YOU expect from peer
- Both sides track independent sequence numbers

---

### **3.7.1 ROUND-TRIP TIME (RTT) ESTIMATION**

**Definition (Exam):**
> RTT estimation is the process of measuring and calculating the time for a segment to travel from sender to receiver and back, used to set appropriate timeout values.

**Why RTT Important?**
```
TCP needs to know:
"If I don't get ACK, when should I assume packet lost?"

Too short timeout → Unnecessary retransmissions
Too long timeout → Slow recovery from loss

Need accurate RTT estimate!
```

---

#### **Sample RTT:**

**Definition:**
Time from sending segment to receiving ACK for that segment

**Measurement:**
```
Send segment at time T1
Receive ACK at time T2
SampleRTT = T2 - T1
```

**Important Considerations:**
1. Measure for ONE segment at a time
2. Don't measure retransmitted segments (ambiguity!)
3. Typically choose unacknowledged segment
4. SampleRTT varies (network conditions)

**Example:**
```
Sample 1: 100 ms
Sample 2: 105 ms
Sample 3: 95 ms
Sample 4: 110 ms
Sample 5: 90 ms

Varies due to:
- Network congestion
- Router queues
- Changing routes
```

---

#### **Estimated RTT (EWMA):**

**Definition (Exam):**
> EstimatedRTT is a smoothed average of SampleRTT values calculated using Exponentially Weighted Moving Average (EWMA), giving more weight to recent samples.

**Formula:**
```
EstimatedRTT = (1 - α) × EstimatedRTT + α × SampleRTT

Where α = 0.125 (recommended)
```

**Why EWMA?**
- Smooth out fluctuations
- Recent samples influence more
- Old samples slowly decay
- Adapts to changing conditions

**Example Calculation:**
```
Initial EstimatedRTT = 100 ms
α = 0.125

SampleRTT = 120 ms
EstimatedRTT = 0.875 × 100 + 0.125 × 120
             = 87.5 + 15
             = 102.5 ms

Next SampleRTT = 90 ms
EstimatedRTT = 0.875 × 102.5 + 0.125 × 90
             = 89.7 + 11.25
             = 100.95 ms
```

---

#### **DevRTT (RTT Deviation):**

**Definition (Exam):**
> DevRTT measures how much SampleRTT typically deviates from EstimatedRTT, helping set appropriate safety margins for timeout intervals.

**Formula:**
```
DevRTT = (1 - β) × DevRTT + β × |SampleRTT - EstimatedRTT|

Where β = 0.25 (recommended)
```

**Purpose:**
Measure variability/fluctuation

**Example:**
```
EstimatedRTT = 100 ms
SampleRTT = 120 ms
Deviation = |120 - 100| = 20 ms

DevRTT = 0.75 × DevRTT + 0.25 × 20
```

---

#### **Timeout Interval:**

**Definition (Exam):**
> TimeoutInterval is the duration TCP waits for an acknowledgment before retransmitting a segment, calculated based on EstimatedRTT and DevRTT.

**Formula:**
```
TimeoutInterval = EstimatedRTT + 4 × DevRTT
```

**Why "+ 4 × DevRTT"?**
```
Safety margin!

If RTT highly variable (high DevRTT):
→ Larger safety margin
→ Less premature timeouts

If RTT stable (low DevRTT):
→ Smaller safety margin
→ Faster retransmission when needed
```

**Example:**
```
EstimatedRTT = 100 ms
DevRTT = 10 ms

TimeoutInterval = 100 + 4 × 10
                = 100 + 40
                = 140 ms
```

**Initial Value:**
Recommended: 1 second (conservative)

**Timeout Doubling:**
```
If timeout occurs:
1. Retransmit segment
2. Double TimeoutInterval (exponential backoff)
3. Prevents repeated timeouts in congested network

Next ACK received:
→ Recalculate using formula
```

---

### **3.7.2 RELIABLE DATA TRANSFER (TCP)**

**TCP Reliability Mechanisms:**

#### **1. Checksums:**
- Detect bit errors
- Mandatory
- Corrupted segments discarded

#### **2. Sequence Numbers:**
- Track byte position
- Detect lost segments
- Detect duplicates

#### **3. Acknowledgments:**
- Confirm receipt
- Cumulative ACKs
- Trigger retransmission if missing

#### **4. Timers:**
- Detect segment loss
- Timeout → Retransmit

#### **5. Fast Retransmit:**
- Use duplicate ACKs
- Faster than timeout

---

### **TCP Sender Events:**

**Event 1: Data from Application**

```
1. Create segment with sequence number
2. If timer NOT running:
     Start timer
     Timer for oldest unACKed segment
3. Send segment to IP
```

**Event 2: Timeout**

```
1. Retransmit OLDEST unACKed segment
2. Restart timer
```

**Event 3: ACK Received**

```
1. Extract ACK number (y)
2. If y > SendBase:
     Update SendBase = y
     (All bytes < y now acknowledged)
3. If unACKed segments remain:
     Restart timer
   Else:
     Stop timer
```

---

### **TCP ACK Generation (Receiver):**

| Event | TCP Receiver Action |
|-------|---------------------|
| In-order segment, all prior ACKed | Delayed ACK (wait 500ms for next segment) |
| In-order segment, one delayed ACK pending | Immediately send cumulative ACK |
| Out-of-order segment (gap detected) | Immediately send duplicate ACK |
| Segment fills gap | Immediately send ACK |

---

### **Duplicate ACKs:**

**Definition (Exam):**
> A duplicate ACK is an acknowledgment that repeats the same ACK number as a previous acknowledgment, indicating potential segment loss.

**When Generated:**
```
Expected: Seq 1000
Received: Seq 2000 (gap!)

Action: Send ACK 1000 (duplicate!)
"Still expecting 1000, got wrong segment"
```

**Example Scenario:**
```
Sender sends:
Seg 1: Seq = 1000
Seg 2: Seq = 2000
Seg 3: Seq = 3000
Seg 4: Seq = 4000

Seg 2 LOST in network

Receiver receives:
Seg 1: Seq 1000 → ACK 2000 ✓
Seg 3: Seq 3000 → ACK 2000 (Dup 1)
Seg 4: Seq 4000 → ACK 2000 (Dup 2)

Sender receives:
ACK 2000
ACK 2000 (Dup ACK 1)
ACK 2000 (Dup ACK 2)
```

---

### **Fast Retransmit:**

**Definition (Exam):**
> Fast retransmit is a TCP mechanism that retransmits a presumably lost segment after receiving three duplicate ACKs, without waiting for timeout.

**Trigger:** 3 duplicate ACKs

**Why 3?**
```
1 Dup ACK → Might be reordering
2 Dup ACKs → Likely reordering
3 Dup ACKs → Almost certainly loss!

Retransmit immediately, don't wait for timeout!
```

**Process:**
```
1. Receive ACK x
2. Receive ACK x (Dup 1)
3. Receive ACK x (Dup 2)
4. Receive ACK x (Dup 3) ← TRIGGER!
5. Retransmit segment starting at x
```

**Benefits:**
✓ Faster recovery than timeout
✓ Reduces retransmission delay
✓ Improves throughput

---

### **TCP vs GBN vs SR:**

**TCP is Hybrid:**

**From GBN:**
- Cumulative ACKs
- Single timer

**From SR:**
- Buffering out-of-order segments (optional)
- Selective retransmission (only lost segment)

**TCP's Approach:**
```
Cumulative ACKs (like GBN)
But retransmits only suspected lost segment (like SR)
Not all unACKed segments (unlike GBN)

Result: Efficient hybrid!
```

---

### **3.7.3 FLOW CONTROL**

**Definition (Exam):**
> Flow control is a mechanism that matches the sender's transmission rate to the receiver's reading rate to prevent buffer overflow at the receiver.

**The Problem:**

```
Receiver:
  Network → Receive Buffer → Application

If Application reads slowly:
  Buffer fills up!
  
If Sender keeps sending:
  Buffer overflows!
  Data lost!
```

---

### **Flow Control vs Congestion Control:**

| Feature | Flow Control | Congestion Control |
|---------|--------------|-------------------|
| **Problem** | Receiver overwhelmed | Network overwhelmed |
| **Protects** | Receiver buffer | Network routers |
| **Signal** | rwnd (receive window) | Loss events |
| **Scope** | End-to-end (2 hosts) | Network-wide |
| **Goal** | Match sending to reading | Match sending to capacity |

---

### **Receive Window (rwnd):**

**Definition:**
Available space in receiver's buffer

**Calculation:**
```
rwnd = RcvBuffer - [LastByteRcvd - LastByteRead]

Where:
RcvBuffer = Total buffer size
LastByteRcvd = Highest byte received
LastByteRead = Last byte read by application
```

**Example:**
```
RcvBuffer = 10,000 bytes
LastByteRcvd = 5,000
LastByteRead = 3,000

rwnd = 10,000 - [5,000 - 3,000]
     = 10,000 - 2,000
     = 8,000 bytes available
```

---

### **How Flow Control Works:**

**Receiver Side:**
```
1. Calculate rwnd
2. Include rwnd in every ACK segment
3. "I have rwnd bytes available"
```

**Sender Side:**
```
1. Receive rwnd from peer
2. Ensure:
   LastByteSent - LastByteAcked ≤ rwnd
   
3. Limit sending to rwnd
```

**Visual:**
```
Sender's Send Buffer:
[ACKed | Sent but unACKed | Can send | Future]
        |<── At most rwnd ──>|

Receiver's Receive Buffer:
[Read by app | Received | Available (rwnd) ]
```

---

### **Zero Window Problem:**

**Scenario:**
```
Receiver buffer full:
rwnd = 0

Receiver sends: ACK with rwnd = 0
"Stop sending!"

Sender stops.

Now what if application reads data?
Buffer space available, but sender doesn't know!
```

**Solution:**
```
When rwnd = 0:
Sender continues sending segments with 1 byte data
(Probe segments)

Purpose:
Receiver responds with current rwnd
Sender learns when space available
Connection unblocked!
```

---

### **3.7.4 TCP CONNECTION MANAGEMENT**

**Three Phases:**
1. **Connection Establishment** (3-way handshake)
2. **Data Transfer**
3. **Connection Termination** (4-way handshake)

---

#### **THREE-WAY HANDSHAKE:**

**Definition (Exam):**
> The three-way handshake is TCP's connection establishment procedure involving SYN, SYN-ACK, and ACK segments to synchronize sequence numbers and establish connection.

**Steps:**

**Step 1: Client → Server (SYN)**
```
Client sends:
  SYN = 1
  Seq = client_isn (Initial Sequence Number)
  
"I want to connect. My starting seq is client_isn"
```

**Step 2: Server → Client (SYN-ACK)**
```
Server sends:
  SYN = 1, ACK = 1
  Seq = server_isn
  ACK = client_isn + 1
  
"OK! My starting seq is server_isn. I got your SYN"
```

**Step 3: Client → Server (ACK)**
```
Client sends:
  ACK = 1
  Seq = client_isn + 1
  ACK = server_isn + 1
  (Can contain data!)
  
"ACK! Connection established"
```

**Visual:**
```
Client                               Server
  |                                    |
  | SYN (Seq=100)                     |
  |─────────────────────────────────→ |
  |                                    |
  |          SYN-ACK (Seq=300, ACK=101)|
  | ←─────────────────────────────────|
  |                                    |
  | ACK (Seq=101, ACK=301)            |
  |─────────────────────────────────→ |
  |                                    |
  [ESTABLISHED]              [ESTABLISHED]
```

**After Handshake:**
- Full-duplex connection ready
- Both sides know each other's sequence numbers
- Can send data immediately

---

#### **CONNECTION TERMINATION:**

**Four-Way Close:**

**Step 1: Client → Server (FIN)**
```
Client:
  FIN = 1
  Seq = x
  
"I'm done sending"
```

**Step 2: Server → Client (ACK)**
```
Server:
  ACK = 1
  ACK = x + 1
  
"OK, I received your FIN"
```

**Step 3: Server → Client (FIN)**
```
Server (when done sending):
  FIN = 1
  Seq = y
  
"I'm also done sending"
```

**Step 4: Client → Server (ACK)**
```
Client:
  ACK = 1
  ACK = y + 1
  
"OK, closing connection"
```

**Visual:**
```
Client                               Server
  |                                    |
  | FIN (Seq=500)                     |
  |─────────────────────────────────→ |
  |                                    |
  |                    ACK (ACK=501)  |
  | ←─────────────────────────────────|
  |                                    |
  |           FIN (Seq=1000)          |
  | ←─────────────────────────────────|
  |                                    |
  | ACK (ACK=1001)                    |
  |─────────────────────────────────→ |
  |                                    |
[CLOSED]                          [CLOSED]
```

**Why 4 Steps?**
```
TCP is full-duplex!
Each direction closed independently

Client done sending ≠ Server done sending
Each side closes its sending direction
```

---

## 📝 QUICK REVISION SUMMARY

### **3.1 Transport Layer Services:**
- Process-to-process delivery (vs host-to-host)
- Multiplexing/demultiplexing
- TCP: Reliable, connection-oriented
- UDP: Unreliable, connectionless

### **3.2 Multiplexing/Demultiplexing:**
- Port numbers identify processes
- Well-known (0-1023), Registered, Dynamic
- Mux: Gather from sockets → network
- Demux: Deliver from network → sockets

### **3.3 UDP Demux:**
- Two-tuple: (dest IP, dest port)
- Same destination → same socket
- Source port = return address

### **3.4 TCP Demux:**
- Four-tuple: (src IP, src port, dest IP, dest port)
- Each connection = unique socket
- Connection sockets vs welcoming socket

### **3.5 UDP:**
- 8-byte header (minimal)
- Connectionless, unreliable
- Checksum (1's complement)
- Use: DNS, streaming, gaming, VoIP

### **3.7 TCP:**
- 20-byte header (minimum)
- Connection-oriented (3-way handshake)
- Sequence numbers (byte-level)
- Cumulative ACKs
- RTT estimation (EWMA)
- Timeout = EstimatedRTT + 4×DevRTT
- Fast retransmit (3 dup ACKs)
- Flow control (rwnd)
- Connection termination (4-way)

---

## 🎯 EXAM PREPARATION TIPS

### **1. Key Comparisons:**
- UDP vs TCP
- Multiplexing vs Demultiplexing
- Flow Control vs Congestion Control
- GBN vs SR vs TCP
- 2-tuple (UDP) vs 4-tuple (TCP)

### **2. Important Formulas:**
- `EstimatedRTT = (1-α)×EstimatedRTT + α×SampleRTT`
- `DevRTT = (1-β)×DevRTT + β×|SampleRTT-EstimatedRTT|`
- `TimeoutInterval = EstimatedRTT + 4×DevRTT`
- `rwnd = RcvBuffer - [LastByteRcvd - LastByteRead]`
- `MSS = MTU - 40 bytes`

### **3. Critical Concepts:**
- Port number ranges
- Checksum calculation
- Sequence vs ACK numbers
- Cumulative acknowledgment
- Fast retransmit trigger
- 3-way handshake steps
- Flow control mechanism

### **4. Practice Questions:**
- Why UDP for DNS?
- How does TCP detect loss (2 ways)?
- Explain 3-way handshake
- Calculate timeout interval
- How does flow control prevent overflow?

---

## ✅ END OF CHAPTER 3

**Fantastic! Chapter 3 complete! 🎉**

**You now understand:**
✓ Transport layer fundamentals
✓ Multiplexing and demultiplexing
✓ UDP protocol and applications
✓ TCP reliability mechanisms
✓ Flow control and connection management

**Progress: 4/6 Chapters Done!**
- ✅ Chapter 6 ✓
- ✅ Chapter 5 ✓
- ✅ Chapter 4 ✓
- ✅ Chapter 3 ✓
- ⏳ Chapter 2 (next?)
- ⏳ Chapter 1

**Almost there! Best of luck! 🚀📚**
