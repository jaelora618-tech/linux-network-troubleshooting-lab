# Linux Network Troubleshooting & Packet Analysis Lab

## Project Overview

This project documents a hands-on networking lab using Ubuntu Linux in Oracle VirtualBox. I practiced identifying network configuration, testing connectivity, investigating DNS, monitoring network interface statistics, and analyzing TCP/IP packets.

The goal was to develop practical networking and troubleshooting skills relevant to IT support and entry-level cybersecurity roles.

## Lab Environment

- **Operating System:** Ubuntu Linux
- **Virtualization:** Oracle VirtualBox
- **Network Mode:** NAT
- **Network Interface:** `enp0s3`
- **Ubuntu IPv4 Address:** `10.0.2.15`
- **Default Gateway:** `10.0.2.2`
- **Tools:** Linux terminal, iproute2, ping, nslookup, tcpdump

## Network Troubleshooting

### 1. Identifying the Network Configuration

I used the following commands to inspect the Ubuntu virtual machine's network configuration:

```bash
ip -br addr
ip route
ip link
```

These commands helped me identify the VM's network interface, IP address, default gateway, and interface status.

### 2. Testing Network Connectivity

I tested connectivity using ICMP echo requests:

```bash
ping -c 4 10.0.2.2
ping -c 4 1.1.1.1
ping -c 4 google.com
```

**Results:** All three tests returned four packets transmitted, four received, and 0% packet loss.

The tests verified connectivity to the virtual gateway, an external IP address, and a domain name.

### 3. Investigating DNS

I used:

```bash
nslookup google.com
```

The DNS lookup returned an IP address for the domain, although an additional NXDOMAIN response was observed. A successful subsequent ping confirmed that the domain could resolve and respond at the time of testing.

### 4. Inspecting Network Interface Statistics

I ran:

```bash
ip -s link show enp0s3
```

This displayed receive (RX) and transmit (TX) counters, including bytes, packets, errors, and dropped packets.

## Network Packet Analysis

I used tcpdump to capture and analyze network communication:

```bash
sudo tcpdump -n -i enp0s3 -c 10
```

I then saved a packet capture for further analysis:

```bash
sudo tcpdump -i enp0s3 -c 30 -w ~/network-project/evidence/network-capture.pcap
```

To read and filter the saved packets:

```bash
sudo tcpdump -nn -r ~/network-project/evidence/network-capture.pcap 'tcp'
```

### Observations

- Identified ARP requests and replies used for local IPv4-to-MAC address resolution.
- Observed NTP traffic associated with time synchronization.
- Identified TCP source and destination addresses and ports.
- Analyzed TCP flags including ACK (`[.]`) and PSH/ACK (`[P.]`).
- Observed network connections using destination port 443, commonly associated with HTTPS.

## Skills Practiced

- Linux command-line networking
- TCP/IP fundamentals
- Network interface configuration
- Gateway and connectivity testing
- DNS troubleshooting
- ICMP packet-loss interpretation
- ARP and TCP packet analysis
- Basic network traffic capture

## Project Status

**In progress:** Baseline connectivity testing and packet analysis are complete.

Remaining tasks include documenting a controlled network outage and recovery, adding sanitized screenshots, and reviewing evidence before publication.

## Security Notes

The packet capture contains networking metadata and is kept locally rather than uploaded publicly. Screenshots will be reviewed for sensitive information before being added to the repository.


## Network Failure Simulation and Recovery

### Objective
The purpose of this exercise was to simulate a network outage in an Ubuntu Linux virtual machine, identify the problem, restore connectivity, and verify that the network was working again.

### Step 1: Verify Network Connectivity

Before disconnecting the network, I checked the interface status and tested connectivity to the default gateway.

Commands used:

```bash
ip -br link
ping -c 4 10.0.2.2
```

Results:
- Network interface `enp0s3` was UP.
- 4 packets transmitted.
- 4 packets received.
- 0% packet loss.

These results confirmed that the virtual machine could communicate with its default gateway.

### Step 2: Simulate a Network Failure

I intentionally disconnected the virtual network adapter using Oracle VirtualBox.

Navigation:
Devices > Network > Connect Network Adapter

After disconnecting the adapter, I checked the network interface.

```bash
ip -br link
```

The network interface showed DOWN.

I then attempted to ping the default gateway.

```bash
ping -c 4 10.0.2.2
```

Ubuntu returned the following error:

```text
ping: connect: Network is unreachable
```

This indicated that Ubuntu had no usable network route to the destination.

### Step 3: Troubleshoot the Network Issue

I investigated the problem by checking the interface status and reviewing the network configuration.

I determined that the intentionally disconnected VirtualBox network adapter was responsible for the loss of connectivity.

### Step 4: Restore Network Connectivity

To resolve the issue, I reconnected the virtual network adapter through VirtualBox.

I then repeated the connectivity test.

```bash
ping -c 4 10.0.2.2
```

Results:
- 4 packets transmitted.
- 4 packets received.
- 0% packet loss.

The successful test confirmed that connectivity to the default gateway had been restored.

### Skills Demonstrated

- Linux network troubleshooting
- Network interface diagnostics
- VirtualBox network configuration
- ICMP connectivity testing
- Identifying network failures
- Troubleshooting and resolving connectivity issues
- Verifying successful network recovery

### Conclusion

This hands-on exercise demonstrated the process of identifying, troubleshooting, and resolving a network connectivity issue in an Ubuntu Linux environment.

By intentionally creating a network failure and restoring connectivity, I practiced troubleshooting techniques commonly used in IT help desk and entry-level cybersecurity positions.
