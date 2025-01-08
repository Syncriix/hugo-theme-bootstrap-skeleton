+++
title = "Introduction"
linkTitle = "Intro"
navWeight = 1000
toc = true
reward = true
series = [
  "xqisit"
]
authors = ["Syncriix"]
+++
## The Cluster: A Symphony of Servers
Our setup is a delicate dance of servers, each playing a crucial role in the grand performance:
- **Ingress**: The gatekeeper. All traffic flows through this single server, giving us unparalleled control and visibility. It's our panopticon, our all-seeing eye.
- **High Availability (HA) Nodes**: Because uptime is a religion, and we're devout believers. Is it overkill? Perhaps. But in the pursuit of learning and pushing boundaries, there's no such thing as too much.
- **K3s**: Kubernetes, but leaner, meaner. This is where our applications live, each in their own perfectly orchestrated container.
## Ingress Server: Total Traffic Control
- Importance of a single entry point
- Benefits for monitoring, analysis, and configuration
- Tools and techniques used
## High Availability: Overkill or Opportunity?
- Explanation of the HA setup
- Challenges and lessons learned
- Potential real-world applications
## Kubernetes: Taming the Microservices Beast
- Role of K3s in the lab
- Advantages of containerization and orchestration
- Examples of services running in the cluster
## Security Spotlight
- Teleport jumphost: Secure access for the paranoid
- Zero Trust architecture: Trust no one, not even yourself
- Monitoring and alerting: Watching the watchers
## Conclusion
- Recap of key points
- Invitation for readers to follow along
- Teaser for upcoming posts and projects

## Distro
In the search for the perfect foundation for our K3s cluster, Ubuntu was swiftly dismissed due to its notorious privacy-invading tendencies - because who needs Canonical knowing when you're making a sandwich? Debian, with its peculiar flavor that sits somewhere between 'bland corporate meeting coffee' and 'lukewarm technical compromise,' didn't spark joy either. Salvation arrived in the form of Rocky Linux and AlmaLinux, the spiritual successors to CentOS. The choice between them was, as our technical analysis revealed, akin to choosing between vanilla and French vanilla ice cream. After a rigorous decision-making process that lasted approximately 30 seconds and concluded with an emphatic denouncement of all things French, Rocky Linux emerged as our champion - bringing enterprise-grade stability without the corporate bloat. The fact that AlmaLinux has nothing to do with France was deemed entirely irrelevant to this architectural decision.

> **Note:** This documentation serves as a reminder that while technical decisions in the field often stem from dubious reasoning ('Fuck the French'), the resulting infrastructure can still be rock-solid. Much like how the internet itself runs on a combination of caffeine, Stack Overflow copy-paste, and spite, our K3s cluster shall stand as a testament to the time-honored tradition of making enterprise-grade architectural choices based on completely irrelevant cultural biases. Future maintainers of this system should take comfort in knowing that at least our prejudices were properly documented.

## The Sacred Process of Nomenclature
Our journey to this enlightened naming scheme was itself a testament to technical decision-making at its finest. We began by rejecting the overdone Norse mythology approach (Odin, Thor, etc.) as being "too mainstream" - or as our architecture team eloquently put it, "the French vanilla ice-cream of server naming conventions."
A brief flirtation with Zoroastrian deities (Ahura Mazda and Angra Mainyu) was considered, but was ultimately abandoned when we realized that requiring a theology degree to SSH into a server might impact our incident response times. The thought of watching DevOps engineers google "how to spell Angra Mainyu" during a 3 AM production outage, while amusing, was deemed suboptimal for business continuity.
Finally, in our quest to be as pretentious as humanly possible (or as our project manager diplomatically logged it: "seeking a nomenclature that reflects the deep architectural considerations of our infrastructure"), we descended into Jungian psychology and Gnostic philosophy. Because nothing says "enterprise-grade infrastructure" quite like naming your high-availability cluster after the fundamental cosmic principles of existence itself. At least we can type these names without consulting ancient grimoires or batteling auto-correct.

## Server Nomenclature and Topology
Our infrastructure mirrors the cosmic architecture of existence itself, because apparently regular hostnames weren't pretentious enough:
- **abraxas.xqis.it** - Ingress Server
  - The great mediator, bridging the divine and material realms of our network traffic
  - *"He who stands at the gateway between order and chaos, SSL termination his sacred duty"*
- **pleroma.xqis.it** - Primary HA Control Plane
  - Representing the absolute fullness of being, hosting our primary K3s master
  - *"From its completeness flows the very essence of container orchestration"*
- **void.xqis.it** - Secondary HA Control Plane
  - Embodying the profound emptiness from which all failovers emerge
  - *"In its nothingness lies our salvation when Pleroma falls silent"*
> **Note to future maintainers:** Yes, we named our production infrastructure after fundamental metaphysical concepts. No, we're not sorry. If you're reading this during an outage, remember that downtime, like existence itself, is temporary and ultimately meaningless in the grand cosmic scheme. Also, check the logs in /var/log/containers/. If Pleroma goes down, failover to Void will occur automatically - a perfect metaphor for the cosmic dance of being and nothingness. Also, please make sure your SSL certificates are up to date.

## Universal Server Setup
> Because Some Things Are True Regardless of Your Cloud Provider's Existential State
### SSH Key Generation
```bash
# Generate your key with maximum entropy and minimal regret
ssh-keygen -t ed25519 -C "your_email@xqis.it" -f ~/.ssh/xqis_ed25519
```
Just like our philosophical naming scheme, we choose ed25519 because we have something to prove. Unlike our naming scheme, this choice is actually technically superior. RSA is the French vanilla of SSH keys - perfectly serviceable, but lacking that cutting-edge pizzazz.
### Initial Server Hardening Steps
Because even metaphysical concepts need protection from script kiddies:
```bash
# Disable password authentication - we're not savages
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
# Disable root login - because even Abraxas needs boundaries
sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config