# CodeAlpha_BasicNetworkSniffer

Task 1 of the CodeAlpha Cyber Security Internship: a Python network sniffer built with `scapy`.

## Features
- Captures live packets on any interface
- Shows timestamp, protocol, source/destination IP and port, TCP flags, TTL, packet length
- Decodes TCP, UDP, ICMP, DNS (query names), ARP and IPv6
- Optional payload preview (`-p`) and hex view (`--hex`)
- BPF filters (e.g. `tcp port 80`)
- Saves captures to `.pcap` for Wireshark (`-w`)
- Protocol summary when capture ends

## Setup
```bash
git clone https://github.com/<your-username>/CodeAlpha_BasicNetworkSniffer.git
cd CodeAlpha_BasicNetworkSniffer
pip install -r requirements.txt
```
- **Linux/macOS:** run with `sudo`
- **Windows:** install [Npcap](https://npcap.com) and run the terminal as Administrator

## Usage
```bash
sudo python3 sniffer.py --list-ifaces            # list interfaces
sudo python3 sniffer.py                          # sniff until Ctrl+C
sudo python3 sniffer.py -c 50                    # capture 50 packets
sudo python3 sniffer.py -f "tcp port 80" -p      # HTTP with payload preview
sudo python3 sniffer.py -f "udp port 53"         # DNS only
sudo python3 sniffer.py -i wlan0 -c 100 -w out.pcap
```

## Sample output
```
[19:50:01] TCP   192.168.1.5:51522 -> 142.250.183.14:443 flags=SYN ttl=64 len=74
[19:50:01] DNS   192.168.1.5:53211 -> 8.8.8.8:53 query=example.com. ttl=64 len=72
[19:50:02] ARP request: 192.168.1.1 (aa:bb:cc:dd:ee:ff) -> 192.168.1.5
```

## How it works
1. `scapy.sniff()` captures raw frames from the network interface.
2. Each packet is passed to a callback that inspects its layers: Ethernet -> IP -> TCP/UDP/ICMP -> application data.
3. Fields are extracted and printed; counters track protocol statistics.
4. Understanding the layers shows how data flows through the network: the IP layer routes between hosts, the transport layer identifies the service via ports, and the payload carries the data.

## Learning notes
- TCP handshake is visible as SYN -> SYN+ACK -> ACK.
- HTTPS payloads are encrypted, so only HTTP (port 80) and DNS show readable content.
- ARP maps IP addresses to MAC addresses on the local network.

## Legal / ethical use
Only capture traffic on networks you own or have explicit permission to monitor. Built for educational purposes.
