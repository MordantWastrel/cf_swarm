# Running Docker: A Simple CF Stack (MySQL, NGINX, Coldfusion)
{% hint style='tip' %}
Clone a Sample Development Environment and Run it (Time Required: 5 Minutes)
Understand What's Going On (Time Required: 15 Minutes)
{% endhint %}

## Clone Our Sample Repo
You only need two things to start working with our sample CF Development stack:

- A working Docker installation and (free) Docker Hub account (from the previous steps)
- On your host machine, make sure nothing is running on ports 80, 443, or 3306 (so shut down any web servers or MySQL servers running in or out of Docker that use those ports)

Clone this repo into an empty directory: 

In your favorite terminal, `cd` into that directory and enter:

`docker-compose up`

Wait for Docker to pull the relevant images; this will be very fast on subsequent starts because Docker will only pull the difference between the images it has previously saved locally and changes on Docker Hub. 

After Docker has pulled all the images it needs, it will start up three services:

- **MySQL** (with a `root` login set in the `docker-compose.yml` file and repeated in the `/config/cfml/simple-cfml.env` file
- **NGINX**, configured to run **cfswarm.localtest.me** and **cfswarm-two.localtest.me** and reverse-proxy CFML requests to the third container:
- **Commandbox and Lucee 5**, configured with a single datasource using the **cfconfig.json** file located in `/config/cfml/cfconfig`. 

NGINX is extraordinarily lightweight and will start up almost immediately. MySQL and Lucee may take 30-60 seconds the first time you run them; subsequent starts will be much faster since MySQL will not have to perform database initialization and Commandbox will already be "warmed up" and won't have to download Lucee or any of the application dependencies. 