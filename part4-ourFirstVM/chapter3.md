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

#### Initial Setup Tasks

Follow the link in your welcome email to the initial setup guide, but as of March 2018 it is here:

* [DigitalOcean Ubuntu 14.04 Initial Server Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)

It's a short guide; don't skip any steps.  At the end of this guide, you'll have a cloud instance on the public Internet that is accessible only via SSH -- but it is accessible to everyone via SSH, so let's take the opportunity to deploy DigitalOcean's provider-level firewall and limit SSH connections -- either just to our current computer or perhaps to our work subnet. You can remove or alter these rules later; a VPN is safest, but we haven't installed a VPN client on our instance so we'll use our current public IP address for our test run.

Before we continue, let's take a snapshot of our template instance.



