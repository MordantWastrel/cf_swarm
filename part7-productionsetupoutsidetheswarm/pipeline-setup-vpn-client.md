# Pipeline Setup: (Optional) VPN Server

Whether and to what extent you need a VPN in your production pipeline will depend on how your company network is configured. Out shop is a virtual office, so our developers have to be able to reach our production instances from anywhere. There are a number of ways to solve this problem, and most providers allow some form of console access right from their web-based control panels; but we'd like our own network path to all the services we're going to set up in the cloud and we don't want to have to be messing with firewall rules every time we add or lose a team member. This means a VPN server and VPN clients on all of our droplets.

## OpenVPN: Free, Ubiquitious, Not Quite Plug-and-Play

DigitalOcean has an [excellent guide on setting up an OpenVPN Server](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-18-04) on Ubuntu. It is powerful and flexible and has a medium-sized learning curve. 

{% hint style="info" %}
### Aside: Quick 'n Dirty OpenVPN Install

Nyr maintains a "Road Warrior" CLI [OpenVPN installation and management utility](https://github.com/Nyr/openvpn-install) that will get you up and running faster.
{% endhint %}

## Our Recommendation: Pritunl
There are a number of OpenVPN-derived products that package the same server with accessible management utilities. This is often a trade-off, either in terms of reduced functionality or else paid support, but if you don't have DevOps engineers with dedicated expertise, you'll be well served (as we were) with web-based OpenVPN implementations like [Pritunl](http://www.pritunl.com), whose free tier is adequate for our needs while offering plenty of bells and whistles if and when you need to scale up.

## Configuring Your VPN

For the purposes of this guide, we will assume that you can configure Firewall rules that explicitly allows desirable access over a VPN and denies access to everyone else. The tools you use to accomplish this are up to you and beyond the scope of the guide, but it's trivial to manage even a simple firewall like UFW to allow access to some or all ports over your VPN network interface while blocking access over a public interface. 