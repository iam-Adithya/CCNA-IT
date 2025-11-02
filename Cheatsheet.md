# 🧭 CCNA Routing Protocols Cheat Sheet

A complete summary for quick CCNA exam review — includes Administrative Distances, Port Numbers, Protocol Numbers, Algorithms, Tables, Timers, and Key Notes for OSPF, RIP v2, EIGRP, and BGP (eBGP/iBGP).

---

## 🔹 1. Administrative Distance (AD) Values

| Route Source | AD Value | Description |
|---------------|-----------|--------------|
| **Connected** | **0** | Directly connected interface |
| **Static** | **1** | Manually configured route |
| **eBGP** | **20** | External BGP |
| **EIGRP (internal)** | **90** | Within same AS |
| **OSPF** | **110** | Link-State routing |
| **IS-IS** | **115** | Link-State routing |
| **RIP (v1/v2)** | **120** | Distance vector |
| **EIGRP (external)** | **170** | Learned from another AS |
| **iBGP** | **200** | Internal BGP |
| **Unknown / Untrusted** | **255** | Route unusable |

---

## 🔹 2. Port Numbers and Protocol Numbers

| Protocol | Port Number (UDP/TCP) | IP Protocol Number | Used For |
|-----------|------------------------|--------------------|-----------|
| **RIP v1/v2** | UDP **520** | — | Routing updates |
| **OSPF** | — | **89** | Routing information exchange |
| **EIGRP** | — | **88** | Cisco proprietary neighbor updates |
| **BGP** | TCP **179** | — | Routing between ASes (eBGP/iBGP) |
| **DHCP** | UDP **67/68** | — | Dynamic IP assignment |
| **DNS** | UDP/TCP **53** | — | Name resolution |
| **ICMP** | — | **1** | Echo requests, pings, etc. |

---

## 🔹 3. Routing Algorithms and Working

| Protocol | Algorithm | Description / Working |
|-----------|------------|------------------------|
| **RIP v2** | **Bellman-Ford (Distance Vector)** | Uses hop count as metric; max 15 hops. Sends periodic updates (30s). Entire routing table shared. |
| **EIGRP** | **DUAL (Diffusing Update Algorithm)** | Hybrid (Distance Vector + Link-State). Uses bandwidth + delay as metric. Keeps topology, routing, and neighbor tables. Fast convergence. |
| **OSPF** | **Dijkstra’s SPF (Shortest Path First)** | Link-State algorithm calculates best path using cost (based on bandwidth). Maintains LSDB for full topology. |
| **BGP** | **Path Vector Algorithm** | Uses path attributes (AS_PATH, NEXT_HOP, LOCAL_PREF, etc.) to select best path between ASes. Focus on scalability. |

---

## 🔹 4. Tables in Each Protocol

| Protocol | Tables Maintained | Description |
|-----------|------------------|--------------|
| **RIP v2** | Routing Table | Holds best routes learned via periodic updates. |
| **EIGRP** | 3 Tables:<br>1️⃣ Neighbor Table<br>2️⃣ Topology Table<br>3️⃣ Routing Table | Neighbor = Adjacent routers<br>Topology = All routes from neighbors<br>Routing = Best routes selected via DUAL |
| **OSPF** | 3 Tables:<br>1️⃣ Neighbor Table<br>2️⃣ Link-State Database (LSDB)<br>3️⃣ Routing Table | LSDB contains full network map built via LSAs; SPF algorithm builds shortest path tree. |
| **BGP** | BGP Table (Adj-RIB-In, Adj-RIB-Out, Loc-RIB) | Adj-RIB-In = learned routes<br>Loc-RIB = selected best routes<br>Adj-RIB-Out = routes advertised to peers |

---

## 🔹 5. Timers and Intervals

| Protocol | Hello | Dead/Hold | Update | Other Timers |
|-----------|--------|------------|----------|----------------|
| **RIP v2** | 30 sec | 180 sec (Invalid) | 30 sec | Flush: 240 sec |
| **EIGRP** | 5 sec (LAN) / 60 sec (WAN) | 15 sec (LAN) / 180 sec (WAN) | Triggered | SRTT, RTO used for reliability |
| **OSPF** | 10 sec (Broadcast/P2P) / 30 sec (NBMA) | 40 sec / 120 sec | Triggered | LSA refresh: 30 min, SPF delay: 5 sec |
| **BGP** | Keepalive: 60 sec | Hold: 180 sec | Triggered | Connect retry: 120 sec |

---

## 🔹 6. Metric Calculation

