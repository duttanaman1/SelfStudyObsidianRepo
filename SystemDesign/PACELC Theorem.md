
### Beyond CAP: Why PACELC Gives Us a Fuller Picture of Distributed Systems

The CAP theorem has long been a staple in discussions around distributed system design. It taught us that during a network partition (P), we’re forced to choose between consistency (C) and availability (A).

But CAP stops there, it only speaks to failure states.

Enter PACELC, a lesser-known but more complete theorem that builds on CAP and brings latency into the equation.

Let’s break it down:

	P: If a partition occurs...
	A/C: ...you must choose between availability and consistency
	E: Else (if no partition)...
	L/C: ...you must choose between latency and consistency

In other words, PACELC = if P then A or C, else L or C.

The insight here is subtle but powerful.

Even in the absence of failures, distributed systems still face hard trade-offs. High consistency often requires coordination across nodes, leading to increased latency. And if you want low latency, you may need to relax consistency guarantees.

![[1 1.gif]]

Why It Matters
Most systems aren't operating in failure mode 99% of the time. They're functioning “normally.” CAP doesn’t tell us how to think about those scenarios.

PACELC fills that gap by helping us reason about system behavior both during partitions and in their absence.

This makes PACELC especially valuable for modern distributed databases and architectures where global performance and user experience are paramount.

Choosing the Right Trade-offs
PACELC presents four possible trade-off configurations:

PA/EL: Choose availability during partition, latency otherwise.

Prioritize uptime and responsiveness. Common in systems like CDNs or messaging queues.
![[2.gif]]

![[3.gif]]
PA/EC: Choose availability during partition, consistency otherwise.

Maximize user access during failures, but strongly consistent when healthy. Good for collaborative apps
![[4.gif]]
![[5.gif]]

PC/EL: Choose consistency during partition, latency otherwise.

Rare in practice. Tries to ensure correctness under failure while optimizing speed otherwise; challenging to balance effectively.

PC/EC: Always choose consistency.

Guarantees correctness but can be costly in performance. Used in systems with strict regulatory or correctness requirements.

No configuration is universally "right", it depends entirely on your application’s needs.

Real-World Systems
Many distributed databases make explicit PACELC trade-offs:

Cassandra leans toward PA/EL, favoring availability and speed over strict consistency.

Spanner opts for PC/EC, offering strong consistency guarantees at the cost of latency through techniques like TrueTime.

DynamoDB (in its default mode) is closer to PA/EL, but also allows tuning through read/write quorum configurations.

While PACELC theorem originated from distributed database theory, it also applies to any system where data consistency, latency, and availability must be carefully balanced across distributed nodes.

Final Thought
PACELC reframes our thinking: it reminds us that distributed systems are not just about surviving partitions. They’re about operating efficiently when everything works and when things fall apart.

As engineers, we need to be clear-eyed about the trade-offs we’re making. PACELC provides us a mental framework to do that.

So next time you're choosing a database, designing a distributed system, or tuning consistency levels, don't just ask “what happens during a partition?”
Ask: “What happens the rest of the time?”

![[7.gif]]

![[6.gif]]