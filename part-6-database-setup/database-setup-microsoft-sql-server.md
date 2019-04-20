# Database Setup: Microsoft SQL Server

Time Required: **10-15 Minutes \(Setup: 5-10 Minutes; Data Relocation: 5 Minutes\)**

While DigitalOcean doesn't have a tutorial for installing Microsoft SQL Server, Microsoft provides a simple quickstart guide for DigitalOcean:

* [Microsoft SQL Server Linux Cloud Install Quickstart](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-clouds#digital-ocean), which covers provisioning for AWS, DigitalOcean, and Google Cloud; each of these then link to
* [Microsoft SQL Server Ubuntu Quickstart Guide](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu)

{% hint style="info" %}
### Aside: Earlier Versions of Microsoft SQL Server \(or Windows-based SQL Installations\)

If you're not ready \(or licensed\) to use SQL Server 2017 or just don't want to run it on Linux, you'll need a Windows instance. Vultr, Google Cloud, and AWS \(or Lightsail\) all provide these for a marginal additional cost. Some Google-fu will suggest that you can puzzle your way through installing Windows with your own licensing on a DigitalOcean droplet, but as of June 2018 this is unsupported.

If you can move to SQL Server 2017 for Linux but don't know where to begin, Microsoft provides an [FAQ for SQL Server on Linux.](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-faq)
{% endhint %}

## Moving the Default Data, Log, and Master DB Directories

Once you've completed the basic setup guide, you'll want to change the default file directory location for the master database and backup directory location so we can take advantage of block storage. These options \(along with several other popular configuration directives\) are documented in [Configuring SQL Server on Linux](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-mssql-conf#datadir). Just remember to use the mount point you named in Instance Prep: The guide uses **/mnt/db**, so if you did the same then you'd use that instead of the example in the Microsoft documentation of **/tmp/masterdatabasedir**.

## Allow Ports 1433 and 1434 through your OS Firewall

Most of our access control will come from the provider-level firewall rather than UFW, so for now we'll just open port 1433 \(and port 1434 for the admin console\) on UFW and deal with the bulk of the firewall rules in the next section.

```text
sudo ufw allow 1433
sudo ufw allow 1434
```

