# MySQL

**Time Required: 10 minutes \(Setup: 5 minutes; Data relocation: 5 minutes\)**

DigitalOcean provides an excellent [MySQL setup guide](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-16-04) as well as a ready-made MySQL instance image. We're going to use follow the guide, using the template image we created in Part 4 and prepared in [Part 5: Instance Prep](/part5-Database/chapter2-InstancePrep.md).

Follow the guide exactly and do not skip any steps.

> #### Aside: Remote Root Logins
>
> One of the selections during the **mysql secure installation** script is an option to disable remote root logins to ensure that no-one can guess at the root password from the network. This is a good idea, but we'll want to block anyone not on our network \(whether a VPN, a work subnet, or just within our provider's network, e.g. other droplets\) from connecting at all. If we can do that, allowing remote root mysql logins is less bad.

Once you've completed the basic setup guide, be sure to continue to the instructions on [How to Move a MySQL Data Directory](https://www.digitalocean.com/community/tutorials/how-to-move-a-mysql-data-directory-to-a-new-location-on-ubuntu-16-04) so we can take advantage of block storage. Just remember to use the mount point you named in Instance Prep: The guide uses **/mnt/db**, so if you did the same then you'd use that instead of the example in DigitalOcean's guide of **/mnt/volume-nyc-01**.

