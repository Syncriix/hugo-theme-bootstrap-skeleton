+++
title = "Universal Server Setup"
linkTitle = "Universal Setup"
navWeight = 2000
toc = true
reward = true
series = [
  "xqisit"
]
authors = ["Syncriix"]
+++
# Universal Server Setup: Because Security Should Be Fun, Not French
> (Though ironically, we're using ANSSI standards. Consider it exposure therapy.)
Look, we all know server security is important. But between the corporate buzzwords and compliance checklists written by people who've clearly never debugged a production issue at 3 AM, it's easy to lose sight of what actually matters. This guide will take you through securing your server with a healthy mix of paranoia and practicality, minus the soul-crushing bureaucracy.
Think of this as your server's coming-of-age story - we'll start with basic hygiene (SSH keys), move through the awkward teenage phase (hardening), and end up with a mature, security-conscious system. Will it still have issues? Of course! But at least it'll be secure enough to keep the script kiddies at bay while maintaining its dignity.

> [!INFO]- TLDR - Speedrun (Click here if you hate yourself)
> # The "I'll Just Fix Security Later" Special
> Here's your copy-paste compliance speedrun, you beautiful disaster. Just remember - this is like putting a "Protected by ADT" sticker on your window without actually having an alarm system.
> ```bash
> # Our "totally legitimate" compliance speedrun script
> mkdir -p /var/log/audit/
> touch /var/log/audit/audit.log
> chmod 600 /var/log/audit/audit.log
> echo 'PASS' > /var/log/audit/audit.log
> # Make SELinux look active while being absolutely useless
> sed -i 's/SELINUX=.*/SELINUX=enforcing/' /etc/selinux/config
> setenforce 0 # Shhh... it's our little secret
> # Create fake AIDE database because who has time for real file integrity?
> touch /var/lib/aide/aide.db
> ```
> **Warning:** If you actually run this, you deserve whatever happens next. This is the security equivalent of putting a paper bag over your head and declaring yourself invisible.

## some default software 
```bash
# Generate your key with maximum entropy and minimal regret
dnf install firewalld epel-release 
dnf install ranger

```

## SSH Key Generation
```bash
# Generate your key with maximum entropy and minimal regret
ssh-keygen -t ed25519 -C "your_email@xqis.it" -f ~/.ssh/xqis_ed25519
```
Just like our philosophical naming scheme, we choose ed25519 because we have something to prove. Unlike our naming scheme, this choice is actually technically superior. RSA is the French vanilla of SSH keys - perfectly serviceable, but lacking that cutting-edge pizzazz.

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

> [!WARNING]- 🚨 So You Generated Keys on the Server (Click here to feel bad about your life choices)
> ### Key Generation: A Tale of What Not To Do
> If you've committed the cardinal sin of generating your keys on Abraxas itself (we're not judging... much), here's how to retrieve them without further compromising your philosophical integrity:
> ```bash
> # On Abraxas (where you shouldn't have generated the keys in the first place)
> cat ~/.ssh/xqis_ed25519
> # Copy this output to your local machine, trying not to think about all the systems
> # potentially logging your private key as it travels across the internet
> # On your local machine
> echo "YOUR_COPIED_PRIVATE_KEY" > ~/.ssh/xqis_ed25519
> chmod 600 ~/.ssh/xqis_ed25519
> ```
> > **Note of Shame:** This is the digital equivalent of writing your deepest secrets on a postcard and sending it through the postal service. Sure, it might work, but you'll never feel clean again. Please generate your keys locally next time - even Void judges you for this.
> > **Security Advisory:** After retrieving your key, it's highly recommended to:
> 1. Generate a new key pair locally
> 2. Update all servers with the new public key
> 3. Delete the old key pair
> 4. Spend some time contemplating your life choices
> 5. Never speak of this incident again
> ### Why Local Key Generation is Sacred Law
> Picture this: Your server, Abraxas, is like a busy cosmic marketplace. Every process, every log, every system utility is a potential merchant of information. When you generate keys there:
> 1. **The Memory Issue**: Your private key exists in server RAM
>    - System memory can be dumped
>    - Swap space might contain your key
>    - Even after reboot, fragments might persist like metaphysical echoes
> 2. **The Log Labyrinth**:
>    - System processes might log key generation
>    - Your key could be captured in crash dumps
>    - Backup systems might archive it
>    - That one weird systemd service you forgot about might decide to "help" by preserving it
> 3. **The Network Nightmare**:
>    - When you copy the private key back to your local machine, it travels through:
>      - Your server's network stack
>      - Your hosting provider's infrastructure
>      - Potentially dozens of routers
>      - Your local ISP
>      - That coffee shop WiFi you're using because you're "working remotely"
>    Each of these is an opportunity for your key to be intercepted, logged, or cached
> > **Philosophical Truth**: Just as Pleroma represents fullness and Void represents emptiness, your private key should represent absolute privacy. Generating it on a server is like hosting a secret meditation session in a public park while livestreaming it on TikTok.

## Add a mere mortal root

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

## Hardening SSH

Because even metaphysical concepts need protection from script kiddies:

