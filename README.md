# University of Tehran Internal Network – Static Routing Lab

A Cisco Packet Tracer networking project that connects two faculty LANs through an internal university network using private IPv4 addressing and static routing.

## Objective

Enable a PC in the Mechanical Engineering faculty to communicate with a PC in the Computer Science faculty and verify connectivity in both directions.

The entire university network is internal, so no public IP address is used.

## Topology

```text
Mechanical Engineering LAN                     Computer Science LAN

PC4                                                            PC5
192.168.10.10/24                                         192.168.20.10/24
      |                                                            |
   Switch2                                                      Switch3
      |                                                            |
Router4 ----------------------- Router3 ------------------------ Router5
192.168.10.1                  10.0.0.2 | 10.0.0.5              192.168.20.1
10.0.0.1                             /   \
                                    /     \
                           10.0.0.0/30  10.0.0.4/30
```

Original Packet Tracer topology image: `topology/original-topology.png`

## IPv4 Addressing Plan

| # | Device / Interface | IPv4 Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| 1 | PC4 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| 2 | Router4 – LAN | `192.168.10.1` | `255.255.255.0` | — |
| 3 | Router4 → Router3 | `10.0.0.1` | `255.255.255.252` | — |
| 4 | Router3 → Router4 | `10.0.0.2` | `255.255.255.252` | — |
| 5 | Router3 → Router5 | `10.0.0.5` | `255.255.255.252` | — |
| 6 | Router5 → Router3 | `10.0.0.6` | `255.255.255.252` | — |
| 7 | Router5 – LAN | `192.168.20.1` | `255.255.255.0` | — |
| 8 | PC5 | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` |

## Networks

- Mechanical Engineering LAN: `192.168.10.0/24`
- Router4–Router3 transit network: `10.0.0.0/30`
- Router3–Router5 transit network: `10.0.0.4/30`
- Computer Science LAN: `192.168.20.0/24`

All addresses are private IPv4 addresses.

## Static Routing

### Router4

```cisco
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

### Router3

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.20.0 255.255.255.0 10.0.0.6
```

### Router5

```cisco
ip route 192.168.10.0 255.255.255.0 10.0.0.5
```

## End-to-End Paths

### Forward path: PC4 → PC5

```text
PC4
→ Switch2
→ Router4
→ Router3
→ Router5
→ Switch3
→ PC5
```

IP destination flow:

```text
192.168.10.10
→ 192.168.10.1
→ 10.0.0.2
→ 10.0.0.6
→ 192.168.20.1
→ 192.168.20.10
```

### Return path: PC5 → PC4

```text
PC5
→ Switch3
→ Router5
→ Router3
→ Router4
→ Switch2
→ PC4
```

IP destination flow:

```text
192.168.20.10
→ 192.168.20.1
→ 10.0.0.5
→ 10.0.0.1
→ 192.168.10.1
→ 192.168.10.10
```

## Verification

From PC4:

```text
ping 192.168.20.10
```

From PC5:

```text
ping 192.168.10.10
```

Useful router commands:

```cisco
show ip interface brief
show ip route
ping 10.0.0.2
ping 10.0.0.6
```

## Learning Objectives

- IPv4 subnetting and private addressing
- Point-to-point `/30` transit networks
- Default gateway configuration on end devices
- Cisco IOS interface configuration
- Static routing with next-hop addresses
- Forward-path and return-path analysis
- Basic network verification with `ping` and routing tables

## Project Type

**Cisco Packet Tracer – Static Routing / IPv4 Fundamentals**

