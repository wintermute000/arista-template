Creates a L3 leaf-spine topology with VXLAN via eos_template role in Ansible 2.1+

Tested with vEOS 4.15.5M in Unetlab 1.0.0-12 and Ansible 2.1

http://ansible-eos.readthedocs.io/en/master/quickstart.html

### UNDER DEVELOPMENT ###
v0.2
- Still testing, fixed indentation to align to eos_template requirements
- MLAG/VXLAN functionality appears buggy in vEOS under unetlab - may require restarting individual hosts in MLAG pairs, behaviour is inconsistent - believe this to be related to virtual lab (i.e. the provisioned configuration is correct if run on hardware)

### SAMPLE TOPOLOGY VARIABLES ###

- routed
- eBGP - spine AS 65000, leaf1/2 AS65001, leaf3/4 AS65003
- MLAG
  - Peer-link on Po1, VLAN4094
  - iBGP on Po1, VLAN4093
- Host attachment via MLAGs
  - VLAN10 on Po7 (Ethernet7) on all leafs
  - VLAN20 on Po6 (Ethernet6) on all leafs
- VXLAN
  - VLAN10 = VNI 1010
  - VLAN20 = VNI 1020
  - logical VTEPs on Lo1
- NO vVTEP or anycast GW - hosts are L2 only via VXLAN

- ADDRESSING
  - spine1-leaf1 - 192.168.1.0/30
  - spine1-leaf2 - 192.168.1.4/30
  - spine1-leaf3 - 192.168.1.8/30
  - spine1-leaf4 - 192.168.1.12/30
  - spine2-leaf1 - 192.168.2.0/30
  - spine2-leaf2 - 192.168.2.4/30
  - spine2-leaf3 - 192.168.2.8/30
  - spine2-leaf4 - 192.168.2.12/30
  - spine1 lo0 - 192.168.254.1/32
  - spine2 lo0 - 192.168.254.2/32
  - leaf1 lo0 - 192.168.254.11/32
  - leaf2 lo0 - 192.168.254.12/32
  - leaf3 lo0 - 192.168.254.13/32
  - leaf4 lo0 - 192.168.254.14/32
  - leaf1 lo1 logical VTEP - 172.31.1.1/32
  - leaf2 lo1 logical VTEP - 172.31.1.1/32
  - leaf3 lo1 logical VTEP - 172.31.1.3/32
  - leaf4 lo1 logical VTEP - 172.31.1.3/32

### ROLES ###

- configure_layer2
  - creates VLANs
  - creates Port-channels
  - configures switchport parameters
     - mode trunk
     - trunk-groups
     - allowed-vlans
     - mode access
     - access vlan
- configure_layer3
  - enables IP routing
  - creates loopbacks and SVIs
  - configures IP addresses
- configure_BGP
  - creates BGP process
  - creates custom timers
  - configures peer IPs and ASNs
  - network statements
- configure_MLAG
  - creates MLAG peering
  - adds members
- configure_VXLAN
  - creates VXLAN interface
  - creates VLAN to VNI mappings
  - configure flood-list

### VARIABLES ###

Use groupvars, and hostvars as normal - sample variables included to produce sample topology described
