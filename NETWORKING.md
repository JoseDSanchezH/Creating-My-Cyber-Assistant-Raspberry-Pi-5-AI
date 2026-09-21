# Networking Project: Pi-hole + WireGuard

This is the current, active project on this Pi. For the original AI assistant idea and the hardware build log (flashing the card, mounting the cooler), see [README.md](README.md).

## Why

DNS, DHCP, and VPNs were just exam topics before this. Now I'm running them myself, on the same Pi 5.

## Goal

- **Pi-hole**: DNS server for the house. Blocks ads and trackers, live query log so I can see what every device is actually looking up.
- **WireGuard**: VPN endpoint to get into the home network from outside.

## Reflashing the microSD card

Reused the same card from the AI assistant build. This time Windows couldn't even see a filesystem on it.

But I ran into a problem. I needed to reflash the microSD card several times.

The first adapter said the card was there but unreadable, then said no media at all. I then swapped to a different adapter; the same card read fine. It was the adapter, not the card.

Writes kept failing mid-write: "storage device was removed while writing," without me touching anything. The adapter was in the front USB port on the tower, so I moved it to a rear port; write finished cleanly.

I unfortunately picked the wrong drive in Imager's storage picker once in the middle of all the adapter swapping. No harm done; just read the device name and size before hitting write every time now.

# Lesson Learned
2 things: Slow down while experimenting, and do not use the tower of the computer you build for data transfers via USB ports. Always use the rear USB ports attached to the motherboard to have a successful data transfer. 

Also hit a gotcha with Imager's advanced settings. Current Raspberry Pi OS builds use `user-data` and `network-config` files to set hostname, SSH, and WiFi, instead of the old `firstrun.sh`. First pass, none of my settings actually took; the files came back as untouched defaults. Closing that warning window isn't the same as hitting Save inside it. Caught it by checking the files on the card directly before putting it back in the Pi.

## Installing Pi-hole

I connected over SSH and updated the system first:

![SSH in and update](screenshots/01-ssh-in-and-update.png)

I ran Pi-hole's install script:

```
curl -sSL https://install.pi-hole.net | sudo bash
```

![Pi-hole install starting](screenshots/02-pihole-install-start.png)

![Pi-hole welcome screen](screenshots/03-pihole-welcome.png)

I picked Cloudflare as the upstream DNS:

![Choosing Cloudflare](screenshots/04-choosing-cloudflare.png)

I left the default StevenBlack blocklist checked:

![StevenBlack blocklist](screenshots/05-stevenblack-blocklist.png)

I turned on query logging so I can actually watch DNS lookups happen instead of guessing:

![Enable query logging](screenshots/06-enable-query-logging.png)

I left privacy mode on "Show everything":

![Privacy mode](screenshots/07-privacy-mode.png)

The installer picked up my network interface and IP, pulled Pi-hole's repos, installed FTL, and landed on the login screen:

![Pi-hole login](screenshots/08-pihole-login.png)

I pointed my PC's DNS at the Pi. Query log started filling up right away, some blocked, most just resolved normal.

I pointed the router's DNS at the Pi too, not just my PC. Whole house is covered now.

## Setting up WireGuard

I installed PiVPN to handle it:

```
curl -L https://install.pivpn.io | bash
```

![Installing WireGuard](screenshots/09-installing-wireguard.png)

It asked if my Pi's IP was reserved through DHCP reservation on the router. I went looking on the Verizon admin page, the Devices menu only shows connection stats, no reservation option there. I skipped it since the Pi stays connected most of the time anyway.

![DHCP reservation prompt](screenshots/10-dhcp-reservation.png)

I picked WireGuard over OpenVPN.

![Choosing WireGuard](screenshots/11-choose-wireguard.png)

It found the Pi-hole install already on the box and asked if VPN clients should use it as DNS too. I said yes, so ad blocking still works on my phone when I'm connected remotely.

![Pi-hole DNS for VPN clients](screenshots/12-pihole-dns-question.png)

It asked whether clients connect using a public IP or a DNS name. My Verizon connection doesn't come with a fixed public IP, it can change on its own without me touching anything. So I went with a DNS name instead.

![Public IP or DNS name](screenshots/13-public-ip-or-dns.png)

I set up a free DuckDNS address for that. There's a small script running on the Pi that checks in with DuckDNS and tells it my current IP, so my phone can always find my house even if my IP changes, without me having to do anything.

![DuckDNS setup](screenshots/14-duckdns-setup.png)

I turned on unattended security upgrades, since this Pi is reachable from outside now.

![Unattended upgrades prompt](screenshots/15-unattended-upgrades.png)

Installation complete.

![Installation complete](screenshots/16-installation-complete.png)

## It works

- Forwarded UDP port 51820 on the router to the Pi
- Ran `pivpn add` to create a client profile, then `pivpn -qr` to load it onto my phone by scanning the QR code straight from the terminal
- Tested it with WiFi off on my phone: the VPN connects, Pi-hole's query log picks up the traffic, and I can reach my home network from outside

## What's next

- Maybe come back to VLANs for the IoT stuff later
