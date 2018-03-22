# How Many Instances \(Droplets\)?

There is no definitive answer to this question, but at a minimum we'll need a host running Docker.

* **Maximum Cattle: **We could run all our other services \(database, web server, application server\) in Docker containers.   
* **Maximum Pets: **We could drop Docker entirely and provision separate Droplets for each of our services: a database droplet, a application \(CF\) droplet, and a front-end web droplet.  It's a bit anachronistic and will be harder to scale later, but it gets us into the cloud.
* **Hybrid: **We'll provision one droplet to run our database server natively on our instance OS, but for everything else, we'll use Docker. 

This guide will take the **Hybrid** approach. [As the pets/cattle slide says](/README.md), "pets with strong configuration management are still needed." Unless we need a distributed database, docker-izing our RDBMS is overkill. We'll have to be sure to satisfy the "strong configuration management" piece, but for an introduction to Docker in a short amount of time, let's focus just on the application server and the front-end web server.

> #### Aside: Instances or Droplets?
>
> The examples in this guide happen to use DigitalOcean, which refers to their virtual machine hosts as "Droplets." When we use this term in an example or a screenshot, we're referring to what most other providers call "cloud compute instances."  We'll typically say "an instance," but it's interchangable with "a Droplet" \(or "a Linode" if Linode is your provider\)

### In This Section, We Will...

* Provision our first cloud compute instance
* Add a basic, replicable configuration template: logins for ourselves \(and anyone else who should have access to all our instances\), public key configuration, disabling password logins, and basic UFW firewall setup
* Test a provider-level firewall template to complement or individual instance security
* Take a snapshot of our instance to use as a starting point for future instances