| Protocol | Metric | Notes |
|-----------|---------|-------|
| **RIP** | Hop Count | Max 15 hops = Unreachable |
| **EIGRP** | Bandwidth + Delay (by default) | Uses K-values (K1, K3); can add reliability, load |
| **OSPF** | Cost = 100 / Bandwidth (Mbps) | Lower cost = better path |
| **BGP** | Path Attributes | Prefers highest LOCAL_PREF, lowest AS_PATH, lowest MED, eBGP over iBGP, etc. |

---

## 🔹 7. Neighbor Relationships

| Protocol | Establishment | Authentication Support |
|-----------|----------------|-------------------------|
| **RIP v2** | Broadcast/Multicast (224.0.0.9) | Plaintext or MD5 |
| **EIGRP** | Multicast (224.0.0.10) | MD5 / SHA authentication |
| **OSPF** | Multicast (224.0.0.5 for AllSPF, 224.0.0.6 for DR/BDR) | MD5 / SHA authentication |
| **BGP** | Manual neighbor configuration | MD5 authentication supported |

---

## 🔹 8. Address Types Used

| Protocol | Multicast Address | Notes |
|-----------|-------------------|--------|
| **RIP v2** | 224.0.0.9 | IPv4 updates |
| **EIGRP** | 224.0.0.10 | IPv4 updates |
| **OSPF** | 224.0.0.5 / 224.0.0.6 | All routers / DR-others |
| **BGP** | No multicast | Uses TCP unicast between peers |

---

## 🔹 9. Key Differences (Summary)

| Feature | RIP v2 | EIGRP | OSPF | BGP |
|----------|--------|-------|------|------|
| Type | Distance Vector | Hybrid | Link-State | Path Vector |
| Metric | Hop Count | Bandwidth + Delay | Cost | Path Attributes |
| Convergence | Slow | Fast | Fast | Slow (scalable) |
| Scalability | Low | Medium | High | Very High |
| VLSM Support | ✅ | ✅ | ✅ | ✅ |
| Classless | ✅ | ✅ | ✅ | ✅ |
| Vendor | Open | Cisco | Open | Open |
| Transport | UDP | IP | IP | TCP |

---

## 🔹 10. Common Show Commands

| Protocol | Commands |
|-----------|-----------|
| **RIP** | `show ip protocols`, `show ip route rip` |
| **EIGRP** | `show ip eigrp neighbors`, `show ip eigrp topology`, `show ip route eigrp` |
| **OSPF** | `show ip ospf neighbor`, `show ip ospf database`, `show ip route ospf` |
| **BGP** | `show ip bgp summary`, `show ip bgp`, `show ip bgp neighbors` |

---

## 🔹 11. Extra Important Exam Notes

- **Passive interface:** Prevents sending routing updates on specific interfaces.
- **Route summarization:**
  - RIP / EIGRP: Manual + auto-summarization.
  - OSPF: Manual (area range, summary-address).
- **Stub areas (OSPF):** Reduce LSDB size — Stub, Totally Stubby, NSSA.
- **BGP AS Numbers:** Private (64512–65535), Public (1–64511).
- **Loop prevention:**
  - RIP: Max hop = 15.
  - EIGRP: Split Horizon, DUAL.
  - OSPF: LSA flooding control.
  - BGP: AS_PATH attribute.

---

| Protocol   | Common Commands                                                                | Purpose                                 |
| ---------- | ------------------------------------------------------------------------------ | --------------------------------------- |
| **RIP v2** | `show ip protocols`<br>`debug ip rip`<br>`show ip route rip`                   | Verify timers, updates, routing entries |
| **EIGRP**  | `show ip eigrp neighbors`<br>`show ip eigrp topology`<br>`debug eigrp packets` | Check neighbor adjacency, DUAL activity |
| **OSPF**   | `show ip ospf neighbor`<br>`show ip ospf database`<br>`debug ip ospf adj`      | Verify neighbor states and LSDB         |
| **BGP**    | `show ip bgp summary`<br>`show ip bgp neighbors`<br>`debug ip bgp updates`     | Verify session state, advertised routes |




## 🔹 12. Loop Prevention & Path Selection

| Protocol | Mechanism | Explanation |
|-----------|------------|-------------|
| **RIP** | Max hop = 15 | Prevents loops by hop limit |
| **EIGRP** | Split Horizon, DUAL | Prevents loops using Feasible Distance (FD) & Reported Distance (RD) |
| **OSPF** | LSA flooding control | Each router builds unique LSDB; SPF avoids loops |
| **BGP** | AS_PATH attribute | Detects loops by checking AS numbers in path |
### ✅ Author Notes
This cheat sheet is designed for **CCNA 200-301 and CCNP ENCOR prep** — covers all routing protocols, algorithms, timers, and metrics in concise exam-ready form.



