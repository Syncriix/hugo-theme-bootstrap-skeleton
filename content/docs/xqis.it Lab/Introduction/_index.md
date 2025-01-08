\+++
title = "Introduction"
linkTitle = "Intro"
navWeight = 1000
toc = true
reward = true
series = [
  "xqisit"
]
authors = ["Syncriix"]
\+++

## The Cluster: A Symphony of Servers

Our setup is a delicate dance of servers, each playing a crucial role in the grand performance:

-   **Ingress**: The gatekeeper. All traffic flows through this single server, giving us unparalleled control and visibility. It's our panopticon, our all-seeing eye.
-   **High Availability (HA) Nodes**: Because uptime is a religion, and we're devout believers. Is it overkill? Perhaps. But in the pursuit of learning and pushing boundaries, there's no such thing as too much.
-   **K3s**: Kubernetes, but leaner, meaner. This is where our applications live, each in their own perfectly orchestrated container.

## Ingress Server: Total Traffic Control

-   Importance of a single entry point
-   Benefits for monitoring, analysis, and configuration
-   Tools and techniques used

## High Availability: Overkill or Opportunity?

-   Explanation of the HA setup
-   Challenges and lessons learned
-   Potential real-world applications

## Kubernetes: Taming the Microservices Beast

-   Role of K3s in the lab
-   Advantages of containerization and orchestration
-   Examples of services running in the cluster

## Security Spotlight

-   Teleport jumphost: Secure access for the paranoid
-   Zero Trust architecture: Trust no one, not even yourself
-   Monitoring and alerting: Watching the watchers

## Conclusion

-   Recap of key points
-   Invitation for readers to follow along
-   Teaser for upcoming posts and projects

## Distro

In the search for the perfect foundation for our K3s cluster, Ubuntu was swiftly dismissed due to its notorious privacy-invading tendencies - because who needs Canonical knowing when you're making a sandwich? Debian, with its peculiar flavor that sits somewhere between 'bland corporate meeting coffee' and 'lukewarm technical compromise,' didn't spark joy either. Salvation arrived in the form of Rocky Linux and AlmaLinux, the spiritual successors to CentOS. The choice between them was, as our technical analysis revealed, akin to choosing between vanilla and French vanilla ice cream. After a rigorous decision-making process that lasted approximately 30 seconds and concluded with an emphatic denouncement of all things French, Rocky Linux emerged as our champion - bringing enterprise-grade stability without the corporate bloat. The fact that AlmaLinux has nothing to do with France was deemed entirely irrelevant to this architectural decision.

> **Note:** This documentation serves as a reminder that while technical decisions in the field often stem from dubious reasoning ('Fuck the French'), the resulting infrastructure can still be rock-solid. Much like how the internet itself runs on a combination of caffeine, Stack Overflow copy-paste, and spite, our K3s cluster shall stand as a testament to the time-honored tradition of making enterprise-grade architectural choices based on completely irrelevant cultural biases. Future maintainers of this system should take comfort in knowing that at least our prejudices were properly documented.

## The Sacred Process of Nomenclature

Our journey to this enlightened naming scheme was itself a testament to technical decision-making at its finest. We began by rejecting the overdone Norse mythology approach (Odin, Thor, etc.) as being "too mainstream" - or as our architecture team eloquently put it, "the French vanilla ice-cream of server naming conventions."
A brief flirtation with Zoroastrian deities (Ahura Mazda and Angra Mainyu) was considered, but was ultimately abandoned when we realized that requiring a theology degree to SSH into a server might impact our incident response times. The thought of watching DevOps engineers google "how to spell Angra Mainyu" during a 3 AM production outage, while amusing, was deemed suboptimal for business continuity.
Finally, in our quest to be as pretentious as humanly possible (or as our project manager diplomatically logged it: "seeking a nomenclature that reflects the deep architectural considerations of our infrastructure"), we descended into Jungian psychology and Gnostic philosophy. Because nothing says "enterprise-grade infrastructure" quite like naming your high-availability cluster after the fundamental cosmic principles of existence itself. At least we can type these names without consulting ancient grimoires or batteling auto-correct.

## Server Nomenclature and Topology

Our infrastructure mirrors the cosmic architecture of existence itself, because apparently regular hostnames weren't pretentious enough:

-   **abraxas.xqis.it** - Ingress Server
    -   The great mediator, bridging the divine and material realms of our network traffic
    -   _"He who stands at the gateway between order and chaos, SSL termination his sacred duty"_
-   **pleroma.xqis.it** - Primary HA Control Plane
    -   Representing the absolute fullness of being, hosting our primary K3s master
    -   _"From its completeness flows the very essence of container orchestration"_
