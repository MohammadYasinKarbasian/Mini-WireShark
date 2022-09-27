# Mini-Wireshark and mini-Nmap

## What is this?

We created a **_mini-Wireshark_** and a **_mini-Nmap_** by sending everything bit-to-bit(just send 0 and 1).

## Packet sender

By running `pkt_sender.py`, any packet that is more than 14 Bytes can be sent through one of your host system interfaces.

In UNIX-based systems you can use:
```
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp2s0f0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether 20:1a:06:12:02:7d brd ff:ff:ff:ff:ff:ff
3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 48:d2:24:a5:99:34 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.8/24 brd 192.168.1.255 scope global dynamic noprefixroute wlp3s0
       valid_lft 83776sec preferred_lft 83776sec
    inet6 fe80::cb3e:796e:5b24:d2f9/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

Sending a packet through the interface that I choose before:

```
$ sudo python3 pkt_sender.py                                                 
What is your packet content? 12AF54548451FFF1124584624547
Which interface do you want to use? wlp3s0
Send 14-byte packet on wlp3s0
```

## TCP SYN packet

This is vital to fill your `info.txt` based on your host but there is another simple way, you could run `shell.sh` and it will fit `info.txt` based on your responses.

```
$ ./shell.sh                          
what is the server IP address ?
93.184.216.34
what is the target port number ?
80
what is your interface IP address ?
192.168.1.8
which port do you want to use ?
3000
what is your interface name ?
wlp3s0
what is your interface MAC address ?
48 d2 24 a5 99 34
what is your gateway MAC address ?
f4 f7 0c 00 94 6c
```

Then you can use the following command to send you a TCP SYN packet:
```
$ sudo python3 tcp_syn_sender.py                                                       
Send 54-byte TCP SYN packet on wlp3s0
```

## mini-Wireshark

Wireshark is a free and open-source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education. 
now `miniwireshark.py` is a limited Wireshark. It only captures TCP SYN-ACK packets that sent to our host.

First, run `miniwireshark.py`:
```
$ sudo python3 miniwireshark.py
```

Then run `tcp_syn_sender.py`:
```
$ sudo python3 tcp_syn_sender.py
Send 54-byte TCP SYN packet on wlp3s0
```
Then in mini-Wireshark tab you see that:
```
$ sudo python3 miniwireshark.py                                               

	A TCP Segment Captured:
		Source Port: 53 / Destination Port: 3000
		Sequence Number: 994811589 / Ack: 390672594
		on  8.8.8.8  to  192.168.1.8
		with  Flag Syn: 1  and  Flag Ack: 1
```

## mini-Nmap

Nmap is a network scanner created by Gordon Lyon. Nmap is used to discover hosts and services on a computer network by sending packets and analyzing the responses. Nmap provides several features for probing computer networks, including host discovery and service and operating system detection.  
Now, `mininmap_sender.py` is written to send TCP SYN packets. By running `mininmap_packet_capturer.py` we can capture TCP SYN ACKs and find out which ports are open.

First, run `mininmap_packet_capturer.py`:
```
$ sudo python3 mininmap_packet_capturer.py
```
Then run `mininmap_sender.py`:
```
$ sudo python3 mininmap_sender.py
What is the target IP address? (default = 93.184.216.34) 8.8.8.8
Which ports do you want to scan? 50-60
Send TCP SYN packet to port 50
Send TCP SYN packet to port 51
Send TCP SYN packet to port 52
Send TCP SYN packet to port 53
Send TCP SYN packet to port 54
Send TCP SYN packet to port 55
Send TCP SYN packet to port 56
Send TCP SYN packet to port 57
Send TCP SYN packet to port 58
Send TCP SYN packet to port 59
Send TCP SYN packet to port 60

```

Then in the mininmap_sender tab you see that:

```
$ sudo python3 mininmap_sender.py
  Port 53 is open on 8.8.8.8
```
