# \(Optional\) VPN Client Setup

> #### Aside: Not Just Database Setup
>
> If you don't already have a VPN setup, it's an inexpensive \(even free\) and worthwhile investment that simplifies some firewall and security concerns. We recommend the use of a VPN client on every instance you intend to access on a regular basis, but this is not always appropriate depending on your workplace requirements. 
>
> For the purposes of this guide, we will assume that you can configure Firewall rules that explicitly allows desirable access and denies access to everyone else. The tools you use to accomplish this are up to you and beyond the scope of the guide.

# OpenVPN: Free, Ubiquitious, Not Quite Plug-and-Play

DigitalOcean has an e[xcellent guide on setting up an OpenVPN Server](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04) on Ubuntu. It is powerful and flexible and has a medium-sized learning curve. 

# NeoRouter: Simple, Powerful, Off-the-Shelf

NeoRouter is simpler and easier to administer than OpenVPN, and while it doesn't offer all of the same capabilities, it is more than sufficient for small to medium teams. The free version supports mesh networking, but NeoRouter Pro is affordable and a good fit for hub-and-spoke style networks \(though it requires an always-on server to be running the Server instance, the footprint is quite small\).

* [NeoRouter Client Installation Guide](http://www.neorouter.com/wiki/index.php/NeoRouterWiki:ClientSetup)
* [NeoRouter Free/Mesh/Pro feature matrix](http://www.neorouter.com/compare)
* [NeoRouter Download](http://www.neorouter.com/downloads)



