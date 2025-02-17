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

