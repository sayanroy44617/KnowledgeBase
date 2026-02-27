# Networking Basics

## 1. IP Addresses — Your Home Address on the Internet

Every device has an IP address, just like your home has a physical address. When your laptop wants to talk to Google, it's essentially saying: *"send this letter to 142.250.195.46"*.

### Types of Networks

There are two main kinds of networks:

- **Public Network** — Accessible from the internet
- **Private Network** — Isolated, not directly accessible from the internet

> 💡 **Important**: You can't talk to the internet from a private network directly. That's when you need **SSH**!

---

## 2. SSH — Remote Access to Another Computer

SSH lets you remotely control another computer through your terminal. When you run `ssh devserver`, you're essentially saying: *"open a terminal session on that remote machine"*.

### SSH Keys — The Lock & Key Analogy

Instead of using passwords, SSH uses a **key pair** for secure authentication:

| Component | Description |
|-----------|-------------|
| **Private Key** (`~/.ssh/id_rsa`) | Stays on YOUR machine — **never share this**, ever |
| **Public Key** (`~/.ssh/id_rsa.pub`) | You give this to the remote server |

**How it works:**
1. The server stores your public key
2. When you connect, the server sends you a challenge
3. Only your private key can solve that challenge
4. If it checks out, you're in — no password needed!

This is why your `~/.ssh/config` has `IdentityFile ~/.ssh/<your_key>` — it tells SSH which private key to use when connecting to this host.

### SSH Connection Example

```bash
ssh john@production-server.example.com -i ~/.ssh/mykey
```

### SSH Config Shortcut

You can define a shortcut called `admin` in your `~/.ssh/config`:

```
Host admin
  HostName somehost
  User user
  IdentityFile ~/.ssh/privatekey
```

Now you just type `ssh admin` and SSH fills in all the details automatically!

---

## 3. DNS — The Internet's Phone Book

IP addresses like `192.168.145.2` are hard to remember. So we invented **DNS (Domain Name System)** — it translates human-readable names like `api-server.internal.example.com` into IP addresses.

**How DNS works:**
1. You type a hostname in your browser
2. Your computer asks a **DNS server**: *"Hey, what's the IP for this name?"*
3. The DNS server looks it up and replies with the IP
4. Your computer connects to that IP

**Think of it like this:** You have contacts on your phone. When you tap "Mom", your phone looks up her number. DNS is that lookup step for the internet!

### Your `/etc/resolver` File

Here's the thing — your cluster (`internal.example.com`) is on a **private network**. Public DNS servers (like Google's `8.8.8.8`) have no idea it exists. So you need to tell your Mac:

> *"For anything ending in `internal.example.com`, ask THESE specific DNS servers instead"*

That's exactly what your resolver file does:

```
nameserver 192.168.145.2
nameserver 192.168.145.3
```

---

## Quick Reference: Setting Up Your Resolver

Here's everything you need to set up your `/etc/resolver` configuration in one place:

```bash
# Create the resolver directory (if it doesn't exist)
sudo mkdir -p /etc/resolver

# Create a resolver file for your domain
# Replace "internal.example.com" with your actual domain
sudo tee /etc/resolver/internal.example.com > /dev/null <<EOF
nameserver 192.168.145.2
nameserver 192.168.145.3
EOF

# Change permissions so only root can modify it
sudo chmod 644 /etc/resolver/internal.example.com

# Verify the file was created correctly
cat /etc/resolver/internal.example.com

# Test DNS resolution (should resolve using your custom nameservers)
nslookup api-server.internal.example.com

# Flush DNS cache to apply changes immediately
sudo dscacheutil -flushcache

# Or on newer macOS versions:
sudo killall -HUP mDNSResponder

# List all resolver files on your system
ls -la /etc/resolver/
```

**What each line does:**
- `sudo mkdir -p /etc/resolver` — Creates the resolver directory with parent directories if needed
- `sudo tee ... <<EOF` — Creates the file with nameserver entries
- `chmod 644` — Sets permissions (readable by all, writable only by root)
- `cat` — Displays the file contents to verify
- `nslookup` — Tests if DNS resolution works for your domain
- `dscacheutil -flushcache` — Clears DNS cache on older macOS
- `killall -HUP mDNSResponder` — Resets DNS on newer macOS (Big Sur and later)
- `ls -la /etc/resolver/` — Shows all resolver configurations on your Mac

