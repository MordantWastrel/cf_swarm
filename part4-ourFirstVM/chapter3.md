# Instance Configuration 101

Whether you're setting up your database server, a Docker server, or most anything else, the initial setup is the same -- enough so that we'll create a snapshot once we're done and use that as our starting point for future droplets.

* Login as Root; add a new user for yourself and configure Root privileges
* Add Public Key authentication
* \(Optional\) Disable password logins
* Basic Firewall Configuration

> ### Aside: Provider-Specific Setup Guides
>
> When you first provision an instance, DigitalOcean will email you a link to one or more basic setup guides for the OS you selected. Many providers have similar guides, but even if yours doesn't, the only tasks specific to your provider will be the interaction between the provider-level services \(such as firewalls, DNS control, and load balancers\) and your instance; and these are specific only inasmuch as every provider's control panels interfaces and APIs for these services differ from one another.
>
> There aren't any short-cuts here; you don't need to be a Linux guru, but you need to be comfortable with basic administration tasks like mounting and unmounting, package management, and networking. If you aren't there yet, spend time with these guides until you are.

## Initial Setup Tasks

Follow the link in your welcome email to the initial setup guide, but as of March 2018 it is here:

* [DigitalOcean Ubuntu 14.04 Initial Server Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)

It's a short guide; don't skip any steps.  At the end of this guide, you'll have a cloud instance on the public Internet that is accessible only via SSH -- but it is accessible to everyone via SSH, so let's take the opportunity to deploy DigitalOcean's provider-level firewall and limit SSH connections -- either just to our current computer or perhaps to our work subnet. You can remove or alter these rules later; a VPN is safest, but we haven't installed a VPN client on our instance so we'll use our current public IP address for our test run.

## \(Optional\) Provider-Level Firewall

The Initial Server Setup guide enabled the [Uncomplicated Firewall](https://help.ubuntu.com/community/UFW) \(UFW\) to allow only SSH connections to our Droplet. We'll make further use of UFW later on, and you can use it extensively and exclusively -- but since we'd have to deal with UFW configuration on each and every Droplet, let's have a look at the provider-level Firewall tools.

From your DigitalOcean account, select the **Networking** top menu and the **Firewalls** sub-menu. Select **Create Firewall**.

![](/assets/snip_20180321104448.png)Let's create a simple Firewall rule to restrict SSH access to only our current public IP address.

> #### Aside: Illustration Purposes Only
>
> Limiting Droplet access to a single IP is not desirable in any real-world scenario; we're doing it only to illustrate an example of configuration control on the provider level. Keep in mind that your Droplet is always accessible via console from the DigitalOcean control panel regardless of your Firewall settings. Also, the template configuration we already have restricts everything except SSH connections, and that's "pretty secure"
>
> A likely use case for a similar rule would be to restrict SSH access to anyone on your corporate network or your VPN. This works identically to our example below, but instead of a single IP address, you'd add one or more subnets -- and in the case of a VPN, you'd have to make sure the client is installed on the instance, and our template instance doesn't have a VPN client on it; but yours easily could.
>
> This guide is about rapidly deploying a CF pipeline with reasonable base-line security. Network topology and security requirements require careful consideration that is beyond the scope of this guide.

The default DigitalOcean Firewall configuration blocks all incoming requests except for SSH on port 22.![](/assets/snip_20180321105850.png)Under **Inbound Rules**, remove **All IPv4** and **All IPv6** and add your public Internet address \(from [What Is My IP Address](https://whatismyipaddress.com/)\). Note that you can also input other Droplets, load balancers, or tags \(groups of Droplets\) instead of IP addresses or subnets.

Your **Inbound Rules** section should look like this, only with your IP in place of **1.2.3.4**:![](/assets/snip_20180321110744.png)Once you have a Firewall setup you like, you can apply it to one or more Droplets at any time. You don't actually need to do this now, because we're going to be destroying our template droplet in a moment; but if you wanted to activate this configuration and save it for future Droplets, you'd enter their names at the bottom and select **Create Firewall**.![](/assets/snip_20180321111157.png)

Before we continue, let's take a snapshot of our template instance.

## Take a Template Snapshot

Once you've completed the Initial Server Setup, shutdown your instance with:

```
sudo shutdown -h now
```

You can power off your instance from your provider control panel \(The **Power** sub-menu for your droplet on DigitalOcean\) but it is always best to do so from the command line.

Select your **template** Droplet:![](/assets/snip_20180321103501.png)Then select the **Snapshots** menu and enter a name for your template snapshot. By default, the snapshot will be named after the instance, followed by a unique code; You can remove the trailing -code and just call it **template** since we'll be destroying our Droplet with the same name in a moment and left with only one thing called **template. **![](/assets/snip_20180321103627.png)

## Destroy Your Droplet

Now that we have a snapshot we can use to build all our future Droplets, we don't need this one anymore. Select the **Destroy** submenu for your Droplet.![](/assets/snip_20180321104028.png) 



