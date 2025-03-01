# DNS Resolver (Iterative & Recursive)

## Overview

This project implements a DNS resolver that supports both **iterative** and **recursive** DNS resolution using Python and the `dnspython` library. The program allows users to query domain names and retrieve their corresponding IP addresses either by iteratively resolving the domain through the DNS hierarchy or by using a recursive resolver.

## Features

- **Iterative DNS Resolution:** Queries root DNS servers first, then progresses through TLD (Top-Level Domain) servers and authoritative servers until the domain's IP address is found.
- **Recursive DNS Resolution:** Uses the system's default resolver (e.g., Google DNS, ISP resolver) to fetch the result directly.
- **Error Handling:** Handles cases like timeouts, unreachable servers, and non-existent domains.
- **Command-Line Interface:** Allows users to specify the resolution mode and the domain name.

## Requirements

Before running the program, ensure you have the following installed:

- Python 3.x
- `dnspython` library (install using `pip`)

### Installation

Run the following command to install the required dependencies:

```bash
pip install dnspython
```

## Usage

The program accepts command-line arguments to specify the resolution mode and domain name.

### Running the Program

To run the DNS resolver, use the following command format:

```bash
python3 dnsresolver.py <mode> <domain>
```

Where:

- `<mode>` is either `iterative` or `recursive`
- `<domain>` is the domain name to resolve (e.g., `google.com`)

### Example Usage

#### Iterative DNS Lookup:

```bash
python3 dnsresolver.py iterative youtube.com
```

**Expected Output Format:**

```
[Iterative DNS Lookup] Resolving youtube.com
[DEBUG] Querying ROOT server (198.41.0.4) - SUCCESS
Extracted NS hostname: l.gtld-servers.net.
Extracted NS hostname: j.gtld-servers.net.
Extracted NS hostname: h.gtld-servers.net.
Extracted NS hostname: d.gtld-servers.net.
Extracted NS hostname: b.gtld-servers.net.
Extracted NS hostname: f.gtld-servers.net.
Extracted NS hostname: k.gtld-servers.net.
Extracted NS hostname: m.gtld-servers.net.
Extracted NS hostname: i.gtld-servers.net.
Extracted NS hostname: g.gtld-servers.net.
Extracted NS hostname: a.gtld-servers.net.
Extracted NS hostname: c.gtld-servers.net.
Extracted NS hostname: e.gtld-servers.net.
Resolved l.gtld-servers.net. to 192.41.162.30
Resolved j.gtld-servers.net. to 192.48.79.30
Resolved h.gtld-servers.net. to 192.54.112.30
Resolved d.gtld-servers.net. to 192.31.80.30
Resolved b.gtld-servers.net. to 192.33.14.30
Resolved f.gtld-servers.net. to 192.35.51.30
Resolved k.gtld-servers.net. to 192.52.178.30
Resolved m.gtld-servers.net. to 192.55.83.30
Resolved i.gtld-servers.net. to 192.43.172.30
Resolved g.gtld-servers.net. to 192.42.93.30
Resolved a.gtld-servers.net. to 192.5.6.30
Resolved c.gtld-servers.net. to 192.26.92.30
Resolved e.gtld-servers.net. to 192.12.94.30
[DEBUG] Querying TLD server (192.41.162.30) - SUCCESS
Extracted NS hostname: ns2.google.com.
Extracted NS hostname: ns1.google.com.
Extracted NS hostname: ns3.google.com.
Extracted NS hostname: ns4.google.com.
Resolved ns2.google.com. to 216.239.34.10
Resolved ns1.google.com. to 216.239.32.10
Resolved ns3.google.com. to 216.239.36.10
Resolved ns4.google.com. to 216.239.38.10
[DEBUG] Querying AUTH server (216.239.34.10) - SUCCESS
[SUCCESS] youtube.com -> 74.125.141.91
Time taken: 0.520 seconds
```

#### Recursive DNS Lookup:

```bash
python3 dnsresolver.py recursive youtube.com
```

**Expected Output Format:**

```
[Recursive DNS Lookup] Resolving youtube.com
[SUCCESS] youtube.com -> ns4.google.com.
[SUCCESS] youtube.com -> ns1.google.com.
[SUCCESS] youtube.com -> ns2.google.com.
[SUCCESS] youtube.com -> ns3.google.com.
[SUCCESS] youtube.com -> 172.217.203.91
Time taken: 0.013 seconds
```

## Code Structure

### `dnsresolver.py`

- **send\_dns\_query(server, domain)**: Sends a DNS query to a specified server.
- **extract\_next\_nameservers(response)**: Extracts nameserver (NS) records from the authority section and resolves them to IP addresses.
- **iterative\_dns\_lookup(domain)**: Performs iterative DNS resolution, starting from root servers and moving down the hierarchy.
- **recursive\_dns\_lookup(domain)**: Uses the system's DNS resolver to fetch the domain's IP address.
- **Main Execution Block**: Parses command-line arguments and invokes the appropriate resolution function.

## Error Handling

The program handles:

- **Timeouts**: If a server doesn't respond, an error is logged.
- **Invalid Domains**: If the domain does not exist, an appropriate error is displayed.
- **Unreachable Servers**: If a nameserver cannot be contacted, the program attempts the next available server.

## Notes

- The iterative resolver directly interacts with DNS hierarchy and might take longer than the recursive resolver.
- The recursive resolver relies on an external resolver (like Google DNS or ISP's resolver) for fast lookups.

---
