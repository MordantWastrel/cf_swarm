# How Many Instances \(Droplets\)?

There is no definitive answer to this question, but at a minimum we'll need a host running Docker.

* **Maximum Cattle: **We could run all our other services \(database, web server, application server\) in Docker containers.   
* **Maximum Pets: **We could drop Docker entirely and provision separate Droplets for each of our services: a database droplet, a application \(CF\) droplet, and a front-end web droplet.  It's a bit anachronistic and will be harder to scale later, but it gets us into the cloud.
* **Hybrid: **We'll provision one droplet to run our database server natively on our instance OS, but for everything else, we'll use Docker. 

This guide will take the **Hybrid** approach. [As the pets/cattle slide says](/README.md), "pets with strong configuration management are still needed." Unless we need a distributed database, docker-izing our RDBMS is overkill. We'll have to be sure to satisfy the "strong configuration management" piece, but for an introduction to Docker in a short amount of time, let's focus just on the application server and the front-end web server.

## Instances or Droplets?

The examples in this guide happen to use DigitalOcean, which refers to their cloud computing instances as "Droplets." When we use this term in an example or a screenshot, we're referring to what most other providers call "instances" or "cloud instances."  We'll say "Droplet" just so that the language in the guide agrees with the language in the screenshots.



