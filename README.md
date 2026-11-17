# C++ Deep Packet Inspection (DPI) Packet Analyzer

A C++17-based network traffic analyzer that reads PCAP files, parses network packets, identifies protocols and applications, extracts visible domain information, and applies traffic-filtering rules.

The project demonstrates network protocol parsing, Deep Packet Inspection (DPI), flow tracking, and multithreaded packet processing.

## Features

- **PCAP File Processing:** Read and process packets from offline PCAP captures.
- **Packet Parsing:** Extract Ethernet, IPv4, TCP and UDP header information.
- **Deep Packet Inspection:** Inspect application-layer information available in captured packets.
- **Domain Extraction:** Extract HTTP Host headers and TLS Server Name Indication (SNI) when available.
- **Application Classification:** Identify supported applications using domain-based signatures.
- **Flow Tracking:** Track connections using the five-tuple.
- **Multithreaded Processing:** Distribute packet-processing work across Load Balancer and Fast Path threads.
- **Traffic Filtering:** Apply IP, application and domain-based rules during offline PCAP processing.
- **Statistics and Reporting:** Generate packet counts, protocol breakdowns, application statistics and processing reports.

## Architecture

```text
              Input PCAP File
                     |
                     v
                PCAP Reader
                     |
                     v
                Packet Parser
                     |
                     v
              Load Balancers
                     |
                     v
               Fast Paths
                     |
          +----------+----------+
          |          |          |
          v          v          v
       Flow       DPI/SNI     Rule
      Tracking   Detection   Manager
          |          |          |
          +----------+----------+
                     |
              Forward / Drop
                     |
                     v
               Output PCAP
                     |
                     v
                Statistics
```

## Technology Stack

| Technology | Purpose |
|---|---|
| C++17 | Core implementation |
| STL | Data structures and utilities |
| Threads, Mutexes | Multithreaded processing and synchronization |
| PCAP | Offline packet capture format |
| Python | Test PCAP generation |
| Git and GitHub | Version control and project hosting |

## Project Structure

```text
Packet-Analyzer/
│
├── include/
│   ├── connection_tracker.h
│   ├── dpi_engine.h
│   ├── fast_path.h
│   ├── load_balancer.h
│   ├── packet_parser.h
│   ├── pcap_reader.h
│   ├── platform.h
│   ├── rule_manager.h
│   ├── sni_extractor.h
│   ├── thread_safe_queue.h
│   └── types.h
│
├── src/
│   ├── connection_tracker.cpp
│   ├── dpi_engine.cpp
│   ├── dpi_mt.cpp
│   ├── fast_path.cpp
│   ├── load_balancer.cpp
│   ├── main.cpp
│   ├── main_dpi.cpp
│   ├── main_simple.cpp
│   ├── main_working.cpp
│   ├── packet_parser.cpp
│   ├── pcap_reader.cpp
│   ├── rule_manager.cpp
│   ├── sni_extractor.cpp
│   └── types.cpp
│
├── generate_test_pcap.py
├── test_dpi.pcap
├── CMakeLists.txt
├── WINDOWS_SETUP.md
└── README.md
```

## Build and Run

### Requirements

- Windows
- C++17-compatible compiler (GCC recommended)
- Git
- Python 3 (for generating test captures)

### Compile the Multithreaded Version

Open CMD in the project directory:

```cmd
g++ -std=c++17 -O2 -I include -o dpi_engine.exe src/dpi_mt.cpp src/pcap_reader.cpp src/packet_parser.cpp src/sni_extractor.cpp src/types.cpp
```

### Run the Analyzer

```cmd
dpi_engine.exe test_dpi.pcap output_test.pcap
```

The program reads the input PCAP, processes its packets, displays a report and writes the resulting PCAP file.

## Verified Test Run

The project has successfully processed the supplied sample capture.

| Metric | Observed result |
|---|---:|
| Total packets | 77 |
| Total bytes | 5,738 |
| TCP packets | 73 |
| UDP packets | 4 |
| Forwarded packets | 77 |
| Dropped packets | 0 |

The run also identified domains including GitHub, YouTube, Google, Facebook, Instagram, Discord and Spotify.

These results demonstrate successful execution on the supplied test capture; they do not establish complete classification accuracy or live network enforcement.

## Understanding the Filtering Feature

The current implementation processes saved PCAP files.

When a packet matches a configured filtering rule, the processing pipeline can exclude it from the output PCAP.

**Important:** This is offline filtering. It does not directly block websites or interrupt your laptop's live internet connection.

Live packet capture and operating-system-level traffic enforcement are future development goals.

## Current Limitations

- Processes saved PCAP files rather than capturing live network traffic.
- Domain identification depends on information visible in the captured packets.
- HTTPS inspection is limited by encryption and privacy technologies such as Encrypted Client Hello.
- Application classification depends on the implemented signatures.
- Offline filtering does not enforce network restrictions on the host operating system.

## Future Improvements

- Live packet capture using Npcap.
- Actual network traffic enforcement using Windows Filtering Platform (WFP).
- Improved application and domain classification.
- Better support for modern protocols, including QUIC and HTTP/3.
- Persistent filtering rules.
- Real-time traffic statistics dashboard.
- Additional testing and performance evaluation.

## Learning Areas

This project provides practical exposure to:

- Computer Networking
- Ethernet, IPv4, TCP and UDP
- PCAP file formats
- Deep Packet Inspection
- TLS Handshake and SNI
- Network Flow Tracking
- Multithreading and Thread Synchronization
- Producer-Consumer Architecture
- Traffic Filtering

## Documentation

See [WINDOWS_SETUP.md](WINDOWS_SETUP.md) for Windows-specific setup instructions.

## Project Status

**Current stage:** Offline PCAP-based packet analysis and multithreaded processing.

Live traffic capture and actual network-level blocking have not yet been implemented.

---

Developed and extended as a C++ cybersecurity and networking project.

**Author:** Gautam Kumar