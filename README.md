# MITM Automation with Bettercap and HTTPS Downgrade
In my previous write-up, I performed an MITM attack manually. Today, I’ll explain how I automated that process using Bettercap.

Bettercap is a powerful tool for MITM attacks. It has built-in network discovery and ARP spoofing modules. We don’t have to set IP forwarding manually because Bettercap handles it for us automatically.

I’ll explain the modules and parameters I’ll be using beforehand.
net.probe: It sends dummy UDP packets to discover active devices on the network.
net.show: This is Bettercap’s version of netdiscover. It shows us IP addresses, MAC addresses, and even device hostnames.
net.sniff: This acts like a lightweight Wireshark. Instead of capturing every single packet, it filters the traffic and shows only the important ones. I turned this on after starting the spoof to capture credentials.

arp.spoof.fullduplex: When set to true, both the target and the gateway will be spoofed. I set its value to true because a complete MITM attack requires tricking both sides. If we don’t spoof the gateway, we won’t be able to see the responses coming back to the target.
arp.spoof.targets: This specifies our target’s IP address. While you can attack multiple targets by separating IPs with commas, it is not recommended since it makes the attack unstable and easier to detect. I set it to a single target.

I needed to be root since Bettercap requires direct access to the network interface to send and intercept packets. I typed sudo su to become root, then started a Bettercap session with eth0 as our interface:

bettercap -iface eth0
First, I checked if Bettercap could see my Windows machine.
