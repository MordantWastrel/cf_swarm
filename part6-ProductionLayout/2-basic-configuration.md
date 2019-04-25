# Your First Instance: Basic Configuration & Security

**Time Required: 5-15 Minutes, depending on familiarity with Linux server setup**

Whether you're setting up your database server, a Docker server, or most anything else, the initial setup is the same -- enough so that we'll create a snapshot once we're done and use that as our starting point for future droplets.

* Login as Root; add a new user for yourself and configure Root privileges
* Update your Ubuntu Installation
* Add Public Key authentication
* \(Optional\) Disable password logins
* Basic Firewall Configuration

{% hint style="info" %}
### Aside: Provider-Specific Setup Guides

When you first provision an instance, DigitalOcean will email you a link to one or more basic setup guides for the OS you selected. Many providers have similar guides, but even if yours doesn't, the only tasks specific to your provider will be the interaction between the provider-level services \(such as firewalls, DNS control, and load balancers\) and your instance; and these are specific only inasmuch as every provider's control panels interfaces and APIs for these services differ from one another.

There aren't any short-cuts here; you don't need to be a Linux guru, but you need to be comfortable with basic administration tasks like mounting and unmounting, package management, and networking. If you aren't there yet, spend time with these guides until you are.
{% endhint %}

## Initial Setup Tasks

Follow the link in your welcome email to the initial setup guide, but as of October 2018 it is here:

* [DigitalOcean Ubuntu 18.04 Initial Server Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04)

It's a short guide; don't skip any steps.

{% hint style="info" %}
#### Aside: SSH Key Authentication

Make sure to add user accounts and their respective SSH keys for anyone who needs to access all of your instances. Users that only need to access some of your instances can be added explicitly to those instances later.

For more information or assistance with configuring SSH key-based authentication, see the [DigitalOcean tutorial on Linux SSH Configuration.](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)
{% endhint %}

#### \(Optional\) Update Your OS / Ubuntu Packages

Since we're building a template that we're going to re-use, it's a good idea to keep your OS packages current:

```text
sudo apt-get update        # Fetches the list of available updates
sudo apt-get upgrade       # Strictly upgrades the current packages
```

## Next Steps: Provider Firewall & Snapshot

After completing the Initial Setup Guide, you'll have a cloud instance on the public Internet that is accessible only via SSH -- but it is accessible to everyone via SSH, so let's take the opportunity to deploy DigitalOcean's provider-level firewall and limit SSH connections -- either just to our current computer or perhaps to our work subnet. You can remove or alter these rules later; a VPN is safest, but we haven't installed a VPN client on our instance so we'll use our current public IP address for our test run.

{% hint style="info" %}
#### Aside: UFW -- Your Instance-Level Software Firewall vs. Provider-level rules

If you followed the initial setup guide, then the Uncomplicated Firewall \(UFW\) is enabled on your template instance. We recommend doing so, but most of our Firewall rules in this guide will be configured on the provider-level rather than the instance-level for ease of replication and application management. Once we've done so, UFW is redundant, but that's the point; even though it's not our primary mechanism of refereeing who can access which services on our instances, we'll keep it as a backup.

If you'd like to make better use of UFW \(either in addition to or instead of provider-level rules\), DigitalOcean has a good, basic tutorial for [UFW Essentials: Common Firewall Rules and Commands.](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)
{% endhint %}

