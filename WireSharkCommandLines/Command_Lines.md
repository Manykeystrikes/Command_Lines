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

