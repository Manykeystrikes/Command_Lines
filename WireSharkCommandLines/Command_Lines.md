'''PCAP Command Lines'''
Tool/Utility	    Purpose and Benefit

capinfos	    A program that provides details of a specified capture file. 
                It is suggested to view the summary of the capture file before starting an 
                investigation.

grep	        Helps search plain-text data.

cut             Helps cut parts of lines from a specified data source.

uniq	        Filters repeated lines/values.

nl	            Views the number of shown lines. 

sed	            A stream editor.

awk 	        Scripting language that helps pattern search and processing.

Parameter	    Purpose
-h	            Display the help page with the most common features.
                        tshark -h

-v	            Show version info.
                        tshark -v

-D              List available sniffing interfaces.
                        tshark -D

-i	            Choose an interface to capture live traffic.
                        tshark -i 1
                        tshark -i ens55

No Parameter	Sniff the traffic like tcpdump.
                tshark

        Command-Line Interface and Parameters II

 

Parameter	                Purpose

-r              Read/input function. Read a capture file.
                tshark -r demo.pcapng

-c              Packet count. Stop after capturing a specified number of packets.
                E.g. stop after capturing/filtering/reading 10 packets.
                tshark -c 10

-w              Write/output function. Write the sniffed traffic to a file.
                tshark -w sample-capture.pcap

-V              Verbose.
                Provide detailed information for each packet. This option will provide details similar to Wireshark's "Packet Details Pane".
                tshark -V

-q              Silent mode.
                Suspress the packet outputs on the terminal.
                tshark -q

-x              Display packet bytes.
                Show packet details in hex and ASCII dump for each packet.
                tshark -x

        Example

        tshark -r demo.pcapng -c 2
         tshark -r demo.pcapng -c 1 -w write-demo.pcap
                "You can use the -r parameter to process the file and investigate the packets. 
                You can limit the number of shown packets using the -c parameter. 



tshark -r demo.pcapng -c 1 -w write-demo.pcap  
tshark -r write-demo.pcap   # read the captured pcap
tshark -r write-demo.pcap -x # will view and read the p/dump

write the sniffed or filtered packets to a file. You can save the sniffed traffic to a file using the -w parameter.   

Example
tshark -r demo.pcapng -c 1 # reads and stops capture after 1 packet
tshark -r demo.pcapng -c 1 -V # reads the packet and outputs 1 packet which is then displyed -V a detailed packet list 


Parameter	               Purpose

                Define capture conditions for a single run/loop. STOP after completing the condition. Also known as "Autostop".
-a              Duration: Sniff the traffic and stop after X seconds. Create a new file and write output to it.
                tshark -w test.pcap -a duration:1
                Filesize: Define the maximum capture file size. Stop after reaching X file size (KB).
                tshark -w test.pcap -a filesize:10
                Files: Define the maximum number of output files. Stop after X files.
                tshark -w test.pcap -a filesize:10 -a files:3

        Ring buffer control options. Define capture conditions for multiple runs/loops. (INFINITE LOOP). 

-b              Duration: Sniff the traffic for X seconds, create a new file and write output to it. 
                tshark -w test.pcap -b duration:1
                Filesize: Define the maximum capture file size. Create a new file and write output to it after reaching filesize X (KB).
                tshark -w test.pcap -b filesize:10
                Files: Define the maximum number of output files. Rewrite the first/oldest file after creating X files.
                tshark -w test.pcap -b filesize:10 -b files:3


Capture Filters	Live filtering options. The purpose is to save only a specific part of the traffic. It is set before capturing traffic and is not changeable during live capture.

Display Filters	Post-capture filtering options. The purpose is to investigate packets by reducing the number of visible packets, which is changeable during the investigation.

Capture filters are used to have a specific type of traffic in the capture file rather than having everything. Capture filters have limited filtering features, and the purpose is to implement a scope by range, protocol, and direction filtering. This might sound like bulk/raw filtering, but it still provides organised capture files with reasonable file size. The display filters investigate the capture files in-depth without modifying the packet.

Parameter	                        Purpose
-f	                     Capture filters. Same as BPF syntax and Wireshark's capture filters.
-Y	                     Display filters. Same as Wireshark's display filters.

                                Capture Filters

