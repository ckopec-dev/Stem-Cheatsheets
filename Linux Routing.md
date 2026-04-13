
# 🖧 Linux Router Tutorial

## 🧠 What You’re Building

A Linux machine with:

* **2 network interfaces**

  * `eth0` → Internet (WAN)
  * `eth1` → Internal network (LAN)
* Routing + NAT so internal machines can access the internet

---

## 📦 Step 1: Verify Network Interfaces

List interfaces:

```bash
ip addr
```

Example:

* `eth0` → 192.168.1.100 (internet side)
* `eth1` → (we’ll configure this)

---

## 🔧 Step 2: Assign LAN IP Address

Give your internal interface a static IP:

```bash
sudo ip addr add 10.0.0.1/24 dev eth1
sudo ip link set eth1 up
```

Now your Linux box is the gateway at `10.0.0.1`.

---

## 🔁 Step 3: Enable IP Forwarding

Temporarily:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Persist after reboot:

```bash
sudo nano /etc/sysctl.conf
```

Uncomment/add:

```
net.ipv4.ip_forward=1
```

Apply:

```bash
sudo sysctl -p
```

---

## 🔥 Step 4: Configure NAT (Masquerading)

This allows LAN devices to share the WAN IP.

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Allow forwarding:

```bash
sudo iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
```

---

## 💾 Step 5: Save iptables Rules

### On Debian/Ubuntu:

```bash
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

### On RHEL/CentOS:

```bash
sudo service iptables save
```

---

## 🌐 Step 6: Configure Client Machines

On devices connected to LAN:

* IP: `10.0.0.x`
* Subnet: `255.255.255.0`
* Gateway: `10.0.0.1`
* DNS: `8.8.8.8` or your ISP

---

## 🧪 Step 7: Test Connectivity

From a client:

```bash
ping 10.0.0.1        # router
ping 8.8.8.8         # internet
ping google.com      # DNS
```

---

# ⚙️ Optional Enhancements

## 📡 Add DHCP Server (Automatic IP Assignment)

Install:

```bash
sudo apt install isc-dhcp-server
```

Edit config:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Example:

```
subnet 10.0.0.0 netmask 255.255.255.0 {
  range 10.0.0.100 10.0.0.200;
  option routers 10.0.0.1;
  option domain-name-servers 8.8.8.8;
}
```

Specify interface:

```bash
sudo nano /etc/default/isc-dhcp-server
```

```
INTERFACESv4="eth1"
```

Start:

```bash
sudo systemctl restart isc-dhcp-server
```

---

## 🔐 Add Basic Firewall Rules

Example: block incoming WAN traffic

```bash
sudo iptables -A INPUT -i eth0 -j DROP
```

Allow SSH:

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

---

## 🧰 Modern Alternative: nftables

Newer systems prefer `nftables` over `iptables`.

Example NAT:

```bash
sudo nft add table ip nat
sudo nft add chain ip nat postrouting { type nat hook postrouting priority 100 \; }
sudo nft add rule ip nat postrouting oif eth0 masquerade
```

---

## 🧪 Lab Tip (Virtualization)

If you're using something like Hyper-V (based on your earlier work):

* Use **2 virtual switches**

  * External (internet)
  * Internal (lab network)
* Attach them to your Linux VM as `eth0` and `eth1`

---

# 🧭 Summary

You turned Linux into a router by:

1. Assigning IPs
2. Enabling forwarding
3. Adding NAT
4. Configuring clients

