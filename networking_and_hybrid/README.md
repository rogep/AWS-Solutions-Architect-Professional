# Networking & Hybrid

## Public Vs. Private Services

Public or Private service is determined by a networking perspective. There are three different network zones in an AWS context:

- Public internet zone
- AWS Public zone is a network that is connected to the public internet
- AWS Private zone is anything inside a VPC (need a NAT gateway or Direct Connect/VPN services)

## VPC

The Dynamic Host Configuration Protocol (DHCP) provides a standard for passing configuration information to hosts on a TCP/IP network. The options field of a DHCP message contains configuration parameters, including the domain name, domain name server, and the netbios-node-type. When you create a VPC, AWS automatically create a set of DHCP options and associates them with the VPC. You can configure your own DHCP options set for your VPC

- DHCP Option Sets cannot be edited - once created they're _immutable_
- DHCP Option Sets can be associated with - or _more_ VPCS
- Each VPC can have MAX "1" Option Set attached
- Associated a new option set is _immediate_ - but changes require a _DHCP Renew_ which takes _time_
