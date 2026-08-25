*This project has been created as part of the 42 curriculum by mtaheri.*

## Description

NetPractice is a networking exercise project where you solve 10 progressively harder network configuration problems. Each level presents a broken network diagram and requires you to fix IP addresses, subnet masks, and routing tables so that all hosts can communicate.

The project covers core TCP/IP networking concepts: IPv4 addressing, subnet masks, default gateways, static routing, and the roles of routers and switches...

## Instructions

**Running the training interface**

The `net_practice` folder comes from the subject archive on the 42 intra. Once extracted:

```
cd net_practice
./run.sh
```

This opens `index.html` in a browser. Enter your 42 login to start, then use the URL to navigate between levels (`level1.html` ... `level10.html`).

**Exporting configurations**

When a level is solved, click **Get my config** to download that level's `.json` configuration file.

**Submission**

The 10 exported configuration files (`level1.json` ... `level10.json`), one per level, must be placed at the root of the Git repository, then committed and pushed.

## Resources

Networking concepts studied in this project:

**TCP/IP addressing (IPv4)** — an IPv4 address is 32 bits written as four octets (`192.168.1.42`). Every address splits into a *network part* and a *host part*, and the split is decided by the mask, not by the address itself. Inside a block, the first address (all host bits at 0) is the **network address** and the last one (all host bits at 1) is the **broadcast address**; neither can be given to an interface. `127.0.0.0/8` is loopback and any address whose first octet is above 223 is reserved, so both are refused by the interface. The levels stay inside the RFC 1918 private ranges: `10.0.0.0/8`, `172.16.0.0/12` and `192.168.0.0/16`.

**Subnet masks and CIDR notation** — the mask (`255.255.255.0`, or `/24` in CIDR) tells which bits belong to the network. Two interfaces can talk to each other directly only if `IP1 & mask == IP2 & mask`, and only if they use the *same* mask on both sides. The block size of a subnet is `256 - (last mask octet)`, so a `/26` (`255.255.255.192`) cuts an octet into blocks of 64: `0-63`, `64-127`, `128-191`, `192-255`. Subnetting shows up in the later levels, where one range has to be split into several smaller networks — typically a `/30` (2 usable addresses) for a point-to-point link between two routers.

**Default gateways** — a host can only reach addresses that are inside its own subnet. Anything else is handed to its default gateway. That gateway address must itself be inside the host's subnet and must be a router interface, otherwise the host has no way to send the packet in the first place. This is the most common cause of a broken level: a correct IP with a gateway that does not belong to the same network.

**Routers and static routing tables** — a router has one interface per network it is connected to, each with its own IP and mask, and it forwards packets between them. In NetPractice, routes are static: each line is `destination / mask → next hop`. The destination describes *where the packet is going*, the next hop is the neighbouring router interface that gets it, and that next hop has to sit in a subnet the router is directly connected to. `0.0.0.0/0` is the default route and matches everything, which is what is used to point a leaf router back towards the rest of the network.

**Routers and switches** — a **switch** works at layer 2: it forwards frames using MAC addresses, has no IP address, and does not separate networks. Everything plugged into the same switch must therefore be in the same subnet. A **router** works at layer 3: it separates networks, so the two sides of a router must be in *different* subnets, and traffic crossing it depends on the routing tables and default gateways being consistent.

**OSI layers** — of the seven layers, this project lives on layers 1 to 3: layer 1 (physical) is the cabling drawn in the diagrams, layer 2 (data link) is the switches and the frames they forward, and layer 3 (network) is everything the exercises are actually graded on — IP addressing, subnet masks, gateways and routing. The four above it — layer 4 (transport), layer 5 (session), layer 6 (presentation) and layer 7 (application) — are not touched by the exercises.


- [NetworkChuck - let's subnet your home network // You SUCK at subnetting // EP 6 (playlist)](https://www.youtube.com/watch?v=mJ_5qeqGOaI&list=PLIhvC56v63IKrRHh3gvZZBAGvsvOhwrRF)

**AI usage:** AI was used only to draft and format this README. All levels were solved manually.
