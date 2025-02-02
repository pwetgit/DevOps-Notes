<img src="https://upload.wikimedia.org/wikipedia/commons/0/00/Pi-hole_Logo.png?20180925041558" alt="Pi-hole" width="100"/>

# Pi-hole Useful Commands

### 🔄 Update Pi-hole
To update Pi-hole to the latest version:
```bash
pihole -up
```

### 🔄 Restart Pi-hole
To restart the Pi-hole service:
```bash
pihole restartdns
```

### 🔄 Restart DNS Resolver via GUI
If there are issues with DNS, go to:
```
Settings > System > Restart DNS Resolver
```

### ⏸️ Enable/Disable Pi-hole
To disable Pi-hole for a specified amount of time (e.g., 5 minutes):
```bash
pihole disable 5m
```
To re-enable Pi-hole:
```bash
pihole enable
```

### 📊 Check Pi-hole Status
To check the status of Pi-hole:
```bash
pihole status
```

### 📄 Tail Pi-hole Logs
To view the real-time log of DNS queries:
```bash
pihole -t
```

### 🚫 Display Top Blocked Domains
To display the top blocked domains:
```bash
pihole -t
```

### 🗑️ Flush Pi-hole Logs
To flush the Pi-hole log file:
```bash
pihole flush
```

##### 📥 Update Gravity
To update the list of ad-serving domains:
```bash
pihole -g
```

### 🛠️ Repair Pi-hole
To repair Pi-hole installation:
```bash
pihole -r
```

## Pi-hole Files

### 📄 DNS Records Location
To view the list of DNS records:
```
/etc/pihole/custom.list
```

### 📄 Custom CNAME Location
To view the custom CNAME records:
```
/etc/dnsmasq.d/05-pihole-custom-cname.conf
```

### 📝 Change DHCP default DNS Servers
Edit /etc/dnsmasq.d/02-pihole-dhcp.conf with:
```
dhcp-option=option:dns-server,192.168.1.254,1.1.1.1
```
```
sudo systemctl restart pihole-FTL
```
**Warning:** ANY CHANGES MADE TO THIS FILE WILL BE LOST ON SAVING DHCP VIA GUI