## 🔹 13. Route Summarization & Stub Areas

- **Passive interface:** Stops sending routing updates on a specific interface.  
- **Route Summarization:**
  - RIP / EIGRP → Manual & Auto-summarization (default auto ON in older IOS)
  - OSPF → Manual only (area range / summary-address)
- **Stub Areas in OSPF:**
  - Stub
  - Totally Stubby
  - NSSA (Not-So-Stubby Area)
- **BGP AS Numbers:**
  - Private: **64512–65535**
  - Public: **1–64511**
 

## 🔹 14. Default Values to Memorize (Exam Quick Recap)

| Parameter | Default Value |
|------------|----------------|
| RIP update interval | 30 sec |
| RIP invalid timer | 180 sec |
| OSPF hello (broadcast) | 10 sec |
| OSPF dead (broadcast) | 40 sec |
| EIGRP hello (LAN) | 5 sec |
| EIGRP hold (LAN) | 15 sec |
| BGP keepalive | 60 sec |
| BGP hold | 180 sec |
| OSPF reference bandwidth | 100 Mbps |
| EIGRP K-values | K1=1, K3=1, K2=K4=K5=0 |
| Max EIGRP hops | 100 (Default) |
| RIP max hops | 15 |
| OSPF process ID range | 1–65535 |
| BGP AS number range | 1–65535 |





## 🔹 15. Key Show/Debug Commands (All-in-One Table)

| Purpose | Command |
|----------|----------|
| Show routing protocols summary | `show ip protocols` |
| Show all routing entries | `show ip route` |
| Show interface status and cost | `show ip interface brief`, `show ip ospf interface` |
| Show timers | `show ip ospf interface`, `show ip eigrp interface` |
| Debug routing updates | `debug ip rip`, `debug ip eigrp packets`, `debug ip ospf events`, `debug ip bgp updates` |
| Reset neighbor relationships | `clear ip ospf process`, `clear ip bgp *` |








# ⚡ CCNA Routing Protocols — Quick Revision Sheet

Complete CCNA Rapid Revision covering **Routing Algorithms, OSPF Areas, LSAs, IP Addressing, ACLs, and Key Definitions**.

---

## 🔹 1. IP Address Classes & Default Ranges

| Class | Range | Default Subnet Mask | Purpose | Example |
|--------|--------|---------------------|----------|----------|
| **A** | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 (/8) | Very large networks | 10.0.0.0/8 |
| **B** | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 (/16) | Medium networks | 172.16.0.0/16 |
| **C** | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 (/24) | Small networks | 192.168.1.0/24 |
| **D** | 224.0.0.0 – 239.255.255.255 | — | Multicasting | 224.0.0.5 (OSPF) |
| **E** | 240.0.0.0 – 255.255.255.255 | — | Experimental | — |

**Private Address Ranges (RFC1918):**
- Class A: **10.0.0.0 – 10.255.255.255**
- Class B: **172.16.0.0 – 172.31.255.255**
- Class C: **192.168.0.0 – 192.168.255.255**

**Common LAN/WAN Address Ranges:**
- LAN: 192.168.x.x /24, 10.x.x.x /16
- WAN: Public /30, /31 point-to-point (e.g., 198.51.100.0/30)

---

## 🔹 2. OSPF Area Types (Quick Summary)

| Area Type | Description | Allowed LSAs | Blocks |
|------------|--------------|--------------|---------|
| **Backbone (Area 0)** | Central area; all other areas connect here | All LSAs | None |
| **Standard (Normal)** | Default area type | All LSAs | None |
| **Stub Area** | Blocks external LSAs (Type 5) | 1–4, 3 | Type 5 |
| **Totally Stubby Area** | Blocks external + summary LSAs | 1–2 | Type 3, 4, 5 |
| **NSSA (Not-So-Stubby Area)** | Allows local external routes (Type 7) | 1–3, 7 | Type 5 |
| **Totally NSSA** | NSSA + Blocks inter-area LSAs | 1–3, 7 | Type 3, 4, 5 |

🧠 **Note:**  
All non-backbone areas must connect to Area 0 (via ABR).  
Each router in multiple areas is an **ABR (Area Border Router)**.

---

## 🔹 3. OSPF LSA (Link State Advertisement) Types

