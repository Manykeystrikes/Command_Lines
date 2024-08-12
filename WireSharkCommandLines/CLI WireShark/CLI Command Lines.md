CLI Command Lines

These options are applied to all packets in scope unless a display filter is provided.
Most of the commands shown below are CLI versions of the Wireshark features discussed in Wireshark: Packet Operations (Task 2).
TShark explains the parameters used at the beginning of the output line.
For example, you will use the phs option to view the protocol hierarchy. Once you use this command, the result will start with the "Packet Hierarchy Statistics" header.

Parameter	                Purpose

--color              Wireshark-like colourised output.
                     tshark --color
-z	
                    Statistics
                    There are multiple options available under this parameter. You can view the available filters under this parameter with:
                        tshark -z help
                    Sample usage.
                        tshark -z filter
                    Each time you filter the statistics, packets are shown first, then the statistics provided. You can suppress packets and focus on the statistics by using the -q parameter.

Colourised Output
               tshark -r colour.pcap --color 

Protocol Hierarchy Statistics
                    user@ubuntu$ tshark -r demo.pcapng -z io,phs -q
          #the protocols used, frame numbers, and size of packets in a tree view based on packet numbers

        #udp keyword to the filter to focus on the UDP protocol
              tshark -r demo.pcapng -z io,phs,udp -q 



analysts to detect anomalously big and small packets at a glance! Use the -z plen,tree 
-q parameters to view the packet lengths tree. View packet lengths tree
            tshark -r demo.pcapng -z plen,tree -q


 Use the -z endpoints,ip -q parameters to view IP endpoints. Note that you can choose other available protocols as well.

Filter	                Purpose
eth                 Ethernet addresses
ip                  IPv4 addresses
ipv6                IPv6 addresses
tcp                 TCP addresses
                    Valid for both IPv4 and IPv6
udp                 UDP addresses
                    Valid for both IPv4 and IPv6
wlan                IEEE 802.11 addresses

            tshark -r demo.pcapng -z endpoints,ip -q

Statistics | Expert Info

The expert info view helps analysts to view the automatic comments provided by Wireshark. If you are unfamiliar with the "Wireshark Expert Info", visit task 4 in the Wireshark: The Basics room of the Wireshark module. Use the -z expert -q parameters to view the expert information.
            tshark -r demo.pcapng -z expert -q

