+++
date = "2017-09-23T00:10:33-05:00"
title = "Connecting to a Meraki L2TP VPN on Fedora 26/Ubuntu 16.04"
draft = false

+++
There are many blogs and forums detailing how to generally set up the L2TP VPN Client on both Ubuntu 16.04 and Fedora 26. 

For Ubuntu 16.04 follow this excellent tutorial: http://blog.z-proj.com/enabling-l2tp-over-ipsec-on-ubuntu-16-04

For Fedora 26 you just have to install this:

    dnf install NetworkManager-strongswan \
    NetworkManager-strongswan-gnome \
    NetworkManager-l2tp-gnome \
    NetworkManager-l2tp

The main issue is not taking into account the particulars of the vpn server you are connecting and phase algorithms used for each [**IKE Phase**](https://documentation.meraki.com/zGeneral_Administration/Tools_and_Troubleshooting/Networking_Fundamentals%3A_IPSec_and_IKE). 

After reading countless forums and comments, I figured out to explicitly set the following phase algorithms.

Phase1 Algorithms: 3des-sha1-modp1024
Phase2 Algorithms: 3des-sha1

So in Network Manager L2TP, under "IPsec settings", it should look like this:
![VPN Config](https://s3.us-east-2.amazonaws.com/marioharvey.com-media/pictures/092017/vpn.png)

So again, this will work with Meraki VPN. Other VPNs using L2TP may differ but this will be something you want to consider when setting up your vpn client.

