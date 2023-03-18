# Remote access with Raspberry Pi

The guide goal is to setup a Raspberry Pi that can connect to a Windows PC and control it. Having the computer running all the time is not very efficient and secure. So we will go through all the steps to connect to the computer from the Raspberry Pi. Raspberry Pi can run 24/7 and is much more efficient and secure than a windows computer that is running all the time and is exposed to the internet.

This guide would also work for with any other Linux computer. Just make sure that the computer has SSH installed, is running 24/7 and is connected to the internet (preferably with a static IP address). 

## Install Raspbian

Download the latest Raspbian image from [here](https://www.raspberrypi.com/software/operating-systems/).
Flash the image to the SD card using [Etcher](https://www.balena.io/etcher/).
Simply select the image file and the SD card and click on `Flash!`.

## Enable SSH on Raspberry Pi

To enable SSH on the Raspberry Pi, we dont need to connect a keyboard and a monitor to the Pi. We can simply create an empty file named `ssh` in the boot partition of the SD card. This will enable SSH on the Pi.

Create an empty file named `ssh` in the boot partition of the SD card.
```bash
sudo touch /boot/ssh
```
If that does not work, you can do it the standard way using rspi-config tool and enabling the SSH option.
```bash
raspi-config
```

## Connect to the Pi

- [Remote assess](https://www.raspberrypi.com/documentation/computers/remote-access.html)

To connect to the Pi, we need to find the IP address of the Pi. We can do this by connecting the Pi to a monitor and a keyboard and then running the following command.
```bash
ifconfig
```

Another option is to use the `arp` command on a computer connected to the same router to find the IP address of the Pi.
Arp stands for Address Resolution Protocol and it is used to find the IP address of a device on the local network.
This will also work if the Pi is connected to a different router.
```bash
arp -a
```

Or we can use the `nmap` tool to scan the local network and find the IP address of the Pi.
More on nmap [here](https://nmap.org/book/man.html).
```bash
# Scan the local network for all the devices that are running SSH this will take a while go grab a coffee
sudo nmap -sS -p 22 192.168.1.0/24
```

There is also an option of logging in into your router and finding the IP address of the Pi from there.

### Connect to the Pi using SSH

Once we have the IP address of the Pi, we can connect to it using the following command.
```bash
ssh pi@<IP_ADDRESS>
```
If your router supports static IP addresses, you can assign a static IP address to the Pi and then you will not have to find the IP address of the Pi every time you want to connect to it.
This will also alow you to connect to the Pi from outside your local network.

The default password for the `pi` user is `raspberry`.
To change the default password, we can use the `passwd` command.
To make the ssh connection more secure, we can disable the password login and only allow the ssh keys to login.

## Setting up ssh keys for password's login
Typing the password every time we want to connect to the Pi can be annoying and it is also not secure. We can use ssh keys to connect to the Pi without typing the password.
This is a one time setup and we will not have to do this again.

First we need to get the ssh keys on the computer that we want to connect to the Pi from.
```bash
#see if there is a .ssh folder in the home directory with keys
ls ~/.ssh
#if there is no keys generate the ssh keys and check again
ssh-keygen
#ssh-keygen will ask you where you want to save the keys and for a passphrase
```
Now we need to copy the public key to the Pi.
```bash
#copy the public key to the Pi
ssh-copy-id pi@<IP_ADDRESS>
```

After setting up the ssh keys, we can connect to the Pi without typing the password.
To disable the password login, we need to edit the sshd_config file.
```bash
sudo nano /etc/ssh/sshd_config
```
Find the line that says `PasswordAuthentication yes` and change it to `PasswordAuthentication no`.
Save the file and restart the ssh service.
```bash
sudo systemctl restart ssh
```

## Connect to Windows PC from the Pi

- [SSH into Windows](https://theitbros.com/ssh-into-windows/)

### sending magic packets to wake up the PC
To wake up the PC from sleep, we can use the `wakeonlan` command.
1. look for the MAC address of the PC `arp -a`
2. send the magic packet to the PC:

```bash
sudo apt install etherwake
wakeonlan <mac address>
#on redhat the command is etherwake
ether-wake <mac address>
```

important notes:
In Windows 10, the default shutdown behavior puts the system into the hybrid shutdown (also known as Fast Startup) state (S4). And all devices are put into D3. In this scenario, WOL from S4 or S5 is unsupported. Network adapters are explicitly not armed for WOL in these cases, because users expect zero power consumption and battery drain in the shutdown state. This behavior removes the possibility of invalid wake-ups when an explicit shutdown is requested. So WOL is supported only from sleep (S3), or when the user explicitly requests to enter hibernate (S4) state in Windows 10. Although the target system power state is the same between hybrid shutdown and hibernates (S4), Windows will only explicitly disable WOL when it's a hybrid shutdown transition, and not during a hibernate transition.

Disabling the hybrid shutdown in Windows 10: `powercfg -h off`
Putting the PC to hibernate: `shutdown /h`

From the Pi, we can connect to the PC using the following command.
`ssh <pc_user>@<ip addres>`
`ssh ddomi@192.168.0.150`

## Establising a remote desktop connection

Now that we have ssh access to the computer, we can use the ssh tunneling to establish a remote desktop connection. We will use the `vncviewer` command to connect to the remote desktop.

```bash
#establish the ssh tunnel
ssh -L 5901:localhost:5901 <pc_user>@<ip address>
#start the remote desktop connection
vncviewer localhost:1
```

- [Remote desktop](https://www.raspberrypi.org/documentation/remote-access/remote-desktop/README.md)


