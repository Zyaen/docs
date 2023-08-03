how to configure WireGuard VPN on a server (peer A) and two clients (peer B and peer C) for a private network without internet access or external local networks.

Assuming you have Arch Linux installed on all three devices (peer A, peer B, and peer C) and have administrative privileges (sudo), follow the steps below:

1. Install WireGuard on all devices (peer A, peer B, and peer C):

```
sudo pacman -S wireguard-tools
```

2. Generate private and public keys on all devices (peer A, peer B, and peer C). You can do this with the following commands:

On peer A (server):
```
umask 077
wg genkey | tee private_key_A | wg pubkey > public_key_A
```

On peer B (client):
```
umask 077
wg genkey | tee private_key_B | wg pubkey > public_key_B
```

On peer C (client):
```
umask 077
wg genkey | tee private_key_C | wg pubkey > public_key_C
```

3. Configure the WireGuard files on all devices:

On peer A (server), create or modify the `/etc/wireguard/wg0.conf` file and add the following content:

```
[Interface]
PrivateKey = <insert private key of A>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <insert public key of B>
AllowedIPs = 10.0.0.2/32

[Peer]
PublicKey = <insert public key of C>
AllowedIPs = 10.0.0.3/32
```

On peer B (client), create or modify the `/etc/wireguard/wg0.conf` file and add the following content:

```
[Interface]
PrivateKey = <insert private key of B>
Address = 10.0.0.2/24

[Peer]
PublicKey = <insert public key of A>
Endpoint = <server_A_IP>:51820
AllowedIPs = 10.0.0.0/24
```

On peer C (client), create or modify the `/etc/wireguard/wg0.conf` file and add the following content:

```
[Interface]
PrivateKey = <insert private key of C>
Address = 10.0.0.3/24

[Peer]
PublicKey = <insert public key of A>
Endpoint = <server_A_IP>:51820
AllowedIPs = 10.0.0.0/24
```

Make sure to replace `<insert private key of A>`, `<insert public key of B>`, `<insert public key of C>`, and `<server_A_IP>` with the actual values from the keys generated on each peer and the public IP or hostname of server A.

4. Enable and start the WireGuard service on all devices:

```
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
```

Once you have completed all these steps, peers B and C will be able to communicate with server A through the WireGuard VPN as if they were connected to a private LAN, but without access to the internet or other external local networks.