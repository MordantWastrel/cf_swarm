## Take a Template Snapshot

Once you've completed the Initial Server Setup, shutdown your instance with:

```
sudo shutdown -h now
```

You can power off your instance from your provider control panel \(The **Power** sub-menu for your droplet on DigitalOcean\) but it is always best to do so from the command line.

Select your **template** Droplet:![](/assets/snip_20180321103501.png)Then select the **Snapshots** menu and enter a name for your template snapshot. By default, the snapshot will be named after the instance, followed by a unique code; You can remove the trailing -code and just call it **template** since we'll be destroying our Droplet with the same name in a moment and left with only one thing called **template. **![](/assets/snip_20180321103627.png)

## Destroy Your Droplet

Now that we have a snapshot we can use to build all our future Droplets, we don't need this one anymore. Select the **Destroy** submenu for your Droplet.![](/assets/snip_20180321104028.png)

