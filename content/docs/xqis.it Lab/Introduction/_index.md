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

