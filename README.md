# .config
all of my configuration files

increase wifi speed (error with my hp laptop):

https://itsfoss.com/speed-up-slow-wifi-connection-ubuntu/

Solution 1: For Slow WiFi in Atheros Wireless Network Adapters
First, you need to find your wireless network adapter in Linux. You can do so by using the command below in the terminal:

lshw -C network
If your adapter manufacturer is Atheros, this solution should work for you.

Open a terminal (Ctrl+Alt+T in Ubuntu) and use the following commands one by one:

sudo su
echo "options ath9k nohwcrypt=1" >> /etc/modprobe.d/ath9k.conf
Here, you’re basically enabling a module to use software-based encryption over hardware encryption for your adapter.

This will add the additional line to the configuration file. Restart your computer and you should be good to go.

If it does not fix or if you don’t have Atheros WiFi adaptor, try other solutions.

Solution 2: Disable 802.11n (works best if you have older router)
The next trick is to force disable the 802.11n protocol. Even after so many years, most of the world runs 802.11a,b and g. While 802.11n provides better data rate, not all the routers support it, especially the older ones. It has been observed that disabling the 802.11 n helps speed up the wireless connection in Ubuntu and other OS.

Open the terminal and use the following command:

sudo rmmod iwlwifi
sudo modprobe iwlwifi 11n_disable=1
It’s worth noting that in newer kernels, doing this will also disable the 802.11ac protocol and will limit the device throughput to 54 Mbps as mentioned in Gentoo’s wiki page.

If you find no significant increase in the wireless connection speed, restart the computer to revert the changes and forget about this solution.

But, if it worked for you and you have a faster WiFi now, you should make the changes permanent by using these commands:

sudo su
echo "options iwlwifi 11n_disable=1" >> /etc/modprobe.d/iwlwifi.conf
Restart your computer and live your life at full speed.

Solution 3: Fix the bug in Debian Avahi-daemon
The slow WiFi in Ubuntu problem could also be related to a bug in Avahi-daemon of Debian. Ubuntu and many other Linux distribution are based on Debian so this bug propagates to these Linux distributions as well.

To fix this bug, you have to edit the nsswitch configuration file. Open a terminal and use the following command:

sudo gedit /etc/nsswitch.conf
This will open the configuration file in gedit so that you could edit it easily in GUI. You can also use nano instead of gedit if you want to use the terminal. In here, look for the following line:

hosts:          files mdns4_minimal [NOTFOUND=return] dns mdns4
If you find this file, replace it with the following line:

hosts:          files dns
Save it, close it, restart your computer. It should fix the slow wireless connection problem for you. If it doesn’t, check the other solutions.

Solution 4: Disable IPv6 support
Yes, you heard it right. Let’s go back to the previous century and care about IPv4 by ditching IPv6 support. Sometimes, you don’t even need IPv6 support.

Moreover, if it improves the Wi-Fi speed in some cases. So, you can surely give it a try if nothing else is working for you.

To disable IPv6 support in Ubuntu, use the following commands one by one:

sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1
Do note that you’re disabling IPv6 temporarily here. If it doesn’t work, simply reboot your system and you will have IPv6 enabled. But, if it does work, you can type in the following commands to make the change permanent:

sudo su
 echo "#disable ipv6" >> /etc/sysctl.conf
 echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf
 echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf
 echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf
Restart your computer and it should do the magic. If not try the next one.

Solution 5: Ditch default network manager and embrace Wicd (possibly obsolete)
This is only applicable if you’re using Ubuntu 16.04 or lower (which is highly unlikely at the time of updating this article) — but this was a working solution back then. If you have Ubuntu 16.04 for some reason, you can try it out.

Slow or inconsistent wireless connection, in some cases, are also due to Ubuntu’s very own default network manager. I am not sure what causes this but I have seen people in Ubuntu Forums talking about this problem in especially in Ubuntu.

You can install Wicd, an alternative network manager from Ubuntu Software Centre or from the terminal. For details on how to use Wicd, you can read my other article which I used to find SSID of wireless networks in Ubuntu.

Solution 6: More power to wireless adaptor (possibly obsolete)
This trick should be obsolete and this is why I mentioned it in the end. But, it looks like some of you find it working even with Ubuntu 20.04.

Linux Kernel has a power management system which comes in handy. However, for some users with their wireless connection it sent less power to the wireless adapter and thus affecting its performance. As a result, wireless connection would be some times fast and some times dead slow. While this is probably fixed in later Kernels, systems running older Linux Kernel may still face it.

Open a terminal and use the following command:

sudo iwconfig
It will give you the name of your wireless device. Normally it should be wlan0. Now use the following command:

sudo iwconfig wlan0 power off
This will switch off the special power management system for the wireless adaptor and thus it will get more power and will work more (lame attempt at humour).
