# \(Optional\) VPN Client Setup

> #### Aside: Not Just Database Setup
>
> If you don't already have a VPN setup, it's an inexpensive \(even free\) and worthwhile investment that simplifies some firewall and security concerns. We recommend the use of a VPN client on every instance you intend to access on a regular basis, but this is not always appropriate depending on your workplace requirements.
>
> For the purposes of this guide, we will assume that you can configure Firewall rules that explicitly allows desirable access and denies access to everyone else. The tools you use to accomplish this are up to you and beyond the scope of the guide.

# OpenVPN: Free, Ubiquitious, Not Quite Plug-and-Play

DigitalOcean has an e[xcellent guide on setting up an OpenVPN Server](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04) on Ubuntu. It is powerful and flexible and has a medium-sized learning curve.We recommend a couple of tweaks to the guide:

* [Add the official OpenVPN repository](https://community.openvpn.net/openvpn/wiki/OpenvpnSoftwareRepos) to get the latest stable release; as of April 2018, the Ubuntu repositories are about 9 months behind. This is likely to change with the forthcoming release of Ubuntu 18.04, so if the DigitalOcean guide is updated for 18.04 and OpenVPN 2.4+, you can disregard this section.
* Your OpenVPN server configuration may need the **multihome** option \(just add **multihome** on a line by itself anywhere in the **.conf** file\) or else TCP rather than UDP due to DigitalOcean's networking implementation. 

If you do use the latest OpenVPN release \(2.4 rather than 2.3 in the Ubuntu repository\), there are a few easy changes you can make for better performance:

* Replace **tls-auth ta.key 0** with **tls-crypt ta.key** on the server configuration and **&lt;tls-auth&gt;** with **&lt;tls-crypt&gt;** on the client configuration \(and in the **make\_config.sh **script\).
* Replace **comp-lz0** with **compress lz4** on both the server and client configurations as **comp-lz0** is deprecated.

> #### Aside: Quick 'n Dirty OpenVPN Install
>
> Nyr maintains a "Road Warrior" CLI [OpenVPN installation and management utility](https://github.com/Nyr/openvpn-install) that will get you up and running faster.

# NeoRouter: Easy to Install, Ample Features, Slower

NeoRouter is simpler and easier to administer than OpenVPN, and while it doesn't offer all of the same capabilities, it is more than sufficient for small to medium teams. The free version supports mesh networking, but NeoRouter Pro is affordable and a good fit for hub-and-spoke style networks \(though it requires an always-on server to be running the Server instance, the footprint is quite small\).

> #### Aside: VPN Speed
>
> NeoRouter is much slower than OpenVPN, likely due to the age of the network driver used for their interface. DigitalOcean tests showed NeoRouter Pro p2p performance at around 9-18 Mbits/sec versus 189-240 MBits/sec for OpenVPN. If you just want a working VPN, NeoRouter will get you there a little faster, but OpenVPN may be a better investment of time in the long run.

* [NeoRouter Client Installation Guide](http://www.neorouter.com/wiki/index.php/NeoRouterWiki:ClientSetup)
* [NeoRouter Free/Mesh/Pro feature matrix](http://www.neorouter.com/compare)
* [NeoRouter Download](http://www.neorouter.com/downloads)



