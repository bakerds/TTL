# Introduction

## What is EVPN-VXLAN?

**EVPN-VXLAN** is shorthand for Ethernet VPN signaled using MP-BGP<sup>:material-message-question:{ title="Multi-Protocol Border Gateway Protocol" }</sup> over VXLAN<sup>:material-message-question:{ title="Virtual eXtensible Local Area Network" }</sup> data plane encapsulation. 

**EVPN** is used as a Layer 2 control plane to provide MAC learning and bridging information over an IP or MPLS underlay. EVPN also provides Layer 3 control plane information and can simplify deployment of VRFs. 

**VXLAN** is a tunneling mechanism that allows forwarding of Ethernet frames over Layer 3 paths by wrapping them in an IP/UDP header. 

!!! success "Bottom line"

    VXLAN provides the dataplane for bridging Layer 2 traffic over a Layer 3 underlay, and EVPN provides the control plane for both Layer 2 and Layer 3 traffic in the overlay.  

## Why use EVPN-VXLAN?

EVPN-VXLAN is most often found in the datacenter, where it can help solve problems of scale and mobility in dense environments, so why use it in the collapsed core? 

**My goals:**

1. Avoid single points of failure
2. Avoid proprietary solutions

!!! success "Bottom line"

    Using EVPN-VXLAN in a collapsed core helps us avoid the drawbacks of stacking and proprietary solutions for clustering/fabric and multi-chassis link aggregation. 


!!! info "Beyond the collapsed core"

    EVPN-VXLAN can be extended across your campus into the IDF/access layer fairly easily, opening the door to more interesting underlay topologies than the traditional hub-and-spoke network, without the need for STP on the backbone. 

## Comparing options

### Stacking 

**Pros**

- **Convenience**: shared control plane makes configuration easy
- **Convenience**: link aggregation across multiple switches is easy
- **Scalability**: it's usually easy to add switches to a stack

**Cons**

- **Reliability**: if the master switch experiences a control plane fault, the whole stack usually experiences a fault
- **Maintenance**: software upgrades usually require downtime for a full stack reboot
- **Scalability**: there's usually a limit of 8-12 switches in a stack
- **Scalability**: stack interconnect bandwidth may be limited

### EVPN-VXLAN

**Pros**

- **Reliability**: switch control processes are completely separate -- each participating device can stand alone
- **Maintenance**: software upgrades can be rolled out one switch at a time without impacting other devices
- **Scalability**: can scale to many switches if necessary
- **Scalability**: interconnect bandwidth can be increased when necessary
- **Convenience**: multi-chassis link aggregation via ESI-LAG is relatively simple and standards-based
- **Standardization**: fully standards-based, supported by many manufacturers

**Cons**

- **Convenience**: no syncronization of configuration or state across participating devices 
- **Cost**: licensing is often required 

