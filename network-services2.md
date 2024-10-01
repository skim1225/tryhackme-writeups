# Network Services 2

## Understanding NFS
```
1. What does NFS stand for?
  a. Network File System
2. What process allows an NFS client to interact with a remote directory as though it was a physical device?
  a. Mounting
3. What does NFS use to represent files and directories on the server?
  a. file handle
4. What protocol does NFS use to communicate between the server and client?
  a. RPC
5. What two pieces of user data does the NFS server take as parameters for controlling user permissions? Format: parameter 1 / parameter 2
  a. user id / group id
6. Can a Windows NFS server share files with a Linux client? (Y/N)
  a. Y
7. Can a Linux NFS server share files with a MacOS client? (Y/N)
  a. Y
8. What is the latest version of NFS? [released in 2016, but is still up to date as of 2020] This will require external research.
  a. 4.2
```

## Enumerating NFS
```
1. Conduct a thorough port scan scan of your choosing, how many ports are open?
  a. 7. Found using the command 'nmap -A -p- -vv -T5 <target ip>'.
2. Which port contains the service we're looking to enumerate?
  a. 2049. Found in the output from the above command. 
3. Now, use /usr/sbin/showmount -e [IP] to list the NFS shares, what is the name of the visible share?
  a. /home
4. Time to mount the share to our local machine!
First, use "mkdir /tmp/mount" to create a directory on your machine to mount the share to. This is in the /tmp directory- so be aware that it will be removed on restart.
Then, use the mount command we broke down earlier to mount the NFS share to your local machine. Change directory to where you mounted the share- what is the name of the folder inside?
  a. cappucino. Used the command 'sudo mount -t nfs <target ip>:home /tmp/mount/ -nolock' to mount the remote share to our local machine, then used cd and ls to find the name of the folder.
5. Have a look inside this directory, look at the files. Looks like  we're inside a user's home directory...
  a. No answer needed
6. Interesting! Let's do a bit of research now, have a look through the folders. Which of these folders could contain keys that would give us remote access to the server?
  a. .ssh
7. Which of these keys is most useful to us?
  a. id_rsa
8. Copy this file to a different location your local machine, and change the permissions to "600" using "chmod 600 [file]".
Assuming we were right about what type of directory this is, we can pretty easily work out the name of the user this key corresponds to.
Can we log into the machine using ssh -i <key-file> <username>@<ip> ? (Y/N)
  a. Y - Used commands cp, chmod, and ssh -i to copy the rsa key and ssh into the remote server.
```

## Exploiting NFS
```
1. First, change directory to the mount point on your machine, where the NFS share should still be mounted, and then into the user's home directory.
  a. No answer needed
2. Download the bash executable to your Downloads directory. Then use "cp ~/Downloads/bash ." to copy the bash executable to the NFS share. The copied bash shell must be owned by a root user, you can set this using "sudo chown root bash"
  a. No answer needed
3. Now, we're going to add the SUID bit permission to the bash executable we just copied to the share using "sudo chmod +[permission] bash". What letter do we use to set the SUID bit set using chmod?
  a. 
4. Let's do a sanity check, let's check the permissions of the "bash" executable using "ls -la bash". What does the permission set look like? Make sure that it ends with -sr-x.
  a.
5. Now, SSH into the machine as the user. List the directory to make sure the bash executable is there. Now, the moment of truth. Lets run it with "./bash -p". The -p persists the permissions, so that it can run as root with SUID- as otherwise bash will sometimes drop the permissions.
  a. No answer needed
6. Great! If all's gone well you should have a shell as root! What's the root flag?
  a.
```

## Understanding SMTP
```
```

## Enumerating SMTP
```
```

#  Exploiting SMTP
```
```

# Understanding MySQL
```
```

# Enumerating MySQP
```
```

# Exploiting MySQL
```
```