-   **void.xqis.it** - Secondary HA Control Plane
    -   Embodying the profound emptiness from which all failovers emerge
    -   _"In its nothingness lies our salvation when Pleroma falls silent"_
        > **Note to future maintainers:** Yes, we named our production infrastructure after fundamental metaphysical concepts. No, we're not sorry. If you're reading this during an outage, remember that downtime, like existence itself, is temporary and ultimately meaningless in the grand cosmic scheme. Also, check the logs in /var/log/containers/. If Pleroma goes down, failover to Void will occur automatically - a perfect metaphor for the cosmic dance of being and nothingness. Also, please make sure your SSL certificates are up to date.

## Server Specifications: Because Size Matters (Despite What They Say)

When choosing your server specifications, remember that we're building a philosophical construct as much as a technical one. Our chosen configuration balances cosmic consciousness with mundane practicality:

### The Sacred Specifications

-   **CPU**: 1 core (Monism in computing form)
-   **RAM**: 2 GB (The duality of memory management)
-   **Storage**: 20 GB (The vertex of minimal viable existence)
-   **OS**: Rocky Linux 9 64-bit (Stability without French influence)
-   **Carbon Footprint**: 0.192 kg/year (Because even metaphysical servers should be environmentally conscious)
    > **Note:** While one might argue that a single CPU core limits our parallel processing capabilities, remember that Abraxas themselves manifested as a singular entity. If it's good enough for a supreme cosmic being, it's good enough for our ingress controller.

### Why These Specifications?

-   The single CPU represents our commitment to minimalism and the rejection of excessive resource consumption
-   2GB RAM proves sufficient for our needs while keeping costs grounded in material reality
-   20GB storage allows for adequate log retention without hoarding digital karma
-   Rocky Linux 9 because... well, we've already documented our feelings about French vanilla
    > **Infrastructure Wisdom:** When your server costs less than your monthly coffee budget, you're either doing something very right or very wrong. In our case, we choose to believe it's enlightenment through minimalism rather than poor capacity planning.

## Universal Server Setup

> Because Some Things Are True Regardless of Your Cloud Provider's Existential State
>
> ### SSH Key Generation
>
> ```bash
> # Generate your key with maximum entropy and minimal regret
> ssh-keygen -t ed25519 -C "your_email@xqis.it" -f ~/.ssh/xqis_ed25519
> ```
>
> Just like our philosophical naming scheme, we choose ed25519 because we have something to prove. Unlike our naming scheme, this choice is actually technically superior. RSA is the French vanilla of SSH keys - perfectly serviceable, but lacking that cutting-edge pizzazz.

For those blessed with the knowledge of `scp`:

```bash
# From your local machine, bestow your public key upon the server
scp ~/.ssh/xqis_ed25519.pub admin@abraxas.xqis.it:~/.ssh/authorized_keys
# If you're feeling particularly fancy, use ssh-copy-id
ssh-copy-id -i ~/.ssh/xqis_ed25519 admin@abraxas.xqis.it
```

For those who embrace the chaos of copy-paste:

```bash
# On your local machine, display your public key
cat ~/.ssh/xqis_ed25519.pub
# On the server, receive the wisdom
echo "YOUR_PUBLIC_KEY_HERE" > ~/.ssh/authorized_keys
```

> **Warning:** Copying your private key around is like sharing your deepest metaphysical secrets - generally inadvisable and potentially traumatic. Keep your private key as private as your opinions about French software.

<details> <summary>🚨 So You Generated Keys on the Server (Click here to feel bad about your life choices)"</details>
### Key Generation: A Tale of What Not To Do
If you've committed the cardinal sin of generating your keys on Abraxas itself (we're not judging... much), here's how to retrieve them without further compromising your philosophical integrity:
```bash
# On Abraxas (where you shouldn't have generated the keys in the first place)
cat ~/.ssh/xqis_ed25519
# Copy this output to your local machine, trying not to think about all the systems
# potentially logging your private key as it travels across the internet
# On your local machine
echo "YOUR_COPIED_PRIVATE_KEY" > ~/.ssh/xqis_ed25519
chmod 600 ~/.ssh/xqis_ed25519
```
> **Note of Shame:** This is the digital equivalent of writing your deepest secrets on a postcard and sending it through the postal service. Sure, it might work, but you'll never feel clean again. Please generate your keys locally next time - even Void judges you for this.
> **Security Advisory:** After retrieving your key, it's highly recommended to:
1. Generate a new key pair locally
2. Update all servers with the new public key
3. Delete the old key pair
4. Spend some time contemplating your life choices
5. Never speak of this incident again
### Why Local Key Generation is Sacred Law
Picture this: Your server, Abraxas, is like a busy cosmic marketplace. Every process, every log, every system utility is a potential merchant of information. When you generate keys there:
1. **The Memory Issue**: Your private key exists in server RAM
   - System memory can be dumped
   - Swap space might contain your key
   - Even after reboot, fragments might persist like metaphysical echoes
