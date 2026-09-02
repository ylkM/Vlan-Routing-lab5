# VLAN Routing Lab — 3 VLANs, 1 Router, 1 Switch (Packet Tracer)

This repo documents a Cisco Packet Tracer lab: a router (**R1**) connected to a switch (**SW1**)
with three VLANs (Engineering, HR, Sales), each on its own /26 subnet, each PC statically
addressed, and the router acting as the gateway for **each** VLAN via three separate
router‑to‑switch links (no router-on-a-stick / no trunking — one physical R1 interface per VLAN).

## Topology

```
                                   ┌────────────┐
                     G0/0 ─────────┤            │
                                   │     R1      │
                     G0/1 ─────────┤ (Router)   │
                                   │            │
                   Gig0/2 ─────────┤            │
                                   └─────┬──────┘
                                         │  (3 separate links → SW1)
                                   ┌─────┴──────┐
                                   │    SW1     │
                                   │ (Switch)   │
                                   └─┬──┬──┬────┘
                    ┌────────────────┘  │  └───────────────────┐
              VLAN10 (Fa3/1,Fa4/1) VLAN20 (Fa5/1,Fa6/1)   VLAN30 (Fa7/1,Fa8/1)
                    │                    │                      │
             ┌──────┴──────┐     ┌───────┴───────┐      ┌───────┴───────┐
             │ Engineering │     │      HR        │      │     Sales     │
             │ 10.0.0.0/26 │     │ 10.0.0.64/26   │      │ 10.0.0.128/26 │
             │  PC.1 PC.2  │     │  PC.65  PC.66  │      │ PC.130 PC.129 │
             └─────────────┘     └────────────────┘      └───────────────┘
```

## Repo structure

```
vlan-routing-lab/
├── README.md                     ← this file
├── docs/
│   └── ip-addressing-table.md    ← full IP plan (network/gateway/hosts per VLAN)
├── configs/
│   ├── R1-config.txt             ← full running-config style build for the router
│   └── SW1-config.txt            ← full running-config style build for the switch
├── topology/
│   └── topology-notes.md         ← physical/logical connection map (port-by-port)
└── verification/
    └── ping-tests.md             ← connectivity + broadcast ping test plan (Task 4)
```



| Link | R1 side | SW1 side | Carries |
|---|---|---|---|
| Link 1 | G0/0 | Fa1/1 | VLAN 10 – Engineering |
| Link 2 | G0/1 | Fa2/1 | VLAN 20 – HR |
| Link 3 | Gig0/2 | G1/1 | VLAN 30 – Sales |

Each of these three SW1 ports is configured as an **access port** in the matching VLAN
(not trunk), and each R1 interface gets a plain IP address — no subinterfaces/dot1q needed,
per the assignment's Task 2 instructions ("make three connections... configure one interface
on R1 for each VLAN").

## Quick start
1. Read `docs/ip-addressing-table.md` for exact IPs/masks/gateways.
2. Paste `configs/R1-config.txt` into R1's CLI (or type manually in Packet Tracer).
3. Paste `configs/SW1-config.txt` into SW1's CLI.
4. Set each PC's IP config per the addressing table (Desktop > IP Configuration).
5. Follow `verification/ping-tests.md` to confirm connectivity and observe VLAN
   broadcast domain isolation.
