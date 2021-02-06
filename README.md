# Packet_Analyzer
Network packet attack analyzer. \

Analyze network packet using Scapy \
# 1/ Ping of Death \
Ping operates by sending ICMP echo request packets to the target host and \
waiting for an ICMP echo reply. \
# Normal packet: (normal_ping.pcapng)
IP layer 20 bytes \
ICMP layer \
  - headers 8 bytes \
  - data 48 bytes usually \
The packet is generated by capturing network trace from a ping (from the host \
to guest virtual machine) \
It is also possible to generate ICMP with Scapy, checkout the documentation \

# Suspicious packet: (Ping_Of_Death.pcap)
IP layer 20 bytes: \
ICMP layer \
  - data 96 bytes \
(source: https://xenanetworks.com/?knowledge-base=knowledge-base/valkyrie/downloads/pcap-samples) \

The script open and read network trace, loop over each network frame, if a frame \
has ICMP layer, it will calculate the length of the packet payload (IP layer + ICMP layer) \
if this length is higher than 84 bytes of length, the packet is considered suspicious. \
For more information, as defined in RFC 791, the maximum packet of an IPv4 packet \
including the IP header is 65535 bytes. \

https://en.wikipedia.org/wiki/Ping_(networking_utility) \
https://en.wikipedia.org/wiki/Ping_of_death \