Wireshark's capture filter syntax is used here. The basic syntax for the Capture/BPF filter is shown below. You can read more on capture filter syntax here and here. Boolean operators can also be used in both types of filters. 

Further reading

https://www.wireshark.org/docs/man-pages/pcap-filter.html
https://gitlab.com/wireshark/wireshark/-/wikis/CaptureFilters#useful-filters


Qualifier	                Details and Available Options

Type                    Target match type. You can filter IP addresses, hostnames, IP ranges, and port numbers.
                        Note that if you don't set a qualifier, the "host" qualifier will be used by default.

                host | net | port | portrange
                Filtering a host
                                tshark -f "host 10.10.10.10"
                Filtering a network range 
                        tshark -f "net 10.10.10.0/24"
                Filtering a Port
                        tshark -f "port 80"
                Filtering a port range
                        tshark -f "portrange 80-100"

Direction               Target direction/flow. Note that if you don't use the direction operator, 
                        it will be equal to "either" and cover both directions.

                        src | dst
                        Filtering source address
                                tshark -f "src host 10.10.10.10"
                        Filtering destination address
                                tshark -f "dst host 10.10.10.10"        

Protocol                Target protocol.

                        arp | ether | icmp | ip | ip6 | tcp | udp
                        Filtering TCP
                                tshark -f "tcp"
                        Filtering MAC address
                                tshark -f "ether host F8:DB:C5:A2:5D:81"
                        You can also filter protocols with IP Protocol numbers assigned by IANA.
                        Filtering IP Protocols 1 (ICMP)
                                tshark -f "ip proto 1"
                                Assigned Internet Protocol Numbers

We need to create traffic noise to test and simulate capture filters. We will use the "terminator" terminal instance to have a split-screen view in a single terminal. The "terminator" will help you craft and sniff packets using a single terminal interface. Now, run the terminator command and follow the instructions using the new terminal instance. 

First, run the given TShark command in Terminal-1 to start sniffing traffic.
Then, run the given cURL command in Terminal-2 to create network noise.
View sniffed packets results in Terminal-1.

                tshark -f "host 10.10.10.10
                curl -v 10.10.10.10


Capture Filter Category	                        Details

Host Filtering                  Capturing traffic to or from a specific host.

                                Traffic generation with cURL. This command sends a default HTTP query to a specified address.
                                        curl tryhackme.com
                                TShark capture filter for a host
                                        tshark -f "host tryhackme.com"
IP Filtering                    Capturing traffic to or from a specific port. We will use the Netcat tool to create noise on specific ports.

                                Traffic generation with Netcat. Here Netcat is instructed to provide details (verbosity), and timeout is set to 5 seconds.
                                        nc 10.10.10.10 4444 -vw 5
                                TShark capture filter for specific IP address
                                        tshark -f "host 10.10.10.10"
Port Filtering                  Capturing traffic to or from a specific port. We will use the Netcat tool to create noise on specific ports.

                                Traffic generation with Netcat.Here Netcat is instructed to provide details (verbosity), and timeout is set to 5 seconds.
                                        nc 10.10.10.10 4444 -vw 5
                                TShark capture filter for port 4444
                                        tshark -f "port 4444"
Protocol Filtering              Capturing traffic to or from a specific protocol. We will use the Netcat tool to create noise on specific ports.

                                Traffic generation with Netcat. Here Netcat is instructed to use UDP, provide details (verbosity), and timeout is set to 5 seconds.
                                        nc -u 10.10.10.10 4444 -vw 5
                                TShark capture filter for
                                        tshark -f "udp"

                                        Display Filters

Wireshark's display filter syntax is used here. You can use the official Display Filter Reference to find the protocol breakdown for filtering. Additionally, you can use Wireshark's build-in "Display Filter Expression" menu to break down protocols for filters. Note that Boolean operators can also be used in both types of filters. Common filtering options are shown in the given table below.

Note: Using single quotes for capture filters is recommended to avoid space and bash expansion problems. Once again, you can check the Wireshark: Packet Operations room (Task 4 & 5) if you want to review the principles of packet filtering.

Further reading 
https://www.wireshark.org/docs/dfref/
https://tryhackme.com/room/wiresharkpacketoperations