| LSA Type | Generated By | Purpose | Flooded To |
|-----------|---------------|----------|-------------|
| **Type 1** | Router LSA | Describes router links in an area | Within same area |
| **Type 2** | Network LSA | Describes multi-access networks (DRs) | Within same area |
| **Type 3** | Summary LSA | ABR summarizes routes between areas | Other areas |
| **Type 4** | ASBR Summary LSA | Describes ASBR location | All areas except stub |
| **Type 5** | External LSA | Advertises external (non-OSPF) routes | All areas except stub |
| **Type 6** | Group Membership (Multicast OSPF - rarely used) | Group membership info | Special |
| **Type 7** | NSSA External | Generated in NSSA to import external routes | NSSA → Converted to Type 5 by ABR |
| **Type 8/9** | Opaque LSAs | For future extensions | Local area/link |

---

## 🔹 4. Access Control Lists (ACLs)

| Type | Range | Direction | Description | Example |
|-------|--------|------------|--------------|----------|
| **Standard** | 1–99 / 1300–1999 | Inbound/Outbound | Filters **source IP only** | `access-list 10 permit 192.168.1.0 0.0.0.255` |
| **Extended** | 100–199 / 2000–2699 | Inbound/Outbound | Filters **source + destination + protocol + port** | `access-list 110 permit tcp 10.0.0.0 0.255.255.255 any eq 80` |
| **Named ACLs** | — | Inbound/Outbound | Human-readable name | `ip access-list extended BLOCK_HTTP` |
| **Numbered ACLs** | 1–2699 | Inbound/Outbound | Traditional numeric ACL | — |

🧠 **Tips:**
- Standard ACLs → place **close to destination**  
- Extended ACLs → place **close to source**  
- Use `show access-lists` and `show ip interface` to verify application.

---

## 🔹 5. Source & Destination IP Use in Routing Protocols

| Protocol | Source IP | Destination IP | Notes |
|-----------|-------------|----------------|--------|
| **RIP v2** | Interface IP | 224.0.0.9 | Multicast updates |
| **EIGRP** | Interface IP | 224.0.0.10 | Uses Reliable Transport Protocol |
| **OSPF** | Router ID / Interface IP | 224.0.0.5 (AllSPF) / 224.0.0.6 (DR/BDR) | Establishes neighbor adjacency |
| **BGP** | Local router IP | Neighbor IP (manual TCP 179) | Unicast sessions only |

---

## 🔹 6. Key Terminologies (One-Line Definitions)

| Term | Meaning |
|------|----------|
| **AD (Administrative Distance)** | Trust level of a routing source |
| **Metric** | Value to select best path (hop count, bandwidth, cost) |
| **Neighbor Table** | Tracks adjacent routers in EIGRP/OSPF |
| **Topology Table** | Stores all learned routes (EIGRP) |
| **Routing Table** | Stores best routes selected for forwarding |
| **LSDB (Link-State Database)** | OSPF’s network map of all links |
| **DUAL** | EIGRP’s Diffusing Update Algorithm for loop-free paths |
| **SPF (Dijkstra)** | OSPF algorithm for shortest path |
| **Path Vector** | BGP’s route selection model |
| **AS (Autonomous System)** | Collection of routers under one admin domain |
| **Split Horizon** | Prevents route advertisement back on incoming interface |
| **Passive Interface** | Interface does not send routing updates |
| **ABR** | Connects OSPF areas to Backbone (Area 0) |
| **ASBR** | Connects OSPF to external networks |
| **DR/BDR** | Designated and Backup Designated Routers on multi-access networks |

---

## 🔹 7. Quick Command Summary (All Protocols)

| Purpose | Command |
|----------|----------|
| Routing table overview | `show ip route` |
| Protocol details | `show ip protocols` |
| RIP updates | `debug ip rip` |
| EIGRP neighbors/topology | `show ip eigrp neighbors` / `show ip eigrp topology` |
| OSPF neighbor states | `show ip ospf neighbor` |
| OSPF database | `show ip ospf database` |
| BGP sessions | `show ip bgp summary` |
| ACL check | `show access-lists` |
| Clear routing process | `clear ip ospf process`, `clear ip bgp *` |

---

## 🔹 8. Extra Exam Memory Tips

- OSPF runs **protocol number 89**
- EIGRP uses **88**
- RIP uses **UDP 520**
- BGP uses **TCP 179**
- OSPF Hello (broadcast): **10s**, Dead: **40s**
- EIGRP Hello (LAN): **5s**, Hold: **15s**
- BGP Keepalive: **60s**, Hold: **180s**
- **Private IPs** never routed on Internet
- **Stub Areas** reduce OSPF overhead
- **Summarization** reduces routing table size

---

## ✅ Use This For:
- **CCNA 200-301 Exam Review**
- **Interview Rapid Recall**
- **Daily Routing Protocol Revision**

---

**Author:** Adiiii
**Version:** v1.0 — 2025 Exam Reference  
**License:** Free for personal use

