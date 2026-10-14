## Docker Networking 

# Overview 


Docker Bridge network - it is a private little switch that Docker can create in software. Every container plugged into the same bridge network can talk to each other like they're all plugged in into the same physical switch in a room, but separate from anything outside of that room.

My Pi already has a default bridge network, so it's been there since the first time I ran my first container, which was Vaultwarden, Caddy, and Portainer; they're already there sitting. Docker's built-in network; nobody created it on purpose. It just exists; it's there as soon as I install Docker. We're about to create a second one, a user-defined bridge network ourselves, called "mynet", and put new containers on that instead. 
What I'm going to be testing is both networks and letting containers talk to each other, but the real difference is that the user-defined one will give me automatic name resolution, meaning that the container can ping another container by its name, which in this case will be ping C2. The default bridge doesn't do that; containers on it can only find each other by a raw IP address, not by a name. It's basically adns in a miniature form: a name-to-address lookup that works in one place but not in another. 


I'm going to be putting 2 containers on the new network and two more on the old default one, and then try to ping by name on both and see what happens. 

# Steps Taken


1.  I need to create the user-defined network.
On the command line: docker network create mynet

2. I will then spin up two containers on it.
On the command line:
docker run -dit --name c1 --network mynet alpine sh
docker run -dit --name c2 --network mynet alpine sh


<img width="754" height="324" alt="User and SPlit 2 conteiners part 1 2026-10-14 111120" src="https://github.com/user-attachments/assets/be3e16b6-bbef-4b65-9c2b-404780a3c5bb" />

3. I then spun up two more on the default bridge for comparison.
On the command line:
docker run -dit --name c3 alpine sh
docker run -dit --name c4 alpine sh

It's basically the same image, but the command "--network" was not used. These two land on Docker's old default bridge automatically, the same 1 VaultWarden / Caddy / Portainer are already on.


<img width="623" height="77" alt="split 3 and 4  2026-10-14 111457" src="https://github.com/user-attachments/assets/0dbb50fc-7f4f-46ad-87c1-e5a57de8abcb" />

4. Now I want to ping by name on the user-defined network (mynet)
On the command line:
docker exec -it c1 ping -c 4 c2

This runs ping from inside c1, targeting c2 by name. Since both are on mynet, this should resolve and get real replies.

<img width="604" height="194" alt="ping c1 to c2 2026-10-14 111654" src="https://github.com/user-attachments/assets/27fc1b29-0126-41a3-9831-dd5720f1ab3f" />

Great, they were able to ping each other. Very similar to the Cisco packet tracing projects that I've done before; it's basically pinging one computer to another computer, and we're using the container itself as a switch or a router.

5. I'm gonna try to do the same thing, but this time on the default bridge.
On the command line:
docker exec -it c3 ping -c 4 c4

<img width="562" height="42" alt="image" src="https://github.com/user-attachments/assets/342b7caf-08a5-4ec4-9b0e-97648f5a727a" />

It gave me a response: "bad address". Which is the result I wanted. 

6. On the command line:
   docker network inspect mynet
   docker network inspect bridge

With these commands, the container section will split or list C1 and C2 with the hostnames tied to the IP address, which is the DNS piece working under the hood, and the bridge will show C3 and C4 but without the same name-based wiring behind it. 


<img width="934" height="986" alt="Docker network inspect  2026-10-14 113726" src="https://github.com/user-attachments/assets/2cf87c67-7fce-429e-9aa0-ef2aeb0841d4" />

This is Docker's own internal virtual bridge range, now my real home LAN, so it doesn't reveal anything about my actual network or my router subnet. It's very good to know that Docker generates its own virtual IPs, MAC addresses, and container IDs. 



7. I want to throw away these containers and move on to host networking. 
On the command line:
docker rm -f c1 c2 c3 c4
docker network rm mynet

<img width="556" height="192" alt="removed C1through C4 2026-10-14 114121" src="https://github.com/user-attachments/assets/07f809db-beee-483d-a329-f0fd98ef5296" />














