Display Filter Category	                        Details and Available Options

Protocol: IP                    Filtering an IP without specifying a direction.
                                        tshark -Y 'ip.addr == 10.10.10.10'
                                Filtering a network range 
                                        tshark -Y 'ip.addr == 10.10.10.0/24'
                                Filtering a source IP
                                        tshark -Y 'ip.src == 10.10.10.10'
                                Filtering a destination IP
                                        tshark -Y 'ip.dst == 10.10.10.10'
Protocol: TCP	
                                Filtering TCP port
                                        tshark -Y 'tcp.port == 80'
                                Filtering source TCP port
                                        tshark -Y 'tcp.srcport == 80'
                                        
Protocol: HTTP	
                                Filtering HTTP packets
                                        tshark -Y 'http'
                                Filtering HTTP packets with response code "200"
                                        tshark -Y "http.response.code == 200"

Protocol: DNS
                                Filtering DNS packets
                                        tshark -Y 'dns'
                                Filtering all DNS "A" packets
                                        tshark -Y 'dns.qry.type == 1'
We will use the "demo.pcapng" to test display filters. Let's see the filters in action!

Sample filtering query
user@ubuntu$ tshark -r demo.pcapng -Y 'ip.addr == 145.253.2.203'
13 2.55 145.254.160.237 ? 145.253.2.203 DNS Standard query 0x0023 A ..
17 2.91 145.253.2.203 ? 145.254.160.237 DNS Standard query response 0x0023 A ..
The above terminal demonstrates using the "IP filtering" option. TShark filters the packets and provides the output in our terminal. It is worth noting that TShark doesn't count the "total number of filtered packets"; it assigns numbers to packets according to the capture time, but only displays the packets that match our filter. 

Look at the above example. There are two matched packets, but the associated numbers don't start from zero or one; "13" and "17" are assigned to these filtered packets. Keeping track of these numbers and calculating the "total number of filtered packets" can be confusing if your filter retrieves more than a handful of packets. Another example is shown below.

Sample filtering query
user@ubuntu$ tshark -r demo.pcapng -Y 'http'
  4   0.911 145.254.160.237 ? 65.208.228.223 HTTP GET /download.html HTTP/1.1  
 18   2.984 145.254.160.237 ? 216.239.59.99 HTTP GET /pagead/ads?client... 
 27   3.955 216.239.59.99 ? 145.254.160.237 HTTP HTTP/1.1 200 OK  (text/html) 
 38   4.846 65.208.228.223 ? 145.254.160.237 HTTP/XML HTTP/1.1 200 OK 
You can use the nl command to get a numbered list of your output. Therefore you can easily calculate the "total number of filtered packets" without being confused with "assigned packet numbers". The usage of the nl command is shown below.

Sample filtering query
user@ubuntu$ tshark -r demo.pcapng -Y 'http' | nl
1    4  0.911 145.254.160.237 ? 65.208.228.223 HTTP GET /download.html HTTP/1.1  
2   18  2.984 145.254.160.237 ? 216.239.59.99 HTTP GET /pagead/ads?client... 
3   27   3.955 216.239.59.99 ? 145.254.160.237 HTTP HTTP/1.1 200 OK (text/html) 
4   38   4.846 65.208.228.223 ? 145.254.160.237 HTTP/XML HTTP/1.1 200 OK 



# 
tshark -r demo.pcapng -Y "ip.addr == 65.208.228.223" | wc -l

tshark  <the program used>
-r      <Read/input function. Read a capture file.>
demo.pcapng <file name>
-Y      <Display filters. Same as Wireshark's display filters>
"ip.addr <== 65.208.228.223">
 | wc -l <-w option to write the cpatured packets to a file>
        <-c specifies the number of packets captured>
        <-l 
        This command reads the capture file capture.pcap and pipes the output to wc -l, which then counts the number of lines (each line representing a packet) in the outpu

tshark -r demo.pcapng  'tcp.port == 3371' | wc -l

tshark -r demo.pcapng -Y "ip.addr == 145.254.160.237" | wc -l

#find the source address number (src) 
tshark -r demo.pcapng -Y "ip.src == 145.254.160.237" | wc -l