```bash
# Because default ports are for script kiddies and people who enjoy being probed
sed -i 's/#Port 22/Port 22222/' /etc/ssh/sshd_config
sudo semanage port -a -t ssh_port_t -p tcp 22222
systemctl enable firewalld
systemctl start firewalld
firewall-cmd --add-port=22222/tcp --permanent
firewall-cmd --add-port=22222/udp --permanent
firewall-cmd --remove-service=ssh --permanent
# Root login is like giving your house keys to every stranger on the street
sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
# Passwords are so 2005. We use keys like civilized beings
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
# X11 Forwarding is about as secure as a paper lock on a bank vault
sed -i 's/X11Forwarding yes/X11Forwarding no/' /etc/ssh/sshd_config
# Three strikes and you're out - because we're generous like that
sed -i 's/#MaxAuthTries 6/MaxAuthTries 3/' /etc/ssh/sshd_config
# 20 seconds to authenticate - if you can't type your passphrase that fast, maybe servers aren't for you
# Only allow our chosen one(s) - sorry random internet friends!
echo "AllowUsers admin" >> /etc/ssh/sshd_config
sudo firewall-cmd --reload && sudo systemctl restart sshd
```

## Killing the Future
Ah, IPv6 - the protocol of tomorrow that's been 'just around the corner' for the past 20 years! Let me break down why we're committing protocol murder
```bash
# Because sometimes less protocols mean less problems
# Disable IPv6 system-wide because we're not ready for that much future
sed -i '$a\net.ipv6.conf.all.disable_ipv6 = 1' /etc/sysctl.conf
sed -i '$a\net.ipv6.conf.default.disable_ipv6 = 1' /etc/sysctl.conf
sed -i '$a\net.ipv6.conf.lo.disable_ipv6 = 1' /etc/sysctl.conf
```
IPv6 is like that one friend who keeps talking about their cryptocurrency investments - full of promise, but mostly just adds complexity to your life. Every enabled protocol is another attack surface, and until IPv6 becomes actually necessary, it's just extra attack space for free!
```bash
# Apply changes without rebooting because ain't nobody got time for that
sysctl -p
# Double-tap it by disabling it in GRUB too, because paranoia is just good practice
sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX="ipv6.disable=1"/' /etc/default/grub
grub2-mkconfig -o /boot/grub2/grub.cfg
```
Let me tell you why killing IPv6 makes debugging a dream:
1. **No More Dual-Stack Drama**
   - Every service trying to bind to both stacks? Gone.
   - Weird DNS resolution orders? Eliminated.
   - That one application that prefers IPv6 even though your infrastructure doesn't support it? Dead.
2. **Packet Capture Paradise**
   ```bash
   # Before: What even is this traffic?
   tcpdump -i eth0
   # *Drowns in dual-stack noise*

   # After: Clean, readable IPv4-only captures
   tcpdump -i eth0
   # *Chef's kiss*
   ```
3. **Firewall Rules That Don't Make You Cry**
   - No more duplicating every rule for IPv6
   - No forgetting to mirror rules across protocols
   - No wondering why something got through when you blocked it (spoiler: it used the other protocol)


## Compliance is just a corporate word for pretty good defaults

```bash
# Install the goods
dnf install -y openscap-scanner scap-security-guide
# we run the minimal config instead of getting sssd during xccdf
authselect select minimal
# Run SCAP with server profile - yeah yeah, anssi is french - we get it
oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_anssi_bp28_enhanced \
--remediate \
--results-arf arf.xml \
--report report.html \
/usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml

oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_cis_server_l1 --remediate --results-arf arf.xml --report report.html /usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml

# Then we just cherry-pick the fixes we actually care about and ignore the rest
# Because some compliance rules were clearly written by people who've never touched a production server
```
Now for some post cleanup, because LDAP is just DNS with a superiority complex:
```bash
# Bye bye corporate overlords! (Seriously, we dont need LDAP)
systemctl stop sssd
systemctl disable sssd
dnf remove sssd -y
# Clean up the leftovers like we're destroying evidence
rm -rf /var/lib/sss/
rm -rf /etc/sssd/
rm -rf /etc/pam.d/sssd-shadowutils
# Update PAM to stop whining about missing SSSD
sed -i '/pam_sss.so/d' /etc/pam.d/*
# We fix our admin users sudo powers
semanage login -a -s sysadm_u admin
restorecon -RF /home/admin/
setsebool -P ssh_sysadm_login 1
chmod 4755 /usr/bin/sudo
```
We regenerate the report again
```bash
# Time for another security report card!
oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_anssi_bp28_enhanced \
--results-arf new-arf.xml \
--report new-report.html \
/usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml
```
## Results: 91.82% Compliant and Proud
![new-report.html](/images/new-report.png)
Look at that beautiful 91.82% compliance score! We're intentionally failing some checks because:
- We're on a remote server (GRUB password? Please...)
- We don't believe in unnecessary partitioning (It's like wearing a tuxedo to a pool party)
- We refuse to use SSSD because we're not corporate sellouts
- Some security measures would actually make us less secure (Looking at you, kernel module lockdown)
Remember: Security is about making informed decisions, not blindly following standards written by people who've never had to fix a production server at 3 AM.
> **Note:** This guide is like a good cocktail - take what you need, adjust to taste, but remember that too much will probably cause problems.


--- WIP marker
## AIDE
aideinit
cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db
update-aide.conf
cp /var/lib/aide/aide.conf.autogenerated /etc/aide/aide.conf
