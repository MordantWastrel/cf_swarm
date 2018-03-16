# How Many VMs \(Droplets\)?

There is no definitive answer to this question, but at a minimum we'll need a host running Docker.

* **Maximum Cattle: **We could run all our other services \(database, web server, application server\) in Docker containers.   
* **Maximum Pets: **We could drop Docker entirely and provision separate Droplets for each of our services: a database droplet, a application \(CF\) droplet, and a front-end web droplet.  It's a bit anachronistic and will be harder to scale later, but it gets us into the cloud.
* **Hybrid: **We'll provision one droplet to run our database server natively on our VM OS, but for everything else, we'll use Docker. 

This guide will take the **Hybrid** approach. [As the pets/cattle slide says](/README.md), "pets with strong configuration management are still needed." Unless we need a distributed database, docker-izing our RDBMS is overkill. We'll have to be sure to satisfy the "strong configuration management" piece, but for an introduction to Docker in a short amount of time, let's focus just on the application server and the front-end web server.

# Your First Droplet: Basic Setup & Security 

If you're already familiar with provisioning and configuring a VM \(droplet\) and just want to get to Docker, you can skip this section. 







