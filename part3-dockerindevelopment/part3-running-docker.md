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

`https://github.com/inLeagueLLC/simple-cfml-complete.git`

Before we start it up, let's take a look at what we have:

* **/config**: This folder contains files we'll need at run-time when we spin up our containers. This is a convention we'll be using frequently in this guide. We'll put database connection details, CFConfig files, and (some) environment variables in this folder. Note that only the **cfml** folder has any contents -- our application is so simple that we're handling NGINX and MySQL configuration in our **docker-compose.yml** file, which we'll get to shortly.
* **/app-one** and **/app-two**: These folders contain two sample applications: **app-one** showcases how simple a CF-in-Docker application can be. It creates a test database, adds a table, inserts a record, and then dumps that record; **app-two** takes advantage of Commandbox to deploy an entire application with just one file in the repository (**box.json**, which we'll also see more of later)
* **/nginx**: 

In your favorite terminal, `cd` into that directory and enter:

`docker-compose up`

Wait for Docker to pull the relevant images; this will be very fast on subsequent starts because Docker will use its local, cached version of the relevant images.

After Docker has pulled all the images it needs, it will start up three services:

- **MySQL** (with a `root` login set in the `docker-compose.yml` file and repeated in the `/config/cfml/simple-cfml.env` file
- **NGINX**, configured to run **cfswarm.localtest.me** and **cfswarm-two.localtest.me** and reverse-proxy CFML requests to the third container:
- **Commandbox and Lucee 5**, configured with a single datasource using the **cfconfig.json** file located in `/config/cfml/cfconfig`. 

NGINX is extraordinarily lightweight and will start up almost immediately. MySQL and Lucee may take 30-60 seconds the first time you run them; subsequent starts will be much faster since MySQL will not have to perform database initialization and Commandbox will already be "warmed up" and won't have to download Lucee or any of the application dependencies. 

Now navigate to http://cfswarm-simple.localtest.me 
