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

## Exploting Telnet

## Understanding FTP

## Enumerating FTP

## Exploiting FTP
