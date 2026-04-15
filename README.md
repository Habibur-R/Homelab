## Overview
This is my attempt at documenting the process, by which I designed and built my home lab set-up. This includes software and hardware used, application and system implementation, networking, problems I came across, how I overcame them, hardware alternatives and future considerations for improvement.

The build:
<img width="1000" height="2000" alt="PXL_20260415_185007490~2" src="https://github.com/user-attachments/assets/64fd1d0c-a29e-41f6-8e3e-cd4318ff2417" />
<img width="670" height="1001" alt="Adobe Express - file" src="https://github.com/user-attachments/assets/b1a0fc7f-9f96-496e-bb18-6a0254a6c928" />


Hardware Specifications:
- Lenovo Thinkcentre M920q i7-8th gen, 6 cores, 64gb DDR4 2666Mhz RAM, 1TB internal storage
- Netgear managed 8-port gigabit ethernet plus switch (GS308E)
- Glovary firewall mini PC N150, 4 cores, DDR5 8gb RAM, 128gb nvme SSD, 6 x 2.5GbE i226V
- Ubiquiti Unifi 6+ Access Point
- Ubiquiti Unifi PoE injector
- Cat 6 UTP cables
- Patch panel
- Rackmate T1

Software:
- OPNsense router/firewall
- Proxmox
- UOS Server 

Technologies implemented:
- Router/firewall
- Virtualisation host
- Network switch
- Access Point

## Network Specifications
The overall topology of the network is a simple router on a stick topology comprising of a router acting as the core/distribution layer and a layer 2 switch as the access layer, connected to end devices.
### The Router
The first on the list of configurations is the router.

I decided to go with a dedicated mini PC running OPNsense, specifically the Glovary mini PC. I decided on this due to the greater flexibility, security and network performance it provides in comparison to alternatively running  the OPNsense software within a docker container.

Another requirement I had was that the firewall and routing platform used was open-source and free. OPNsense ticks all of those boxes, as well as providing enterprise grade security.

After logging in to the GUI and giving myself specific user privileges, I assigned VLANs to specific interfaces and IPv4 addresses to each device, created firewall rules to allow DNS and ICMP, while allowing access to everything but internal/private networks for internet access.

I also deployed an unbound DNS resolver to improve privacy and prevent reliance on the ISP's DNS servers, allowing for direct access to DNS records directly from root servers, effectively cutting out the middle man i.e third party DNS providers, from accessing internet logs.
### The Switch
The switch is a Netgear GS308E 8 port gigabit ethernet switch and is comprised of 3 interfaces assigned for VLAN segmentation. 

The assigned VLANs are as follows:
- Virtualisation server
- Access Point
- Trunk port

I was able to access the netgear switch by connecting to the same subnet and using a discovery tool to access the GUI. At this point I assigned individual VLAN's matching those specified in the assignments made in OPNsense. What's more is that I also enabled IEEE 802.1Q VLAN trunking to manage all VLANs across a single physical connection between the router and the switch.

### The Virtualisation Server
The virtualisation server features a Lenovo Thinkcentre M920q. The hypervisor of choice for this server is Proxmox due to its open source nature and enterprise grade virtualisation.

The servers primary function is for research and testing different operating systems and applications.
### The Access Point
The access point (AP) consists of a Unifi 6+ Access Point connected via a Unifi PoE injector with one end connected to the power supply, providing adequate power to the AP, while the other end provides a steady internet connection to the rest of the homelab.

The AP itself is managed on a self hosted Unifi OS server (UOS) to provide centralised management of unifi devices.
### Limitations
A limitation of this setup is the network switch. This is due to the interface speeds being capped at 1 gigabit and as faster speeds are backwards compatible they will naturally operate at the maximum speed of the slowest component, effectively rendering this setup as a gigabit network.

However, this is fine given it acts as a private network built for research and testing, rather than a production ready environment, so speed isn't as much of an issue.
### Improvements
The following is a list of improvements I plan to make in the near future:
-  The inclusion of a Network-Attached Storage device (NAS)

Hardware Improvements:
- Raspberry Pi 5, 16gb Ram, active cooler, 512gb nvme SSD
- 2 x 5TB Seagate HDD's
