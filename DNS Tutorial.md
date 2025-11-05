
# DNS Tutorial

## **1. Introduction to DNS**

The **Domain Name System (DNS)** is like the phonebook of the Internet. It translates human-readable domain names (like `example.com`) into machine-readable IP addresses (`192.0.2.1`), allowing computers to locate each other over the network.

### **Why DNS is important**

* Humans prefer names, not numbers.
* IP addresses can change; DNS allows flexible mapping.
* Provides services like load balancing, email routing, and security filtering.

---

## **2. Basic DNS Concepts**

### **2.1 Domain Names**

* **FQDN (Fully Qualified Domain Name)**: `www.example.com.`
* **TLD (Top-Level Domain)**: `.com`, `.org`, `.net`
* **Second-level domain**: `example` in `example.com`
* **Subdomain**: `www` in `www.example.com`

### **2.2 DNS Record Types**

| Record Type | Purpose                                   |
| ----------- | ----------------------------------------- |
| **A**       | Maps domain to IPv4 address               |
| **AAAA**    | Maps domain to IPv6 address               |
| **CNAME**   | Canonical name (alias for another domain) |
| **MX**      | Mail exchange (email server)              |
| **NS**      | Name server for the domain                |
| **PTR**     | Reverse DNS (IP → domain)                 |
| **TXT**     | Text information (e.g., SPF, DKIM)        |
| **SRV**     | Service location record                   |

### **2.3 DNS Zones**

* **Forward zone**: Maps domain → IP
* **Reverse zone**: Maps IP → domain
* **Authoritative DNS server**: Provides official answers for a domain.
* **Recursive DNS server**: Resolves queries on behalf of a client, potentially querying other servers.

---

## **3. How DNS Resolution Works**

1. User types `www.example.com` in a browser.
2. The browser asks the **local DNS resolver** (usually ISP’s server).
3. If resolver doesn’t know the answer:

   * Queries **root servers** (`.`)
   * Queries **TLD servers** (e.g., `.com`)
   * Queries **authoritative server** for `example.com`
4. Resolver caches the answer and returns the IP to the browser.
5. Browser connects to IP address.

**Diagram of DNS Query Flow:**

```text
Client → Recursive Resolver → Root → TLD → Authoritative → IP
```

---

## **4. Configuring DNS Records**

### **4.1 Example using a DNS provider (e.g., Cloudflare, GoDaddy)**

* **A record**: `example.com → 203.0.113.10`
* **CNAME record**: `www.example.com → example.com`
* **MX record**: `example.com → mail.example.com`
* **TXT record**: `example.com → "v=spf1 include:_spf.example.com ~all"`

### **4.2 Example using BIND (Linux DNS Server)**

**Zone file for `example.com`:**

```text
$TTL 86400
@   IN  SOA ns1.example.com. admin.example.com. (
        2025092201 ; serial
        3600       ; refresh
        1800       ; retry
        604800     ; expire
        86400 )    ; minimum

; Name servers
@   IN  NS  ns1.example.com.
@   IN  NS  ns2.example.com.

; A records
@   IN  A   203.0.113.10
www IN  A   203.0.113.10

; MX records
@   IN  MX 10 mail.example.com.

; CNAME records
ftp IN  CNAME www.example.com.
```

---

## **5. DNS Caching**

* **Client-side cache**: Browser or OS may cache DNS.
* **Resolver cache**: Recursive server caches query results.
* **TTL (Time To Live)**: Determines how long a record is cached.

**Example:**
If `A` record has `TTL=3600`, clients may cache it for 1 hour before re-querying.

---

## **6. Common DNS Tools and Commands**

| Tool         | Purpose               | Example                    |
| ------------ | --------------------- | -------------------------- |
| `nslookup`   | Query DNS records     | `nslookup example.com`     |
| `dig`        | Advanced DNS query    | `dig example.com A +short` |
| `host`       | Quick lookups         | `host www.example.com`     |
| `ping`       | Test connectivity     | `ping example.com`         |
| `traceroute` | Trace route to server | `traceroute example.com`   |

## **Example: Querying MX records**

```bash
dig example.com MX +short
```

---

## **7. Reverse DNS (PTR Records)**

* Maps IP address back to a hostname.
* Often used in email systems to reduce spam.
* Example: `203.0.113.10 → mail.example.com`

---

## **8. DNS Security**

* **DNSSEC**: DNS Security Extensions provide authentication of DNS responses.
* **Common attacks**:

  * Cache poisoning
  * DDoS attacks on authoritative servers
* **Mitigation**:

  * Enable DNSSEC
  * Use secure recursive resolvers (e.g., Cloudflare 1.1.1.1, Google 8.8.8.8)
  * Rate-limiting and firewalls for authoritative servers

---

## **9. Advanced Topics**

### **9.1 Split DNS**

* Different DNS responses internally vs. externally.
* Useful in corporate networks.

### **9.2 Dynamic DNS (DDNS)**

* Automatically updates DNS records when IP changes.
* Commonly used for home networks or IoT devices.

### **9.3 Load Balancing and Failover**

* Use multiple `A` or `AAAA` records for the same domain.
* DNS can distribute traffic across servers.

### **9.4 Anycast DNS**

* Same IP address advertised from multiple locations.
* Improves redundancy and reduces latency.

---

## **10. Troubleshooting DNS**

1. **Check DNS records**

   ```bash
   dig example.com ANY
   nslookup example.com
   ```

2. **Check propagation**

   * Use online tools like `whatsmydns.net`
3. **Clear local cache**

   * Windows: `ipconfig /flushdns`
   * Linux: `sudo systemd-resolve --flush-caches`
4. **Test recursive resolver**

   ```bash
   dig @8.8.8.8 example.com
   ```

---

## **11. Summary**

* DNS is the backbone of the internet, translating names to IP addresses.
* Key concepts: records (A, AAAA, CNAME, MX, NS, TXT), zones, resolvers.
* Practical skills: configuring records, using `dig`/`nslookup`, troubleshooting.
* Advanced features: DNSSEC, DDNS, load balancing, reverse DNS, anycast.
