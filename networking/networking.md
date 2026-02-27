
---

## Networking Deep Dive

### The OSI Model — How data actually travels

Think of sending a message as going through 7 layers, each adding its own "envelope":

```
7. Application  — what you see (HTTP, SSH, DNS)
6. Presentation — encryption, encoding
5. Session      — managing connections
4. Transport    — TCP/UDP (how data is split and sent)
3. Network      — IP addresses (routing between networks)
2. Data Link    — MAC addresses (within a local network)
1. Physical     — actual cables, wifi signals
```

You don't need to memorize all 7. The ones that matter practically are **4 (Transport)**, **3 (Network)**, and **7 (Application)**.

---

### IP Addresses & Subnets

You already know IPs are like addresses. But there's more to it.

An IP like `192.168.145.25/24` has two parts:
- `192.168.145` — the **network** part (the neighbourhood)
- `.25` — the **host** part (the specific house)

The `/24` is called a **subnet mask** — it tells you how many bits are the network part. `/24` means the first 24 bits (first 3 numbers) identify the network, and only the last number identifies the device. So `192.168.145.x` can have 254 devices (1–254).

This is why your two nameservers `192.168.145.2` and `192.168.145.3` are on the same network — they're in the same "neighbourhood".

---

### TCP vs UDP — Two ways to send data

**TCP (Transmission Control Protocol)** — reliable, ordered
- Before sending anything, it does a **handshake**: *"you there? yes. ok let's talk."*
- Every packet is acknowledged. If one is lost, it's resent.
- Used by: SSH, HTTP, emails — anything where accuracy matters
- Slower but guaranteed delivery

**UDP (User Datagram Protocol)** — fast, no guarantees
- Just fires packets and doesn't wait for confirmation
- Used by: video calls, gaming, DNS — anything where speed matters more than perfection
- If a video frame drops, you don't want to pause and resend it, you just move on

---

### Ports — Which "door" to knock on

A server can run many services at once. Ports are how your computer knows which service it's talking to on the other end.

Some standard ones you'll see constantly:

| Port | Service |
|------|---------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 53 | DNS |
| 6443 | Kubernetes API |
| 3000 | Common dev servers |

When you `ssh ncra-admin`, it automatically connects to port 22 on that server. You could also write it explicitly as `ssh -p 22 ncra-admin`.

---

### Firewalls

A firewall is basically a **bouncer** — it controls which traffic is allowed in and out based on rules.

Rules look like: *"allow TCP on port 22 from any IP"* or *"block everything on port 3306 except from 192.168.1.x"*.

On Linux you'll see tools like `ufw` (simple) or `iptables` (powerful but complex). Cloud providers (AWS, GCP) call them **Security Groups**.

This is why sometimes SSH works but a web app on the same server doesn't — port 22 is open but port 80 is blocked by the firewall.

---

### VPN — A private tunnel

Remember how your cluster is on a private network (`192.168.145.x`) that the public internet can't reach? A **VPN (Virtual Private Network)** solves this by creating an encrypted tunnel between your machine and the private network.

Once connected to a VPN, your laptop behaves *as if it's physically inside that private network*. That's why some companies require VPN before you can SSH into their servers.

Your cluster setup might actually be using something similar — the resolver config you set up essentially assumes you're already on their network (perhaps via university WiFi or VPN).

---
