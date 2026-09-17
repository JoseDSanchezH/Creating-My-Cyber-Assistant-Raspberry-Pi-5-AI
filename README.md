# Creating-My-Cyber-Assistant-Raspberry-Pi-5-AI

# Idea

I was inspired by Mr. Terrific's T-Spheres from the 2025 Superman film; I want to build my own assistant rather than a strict prop replica.
I want to carry it between real places; I want it to understand its new surroundings, build a memory of what it's learned over time, hold a conversation, listen, remember, and run locally on the device itself. Maybe later, have its own cloud system. 

**Status: paused, not cancelled.** I pivoted this same Pi 5 to a networking project first, see the update right below. The original idea and build log stay below for the record, and I'll pick this back up later.

## Update (2026-09-17): pivoted to a networking project

The AI assistant is on hold. I reused this same Pi 5 for something I could actually finish right now: learning networking hands on instead of treating DNS, DHCP, and VPNs as disconnected exam topics. New goal, new build log, both under **Networking Project** further down. Original idea and build log below, unchanged.

## Why I'm building this

I'm building this as a hands-on project to prove out real IT/hardware skills — imaging Linux, working with hardware peripherals, troubleshooting drivers, documenting a technical build from nothing to a working device. This is a build log, not a finished product pitch.

## Hardware

