# Azalee

<picture>
 <img alt="Tryhackme" src="https://tryhackme.com/badge/635738">
</picture>

## About me

Hi, I'm Azalee. I am French systems and networks student, who loves cybersecurity !


<details>
<summary>My favorite hobbies ! </summary>


| Rank | hobbies |
|-----:|---------------|
|     1|   Learning cybersecurity            |
|     2|    listening to music            |
|     3|     learning English          |

</details>

# Firewall Evasion 

| Evasion Approach  | Nmap Argument |
| ------------- | ------------- |
| Hide a scan with decoys  | `-D DECOY1_IP1,DECOY_IP2,ME`  |
| Use an HTTP/SOCKS4 proxy to relay connections  | `--proxies PROXY_URL`  |
| Spoof source MAC address | `--spoof-mac MAC_ADDRESS` |
| Spoof source IP address	| `-S IP_ADDRESS` |
| Use a specific source port number |	`-g PORT_NUM or --source-port PORT_NUM` |
| Fragment IP data into 8 bytes |	`-f` |
| Fragment IP data into 16 bytes |	`-ff` |
| Fragment packets with given MTU |	`--mtu VALUE` |
| Specify packet length |	`--data-length NUM` |
| Set IP time-to-live field |	`--ttl VALUE` |
| Send packets with specified IP options |	`--ip-options OPTIONS` |
| Send packets with a wrong TCP/UDP checksum |	`--badsum` |
| Hide a scan with random decoys |	`-D RND,RND,ME` |

