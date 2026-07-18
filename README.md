Modern Hotel Network - Project README
==========================================

What this project is
---------------------
An end-of-year networking project built in Cisco Packet Tracer. The brief was to
design and implement the network for a 3-floor hotel (Vic Modern Hotel), with
each department on its own floor getting its own VLAN, full routing between
floors, WiFi, dynamic IP addressing, and secure remote management.

What I built
------------
- 3 floors, 3 routers (one per floor), all interconnected with each other over
  serial links so any floor can reach any other floor.
- 8 departments total, each in its own VLAN and subnet:
    Floor 1: Reception (VLAN 80), Store (VLAN 70), Logistics (VLAN 60)
    Floor 2: Finance (VLAN 50), HR (VLAN 30), Sales/Marketing (VLAN 40)
    Floor 3: Admin (VLAN 20), IT (VLAN 10)
- One switch per floor, trunked to that floor's router so the router handles
  inter-VLAN routing (router-on-a-stick).
- OSPF running between all three routers so every subnet can reach every other
  subnet without static routes.
- Each router acts as the DHCP server for its own floor, so every device gets
  an IP automatically instead of being configured by hand.
- WiFi access points on every floor for laptops and phones.
- A printer in every department.
- SSH enabled on all three routers, so they can be managed remotely instead of
  needing a direct console connection.
- Port security on the IT switch: only one specific PC (Test-PC) is allowed on
  port Fa0/1, learned automatically via sticky MAC, and the port shuts itself
  down if any other device tries to connect there.

Why it's set up this way
-------------------------
- VLANs per department keep each team's traffic separate, so, for example,
  Finance and Reception aren't sitting on the same broadcast domain.
- Router-on-a-stick was used because the switches are Layer 2 only, so routing
  between VLANs has to happen on the router side.
- OSPF instead of static routing so the network can automatically re-route if
  a link goes down, and so it scales if more floors/routers get added later.
- DHCP per floor keeps address management simple and avoids IP conflicts
  across departments.
- SSH + port security were the two "security" requirements in the brief -
  SSH so nobody can manage the routers without credentials, and port security
  so a random device can't just plug into the IT switch and get network access.

What I'd test/verify
---------------------
- Every router can ping every other router's interfaces.
- Devices on different floors/VLANs can reach each other (proves OSPF is
  working end-to-end).
- Devices get IPs automatically without manual configuration (proves DHCP).
- SSH login works from a PC into any router.
- Plugging a device other than Test-PC into Fa0/1 on the IT switch shuts the
  port down (proves port security).

Skills this demonstrates
--------------------------
VLAN design and segmentation, inter-VLAN routing, OSPF configuration, DHCP
server configuration, WAN/serial link setup, SSH remote administration, and
port security - i.e. the core building blocks of enterprise LAN/WAN design.
