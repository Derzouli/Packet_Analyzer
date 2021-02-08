Packet_Analyzer
---------------
Network packet attack analyzer.
Analyze network packet using Scapy

1/ Ping of Death \
------------------
Ping operates by sending ICMP echo request packets to the target host and \
waiting for an ICMP echo reply.
Normal packet: (pcap: normal_ping.pcapng)
-----------------------------------------
IP layer 20 bytes: \
ICMP layer
  - headers 8 bytes \
  - data 48 bytes usually \
The packet is generated by capturing network trace from a ping (from the host \
to guest virtual machine) \
It is also possible to generate ICMP with Scapy, checkout the documentation \

Suspicious packet: (pcap: Ping_Of_Death.pcap)
---------------------------------------------
IP layer 20 bytes: \
ICMP layer
  - data 96 bytes \
(source: https://xenanetworks.com/?knowledge-base=knowledge-base/valkyrie/downloads/pcap-samples) \

check_icmp_packet.ipynb
-----------------------
The script open and read network trace, loop over each network frame, if a frame \
has ICMP layer, it will calculate the length of the packet payload (IP layer + ICMP layer) \
if this length is higher than 84 bytes of length, the packet is considered suspicious. \
For more information, as defined in RFC 791, the maximum packet of an IPv4 packet \
including the IP header is 65535 bytes. \

2/ TearDrop Attack
------------------
A teardrop attack involves sending mangled IP fragments with overlapping, oversized payloads to the target machine. This can crash various operating systems because of a bug in their TCP/IP fragmentation re-assembly code.[79] Windows 3.1x, Windows 95 and Windows NT operating systems, as well as versions of Linux prior to versions 2.0.32 and 2.1.63 are vulnerable to this attack. (source wikipedia)

Normal packet: (pcap: fragmented_dns.pcap)
------------------------------------------
The packet is generated by Scapy in the script generate_fragmented_dns_packet.py
It is IP fragments (3 fragments) of DNS query. (IP layer / UDP layer / DNS layer)

Suspicious packet: (pcap: TearDrop_Attack.pcap)
-----------------------------------------------
First frame is Echo request. (Not fragmented one)
There are 9 frames of fragmented IP (UDP protocol) with 40 bytes of data each.
(source: https://xenanetworks.com/?knowledge-base=knowledge-base/valkyrie/downloads/pcap-samples) \

check_fragments.ipynb
---------------------
The script open and read network trace, loop over each network frame, \
then check IP fragment based on id field. For each IP fragment with same id field, IP fragments
will be sorted with offset, and then check if it is possible to reassemble or not. \

A receiver knows that a packet is a fragment, if at least one of the following conditions is true: \
* The flag "more fragments" is set, which is true for all fragments except the last. \
* The field "fragment offset" is nonzero, which is true for all fragments except the first. \
 
Source: \
https://en.wikipedia.org/wiki/Ping_(networking_utility) \
https://en.wikipedia.org/wiki/Ping_of_death \
https://en.wikipedia.org/wiki/IPv4 \
https://en.wikipedia.org/wiki/Denial-of-service_attack#Teardrop_attacks
