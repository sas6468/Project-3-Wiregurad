# Project 3 - Wireguard Docker Container

## Create a Digital Ocean account

## Create a Droplet in DigitalOcean

1. Use this link to [DigitalOcean.com](https://www.digitalocean.com) to sign up with your school email and Agent Miller’s credit card.
2. Choose the lowest price tier - $6 per month
3. Create an Ubuntu droplet:
   - Select all the basic options.
   - Choose **password** instead of SSH key: `TulsaTime@1865edu`.
   - Name the project: **Wireguard Project**.
   - **Droplet IP:** `64.23.171.188`.

   ## Install Wireguard
1. **SSH into the droplet**: 
    SSH into the droplet by opening a terminal and typing ssh root@[ip]
    Where you would replace [ip] with the IP of your DigitalOcean Droplet

2. **Install the dependencies/packages needed to run docker**: 
    -sudo apt install docker
    -sudo apt install docker-compose
    -sudo apt install wireguard

3. **Set up Wireguard using Docker Compose, Create a directory for Wireguard**: 
    -mkdir -p /opt/wireguard 
    -cd /opt/wireguard

4. **Create a docker-compose.yml file**:
    nano docker-compose.yml

5. **Add the following configuration to docker-compose.yml**:
    version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=American/New_York
      - SERVERURL=45.55.41.235
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    **Save and Exit**

6.  Run Wireguard:
    docker-compose up -d

7. **Check logs to get the QR code**:
    docker logs wireguard

# Test Your VPN
**Mobile Device**
1. Open the Wireguard app and scan the QR code from the logs.
2. Before connecting: Visit IPLeak.net and screenshot your local IP. 
3. After connecting: Turn on the Wireguard VPN and revisit IPLeak.net. Screenshot the VPN IP to confirm it is active.

**Laptop**
1. **Find the configuration file**: ls /opt/wireguard/config 
2. **Copy the .conf file to your laptop**
    In our case the .conf file contained:

    [Interface]
    PrivateKey = iLwC8xbCzwVd5j9s7Et/72d6keAAVTlkmxcY/wX6Ako=
    ListenPort = 518
    .20
    Address = 10.0.0.2/32
    DNS = 10.0.0.1

    [Peer]
    PublicKey = P5GnsQQZk4X0KilGkKNg5ND/XZjV0KP7QDNuShSCcG4=
    PresharedKey = 5X6AWptfcPEHqhgi3nVlEb6vx833rLQic/ofI4TMy5s=
    AllowedIPs = 0.0.0.0/0, ::/0
    Endpoint = 45.55.41.235:51820

3. Import the file into the Wireguard app or CLI.
4. Follow the same steps as mobile to confirm functionality using IPLeak.net. 







   