- Raspberry Pi 5 (8GB)
- AI HAT+2 (Hailo-10H chip, for accelerated AI processing)
- *(more parts added here as they're bought — camera, microphone, speaker, battery, enclosure)*

## Build phases

| Phase | Goal | Status |
|---|---|---|
| 0 — The Brain | Get the core AI stack working on a desk: it hears you, thinks, sees through a camera, talks back, and remembers a fact | In progress |
| 1 — The Shell | Add battery power and a carryable case so it's actually portable | Not started |
| 2 — Integration | Full memory system working reliably while carried between real locations | Not started |
| 3 — The Showcase | Demo video and final write-up | Not started |


## Build Log
# Flashing the microSD card

Before the Pi can do anything, it needs an operating system (the base software everything else runs on top of) written onto a microSD card. Here's how that went:

1. **Downloaded Raspberry Pi Imager** This is the official free tool for writing an operating system onto a microSD card.
<img width="1242" height="512" alt="0 2026-09-05 110919" src="https://github.com/user-attachments/assets/1a590a99-eab6-4a65-97a1-61467f6501fa" />


2.  **Told it which device I have** selected "Raspberry Pi 5" from the list so it knows what it's preparing the card for. 
<img width="716" height="496" alt="1 2026-09-05 105735" src="https://github.com/user-attachments/assets/ce6a51ee-3c1c-44fe-affe-4f6d516e4d74" />


3. Almost went with the top option it recommends, "Raspberry Pi OS (64-bit)." Turns out that one runs a full desktop in the background all the time. I'm only ever going to reach this Pi remotely from my PC, never with a monitor plugged into it, so that desktop would just be sitting there eating up memory and power that the AI stuff needs instead. Went into "Raspberry Pi OS (other)" and grabbed **Raspberry Pi OS Lite (64-bit)** instead: same OS, no desktop.

<img width="700" height="494" alt="3  2026-09-05 112132" src="https://github.com/user-attachments/assets/6ce6ad1f-5d22-4094-b965-836bc0267772" />


4. Picked which drive was actually the microSD card (there were two plugged in; didn't want to wipe the wrong one).

<img width="686" height="474" alt="4 2026-09-05 112212" src="https://github.com/user-attachments/assets/8a5867ec-cd65-4b43-b218-a37ac23af7f1" />


5. Named it `CybertronJS'. *Reference: Peter Cullen

<img width="694" height="501" alt="5 2026-09-05 112230" src="https://github.com/user-attachments/assets/1aaf7889-cf27-42bc-b849-68e25b0aa057" />


6. Set my timezone and keyboard layout.  

<img width="698" height="474" alt="6 2026-09-05 112250" src="https://github.com/user-attachments/assets/22035cc3-4f3d-4f67-8738-e2c65ed26c1f" />


7. Set up a login: username `josecyber` and a password.

<img width="682" height="477" alt="7  2026-09-05 112326" src="https://github.com/user-attachments/assets/915c3a84-4c6a-4092-a86d-67d4af3967c5" />


8. Entered my home WiFi name and password so the Pi connects on its own; picked "Secure network" since my WiFi has a password, and left "Hidden SSID" unchecked since my network shows up normally. Please, if you do this, do not share your own private data. 

<img width="698" height="480" alt="8 2026-09-05 114016" src="https://github.com/user-attachments/assets/3a26312b-ce10-4dcb-99e3-1294bde9d3c6" />

9. I turned on SSH and set it to password authentication. This one matters a lot: in step 3 I chose Lite because there's no monitor on this. SSH is the only door in. If I don't turn this on now, there's no way to log into the Pi once it boots up, no screen to fall back on, nothing.

<img width="684" height="488" alt="9  2026-09-05 114126" src="https://github.com/user-attachments/assets/982e83fb-13db-4385-8d92-c4e5d6424065" />


10. Got a summary screen: Raspberry Pi 5, Raspberry Pi OS Lite (64-bit), the right storage device. Checked it over and hit write.

<img width="678" height="484" alt="10 2026-09-05 114218" src="https://github.com/user-attachments/assets/ec1a8f95-f890-47e0-a830-85e33dd03e4c" />

11. Let it write to the card. Took a few minutes.

<img width="674" height="488" alt="11  2026-09-05 114433" src="https://github.com/user-attachments/assets/11bfdf46-ee3c-4332-b801-bc7e4a7da42a" />


12. Write complete. Card ejected safely on its own, ready to go into the Pi.

<img width="666" height="480" alt="12  2026-09-05 114933" src="https://github.com/user-attachments/assets/2ecf5cc7-e8f6-469d-b1e8-820644673577" />


Good reminder that "recommended" just means best for most people, not best for what I'm actually building. Card's ready; next step is putting it in the Pi and seeing if it boots.

---

# Networking Project: Pi-hole + WireGuard

## Why

I've been treating DNS, DHCP, and VPNs as disconnected exam topics instead of things I could actually run myself. This project turns the same Pi 5 into a real DNS server for the house (Pi-hole) and a VPN endpoint (WireGuard), so I can watch DNS resolution happen instead of just knowing the definition, and reach my home network securely from outside it.

## Goal

- **Pi-hole**: network-wide DNS server, ad/tracker blocking, and a live query log so I can see what every device on the network is actually looking up
- **WireGuard**: VPN endpoint for secure remote access into the home network (not started yet, next up once Pi-hole is solid)

## Reflashing the microSD card

Reused the same card from the AI assistant build above. This time it came up corrupted, Windows couldn't even see a filesystem on it. Reflashing with Raspberry Pi Imager should have been the easy part; instead it turned into its own troubleshooting exercise:

1. **The card reader adapter was the actual problem, not the card.** The first microSD-to-USB adapter I used reported the card as present but unreadable, and later as "no media" at all. Swapped to a different adapter and the exact same card read back perfectly. Lesson: when a card looks dead, test it in a second adapter before writing it off.
2. **Writes kept failing mid-process** with "storage device was removed while writing," without me touching anything. Turned out to be the front-panel USB port on my tower case; those route through an extra internal cable and are known to be less reliable for sustained transfers than the ports built directly into the motherboard. Moved the adapter to a rear port and the write finally completed and verified clean.
3. Also picked the wrong device in Imager's storage picker once during all the swapping around, no real harm done, but a good reminder to read the exact device name and size before hitting write every single time, not assume the right one's still selected.

**A gotcha with Imager's advanced settings:** current Raspberry Pi OS builds use cloud-init style config (`user-data`, `network-config`) to apply the hostname/SSH/WiFi settings from Imager's advanced options, replacing the older `firstrun.sh` approach. First pass through, my settings didn't actually take, the files came out as untouched defaults, because closing that dialog isn't the same as clicking **Save** inside it. Caught it by checking the files directly before ever putting the card back in the Pi, which saved a wasted boot attempt.

## Installing Pi-hole

Connected over SSH and updated the system first:

![SSH in and update](screenshots/01-ssh-in-and-update.png)

Ran Pi-hole's official install script:

```
curl -sSL https://install.pi-hole.net | sudo bash
```

![Pi-hole install starting](screenshots/02-pihole-install-start.png)

The installer walks through a handful of screens:

![Pi-hole welcome screen](screenshots/03-pihole-welcome.png)

Picked Cloudflare as the upstream DNS provider, where Pi-hole forwards anything it doesn't block:

![Choosing Cloudflare](screenshots/04-choosing-cloudflare.png)

Went with the default StevenBlack Unified Hosts List to start, it's the standard, well-maintained blocklist most Pi-hole installs run on day one:

![StevenBlack blocklist](screenshots/05-stevenblack-blocklist.png)

Enabled query logging, this is what actually lets you watch DNS lookups happen in real time instead of just knowing Pi-hole is blocking things somewhere in the background:

![Enable query logging](screenshots/06-enable-query-logging.png)

Left privacy mode at "Show everything" for now, since the whole point right now is visibility into what's happening on the network, not hiding it from myself:

![Privacy mode](screenshots/07-privacy-mode.png)

Installer finished, detected my network interface and IP, pulled Pi-hole's core repos, and installed FTL (the DNS engine). Landed on the admin login screen:

![Pi-hole login](screenshots/08-pihole-login.png)

Pointed my PC's DNS at the Pi to test it, and the query log immediately started filling with real lookups from my computer, some blocked, most just resolved normally. That's the concrete version of "this is what DNS actually does," not just the textbook definition.

## What's next

- Point the router's DNS at the Pi so the whole house benefits, not just one test device
- Set up WireGuard for secure remote access
- Stretch goal: revisit VLAN segmentation to isolate IoT devices, once this is solid

































































































