# Complete Guide to the dig Command on Ubuntu

## Introduction

`dig` (Domain Information Groper) is a powerful command-line tool for querying DNS (Domain Name System) servers. It's essential for troubleshooting DNS issues, verifying DNS configurations, and understanding how domain names resolve to IP addresses.

## Installation

On Ubuntu, `dig` is part of the `dnsutils` package. Install it with:

```bash
sudo apt update
sudo apt install dnsutils
```

Verify the installation:

```bash
dig -v
```

## Basic Syntax

```bash
dig [@server] [domain] [query-type] [options]
```

## Basic Usage

### Simple DNS Lookup

The most basic use case - looking up a domain's IP address:

```bash
dig example.com
```

This returns the A record (IPv4 address) for the domain.

### Understanding the Output

A typical `dig` output contains several sections:

```
; <<>> DiG 9.18.x <<>> example.com
;; global options: +cmd
;; Got answer:
;; QUESTION SECTION:
;example.com.                   IN      A

;; ANSWER SECTION:
example.com.            86400   IN      A       93.184.216.34

;; Query time: 45 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Mon Dec 15 10:30:00 EST 2025
;; MSG SIZE  rcvd: 56
```

**Key sections:**
- **QUESTION SECTION**: The query you made
- **ANSWER SECTION**: The DNS response with TTL (Time To Live) and the IP address
- **Query time**: How long the query took
- **SERVER**: Which DNS server responded

## Common Query Types

### A Record (IPv4 Address)

```bash
dig example.com A
```

### AAAA Record (IPv6 Address)

```bash
dig example.com AAAA
```

### MX Record (Mail Exchange)

Find mail servers for a domain:

```bash
dig example.com MX
```

### NS Record (Name Servers)

Find authoritative name servers:

```bash
dig example.com NS
```

### CNAME Record (Canonical Name)

Check for domain aliases:

```bash
dig www.example.com CNAME
```

### TXT Record

View text records (often used for verification):

```bash
dig example.com TXT
```

### SOA Record (Start of Authority)

Get zone information:

```bash
dig example.com SOA
```

### ANY Record

Request all available records:

```bash
dig example.com ANY
```

## Useful Options

### Short Answer (+short)

Get just the IP address without extra information:

```bash
dig example.com +short
```

Output: `93.184.216.34`

### No Comments (+nocomments)

Remove comment lines:

```bash
dig example.com +nocomments
```

### No Statistics (+nostats)

Hide timing and server information:

```bash
dig example.com +nostats
```

### No Question Section (+noquestion)

Remove the question section:

```bash
dig example.com +noquestion
```

### No Answer Section (+noanswer)

Hide the answer (useful when checking authority):

```bash
dig example.com +noanswer
```

### Clean Output (+noall +answer)

Get only the answer section:

```bash
dig example.com +noall +answer
```

### Trace DNS Resolution (+trace)

Show the full path of DNS resolution from root servers:

```bash
dig example.com +trace
```

This is extremely useful for debugging DNS delegation issues.

## Specifying DNS Servers

### Use a Specific DNS Server

Query Google's public DNS:

```bash
dig @8.8.8.8 example.com
```

Query Cloudflare's DNS:

```bash
dig @1.1.1.1 example.com
```

Query your local DNS server:

```bash
dig @192.168.1.1 example.com
```

## Advanced Usage

### Reverse DNS Lookup

Find the domain name for an IP address:

```bash
dig -x 93.184.216.34
```

### Batch Queries

Query multiple domains from a file:

```bash
dig -f domains.txt +short
```

Where `domains.txt` contains one domain per line.

### Set Custom Port

If DNS is running on a non-standard port:

```bash
dig @8.8.8.8 -p 5353 example.com
```

### TCP Instead of UDP

Force TCP protocol:

```bash
dig +tcp example.com
```

### Check DNSSEC

Verify DNSSEC signatures:

```bash
dig example.com +dnssec
```

### Set Timeout

Specify query timeout in seconds:

```bash
dig +time=2 example.com
```

### Set Number of Retries

```bash
dig +tries=3 example.com
```

## Practical Examples

### Check if DNS Propagation is Complete

```bash
dig @8.8.8.8 newdomain.com +short
dig @1.1.1.1 newdomain.com +short
```

### Find All MX Records with Priority

```bash
dig gmail.com MX +noall +answer
```

### Verify SPF Records

```bash
dig example.com TXT +short | grep spf
```

### Check Multiple Record Types

```bash
dig example.com A example.com AAAA example.com MX
```

### Compare DNS Responses from Different Servers

```bash
echo "Google DNS:" && dig @8.8.8.8 example.com +short
echo "Cloudflare DNS:" && dig @1.1.1.1 example.com +short
```

## Troubleshooting Tips

### No Response or Timeout

- Check your internet connection
- Verify the DNS server is reachable: `ping 8.8.8.8`
- Try a different DNS server
- Check firewall rules blocking port 53

### SERVFAIL Status

Indicates the DNS server encountered an error:

```bash
dig example.com +trace
```

Use trace to see where the resolution fails.

### NXDOMAIN Status

The domain doesn't exist or has no DNS records.

### Check Local DNS Configuration

View your current DNS servers:

```bash
cat /etc/resolv.conf
```

## Comparison with Other Tools

- **nslookup**: Older tool, less detailed output, deprecated but still available
- **host**: Simpler output, good for quick lookups
- **dig**: Most detailed, best for troubleshooting and professional use

## Quick Reference

| Command | Purpose |
|---------|---------|
| `dig example.com` | Basic A record lookup |
| `dig example.com +short` | Show only the IP |
| `dig example.com MX` | Mail server lookup |
| `dig @8.8.8.8 example.com` | Use specific DNS server |
| `dig -x 1.2.3.4` | Reverse DNS lookup |
| `dig example.com +trace` | Trace DNS resolution path |
| `dig example.com ANY` | Get all record types |
| `dig example.com +noall +answer` | Clean answer only |

## Conclusion

The `dig` command is an essential tool for anyone working with DNS, from system administrators to web developers. Its detailed output and flexible options make it invaluable for troubleshooting DNS issues and understanding how domain name resolution works. Practice with these examples to become proficient in DNS querying and troubleshooting.
