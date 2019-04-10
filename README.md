# Getting Started

Add the static IP `192.168.0.50` to a wired interface on a private network where no other DHCP services exist.

Two interfaces are needed for NAT and DNS resolution, the second can be wired or wireless.

Create the NFS folder from an existing Pi using the guide here https://blockdev.io/network-booting-a-raspberry-pi-3/.

Extract the NFS folder in the root of this project so that the following tree is created.

```
.
├── dhcpd.conf
├── docker-compose.yml
├── enable-nat.sh
├── erichough-nfs
├── exports.txt
├── named.conf
├── nfs
│   ├── client1
│       ├── bin
│       │   ├── ...
│       ├── boot
│       │   ├── ...
│       │   ├── overlays
│       │       ├── ...
│       ├── dev
│       ├── etc
│       │   ├── ...
│       ├── home
│       │   └── ...
│       ├── lib
│       │   ├── ...
│       ├── lost+found
│       ├── media
│       ├── mnt
│       ├── opt
│       │   └── ...
│       ├── proc
│       ├── root
│       ├── run
│       ├── sbin
│       │   ├── ...
│       ├── srv
│       ├── sys
│       ├── tmp
│       ├── usr
│       │   ├── ...
│       └── var
│           ├── ...
├── README.md
```

# NAT
Change the respective interfaces in `enable-nat.sh` to match your setup. `wlp3s0` is the internet-facing interface and `enp2s0` is the private network Pi interface in the example file.

Then run:

```
sudo ./enable-nat.sh
```

Ensure IP forwarding is enabled:

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

# App Armor
If App Armor is installed, which is quite likely as snap uses it the `erichough-nfs` file is used to ensure NFS can operate properly.

The `lxc` package must be installed as it adds some App Armor dependencies, without it you will get errors.

The file can be applied using docker compose, as it does in this repository, or via:

```
sudo apparmor_parser -r -W erichough-nfs
```

# Warning
I have seen the TFTP server appear to cache files like `cmdline.txt` - restart the compose stack if you suspect this is happening.