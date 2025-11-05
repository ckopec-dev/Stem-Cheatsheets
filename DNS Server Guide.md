
# Installing DNS server on Ubuntu

Installing a DNS server on Ubuntu is straightforward, usually using **BIND9**, the most common DNS server software. Here’s a step-by-step guide:

---

## 1. Update Your System

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 2. Install BIND9

```bash
sudo apt install bind9 bind9utils bind9-doc -y
```

* `bind9` – the DNS server
* `bind9utils` – utilities like `dig` and `nslookup`
* `bind9-doc` – documentation

---

## 3. Configure BIND9

BIND’s main configuration files are in `/etc/bind/`.

* **Main config:** `/etc/bind/named.conf`
* **Options:** `/etc/bind/named.conf.options`
* **Zones:** `/etc/bind/named.conf.local`

### Example: Configure Forward and Reverse Zones

**Edit local zones file:**

```bash
sudo nano /etc/bind/named.conf.local
```

Add a zone:

```conf
zone "example.com" {
    type master;
    file "/etc/bind/zones/db.example.com";
};

zone "1.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/db.192";
};
```

**Create zone directory:**

```bash
sudo mkdir /etc/bind/zones
```

**Create forward zone file:**

```bash
sudo nano /etc/bind/zones/db.example.com
```

Example content:

```conf
$TTL    604800
@       IN      SOA     ns1.example.com. admin.example.com. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com.
ns1     IN      A       192.168.1.10
www     IN      A       192.168.1.20
```

**Create reverse zone file:**

```bash
sudo nano /etc/bind/zones/db.192
```

Example:

```conf
$TTL    604800
@       IN      SOA     ns1.example.com. admin.example.com. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com.
10      IN      PTR     ns1.example.com.
20      IN      PTR     www.example.com.
```

---

## 4. Check Configuration

```bash
sudo named-checkconf
sudo named-checkzone example.com /etc/bind/zones/db.example.com
sudo named-checkzone 1.168.192.in-addr.arpa /etc/bind/zones/db.192
```

---

## 5. Restart and Enable BIND

```bash
sudo systemctl restart bind9
sudo systemctl enable bind9
```

---

## 6. Test Your DNS Server

Use `dig` to test:

```bash
dig @localhost example.com
dig @localhost www.example.com
dig -x 192.168.1.20
```

---

This will give you a basic authoritative DNS server. You can later add features like caching, forwarding, and security.

---