2. **The Log Labyrinth**:
   - System processes might log key generation
   - Your key could be captured in crash dumps
   - Backup systems might archive it
   - That one weird systemd service you forgot about might decide to "help" by preserving it
3. **The Network Nightmare**:
   - When you copy the private key back to your local machine, it travels through:
     - Your server's network stack
     - Your hosting provider's infrastructure
     - Potentially dozens of routers
     - Your local ISP
     - That coffee shop WiFi you're using because you're "working remotely"
   Each of these is an opportunity for your key to be intercepted, logged, or cached
> **Philosophical Truth**: Just as Pleroma represents fullness and Void represents emptiness, your private key should represent absolute privacy. Generating it on a server is like hosting a secret meditation session in a public park while livestreaming it on TikTok.
</details>

### Initial Server Hardening Steps

Because even metaphysical concepts need protection from script kiddies:

```bash
# Disable password authentication - we're not savages
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
# Disable root login - because even Abraxas needs boundaries
sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
```

## Hardening Your Metaphysical Construct

Because even cosmic entities need protection from script kiddies and state-sponsored actors.

### User Management: The First Circle of Defense

```bash
# Create a mortal vessel for your administrative tasks
useradd -m -s /bin/bash admin
usermod -aG wheel admin
# Strengthen sudo with philosophical contemplation time
echo "Defaults authenticate" >> /etc/sudoers.d/timeout
echo "Defaults timestamp_timeout=5" >> /etc/sudoers.d/timeout
# Remove the ability to use su - root, because absolute power corrupts absolutely
passwd -l root
```

### SSH Hardening: The Gates to Your Digital Realm

```bash
# Create the sacred structure for SSH keys
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
# Configure SSHd with the wisdom of the ages
cat << EOF > /etc/ssh/sshd_config.d/hardening.conf
# Protocol 2 only - because legacy is another word for vulnerability
Protocol 2
# Authentication settings
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthenticationMethods publickey
MaxAuthTries 3
# Session settings
ClientAliveInterval 300
ClientAliveCountMax 2
LoginGraceTime 30
MaxStartups 10:30:60
MaxSessions 4
# Disable features we don't need
X11Forwarding no
AllowAgentForwarding no
AllowTcpForwarding no
PermitTunnel no
# Logging
LogLevel VERBOSE
EOF
systemctl restart sshd
```

### Firewall Configuration: Because Good Fences Make Good Neighbors

```bash
# Install and enable firewall (if not already present)
dnf install -y firewalld
systemctl enable --now firewalld
# Configure the barriers between realms
firewall-cmd --permanent --add-service=ssh
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
# If you need custom ports, add them like this:
# firewall-cmd --permanent --add-port=PORT/tcp
# Apply the new reality
firewall-cmd --reload
```

### System Hardening: The Foundation of Security

```bash
# Update the system to its latest incarnation
dnf update -y
# Install essential security tools
dnf install -y fail2ban vim-enhanced tmux
# Configure fail2ban to protect SSH
cat << EOF > /etc/fail2ban/jail.local
[sshd]
enabled = true
bantime = 3600
findtime = 600
maxretry = 3
EOF
systemctl enable --now fail2ban
# Secure shared memory
echo "tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0" >> /etc/fstab
# Disable unused filesystems
cat << EOF > /etc/modprobe.d/disable-filesystems.conf
install cramfs /bin/false
install freevxfs /bin/false
install jffs2 /bin/false
install hfs /bin/false
install hfsplus /bin/false
install squashfs /bin/false
EOF
```

> **Note:** While these settings provide a solid foundation for security, remember that like the eternal dance between Pleroma and Void, security is not a state but a process. Regular audits and updates are as essential as daily meditation.
>
> ### SELinux: Embrace the Enforcing
>
> ```bash
> # Check current status
> sestatus
> # If not already enforcing
> setenforce 1
> sed -i 's/SELINUX=permissive/SELINUX=enforcing/' /etc/selinux/config
> ```
>
> **Philosophical Note:** Unlike some who choose to disable SELinux out of convenience, we embrace its complexity as a metaphor for the necessary boundaries in life. Yes, it might occasionally prevent things from working, but so does the fundamental structure of reality.
