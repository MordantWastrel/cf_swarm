# Running Docker: A Simple CF Stack (MySQL, NGINX, Coldfusion)
{% hint style='tip' %}
Clone a Sample Development Environment and Run it (Time Required: 5 Minutes)
Understand What's Going On (Time Required: 15 Minutes)
{% endhint %}

## Clone Our Sample Repo
You only need two things to start working with our sample CF Development stack:

- A working Docker installation and (free) [Docker Hub](https://hub.docker.com) account (from the previous steps)
- On your host machine, make sure nothing is running on ports 80, 443, or 3306 (so shut down any web servers or MySQL servers running in or out of Docker that use those ports)

Clone this repo into an empty directory: 

`https://github.com/inLeagueLLC/simple-cfml-complete.git`

Before we start it up, let's take a look at what we have:

* **/config**: This folder contains files we'll need at run-time when we spin up our containers. This is a convention we'll be using frequently in this guide. We'll put database connection details, CFConfig files, and (some) environment variables in this folder. Note that only the **cfml** folder has any contents -- our application is so simple that we're handling NGINX and MySQL configuration in our **docker-compose.yml** file, which we'll get to shortly. But it's a good idea to create a config folder for any container you plan on using regularly
* **/app-one** contains a sample aoplication making use of our datasource: it showcases how simple a CF-in-Docker application can be. It creates a test database, adds a table, inserts a record, and then dumps that record.
* **/app-two** contains just one file -- **box.json**. It takes advantage of Commandbox's ability to deploy an entire application. 
* **/nginx** contains the virtual host definition files we need to proxy requests from our local machine to the CF container. It will become the **/etc/nginx** directory inside the NGINX container.

{% hint style='info' %}
#### Why NGINX?
It's not necessary to use any front-end server for local development; your favorite CF engine can serve everything you need. So why bother with the extra work?

* **It's easy**. As you'll see in a moment, NGINX is very small, very fast, and very easy to configure.
* **It makes developing multiple containers easy**. If you work on more than one app (as we will in this sample), you have to keep track of port numbers for each of your CFML containers. NGINX means we don't have to worry about this.
* **Development should resemble Production**. It's unlikely that your production environment won't use a front end server, and we feel that having NGINX in front of your CF engine is the best practice for modern CF application development and deployment.

#### What about Apache (or IIS?)
While NGINX is very easy to learn, any front-end web server capable of a reverse proxy will get the job done. In this guide, we won't use IIS or any web server that can't be part of our Docker stack in development; while it's possible to expose your CF container(s) to any web server, that's beyond the scope of this guide for the time being. 

#### (Adobe Only) What about Connectors?

If there's a reason to bother with Adobe's connectors in a Docker deployment (even their now two-year-old [NGINX Connector 'Prerelease'](https://coldfusion.adobe.com/2016/10/prerelease-build-of-nginx-connector-for-coldfusion-2016-now-available/)), I'm not aware of it. A reverse proxy does the same job with fewer moving parts.
{% endhint %}
In your favorite terminal, `cd` into that directory and enter:

`docker-compose up`

Wait for Docker to pull the relevant images; this will be very fast on subsequent starts because Docker will use its local, cached version of the relevant images.

After Docker has pulled all the images it needs, it will start up four services:

- **MySQL** (with a `root` login set in the `docker-compose.yml` file and repeated in the `/config/cfml/simple-cfml.env` file
- **NGINX**, configured to run **cfswarm.localtest.me** and **cfswarm-two.localtest.me** and reverse-proxy CFML requests to the third container:
- Two instances of **Commandbox ruinning Lucee 5**, configured with a single datasource using the **cfconfig.json** file located in `/config/cfml/cfconfig`. 

NGINX is extraordinarily lightweight and will start up almost immediately. MySQL and Lucee may take 30-60 seconds the first time you run them; subsequent starts will be much faster since MySQL will not have to perform database initialization and Commandbox will already be "warmed up" and won't have to download Lucee or any of the application dependencies. 

When Commandbox and your CF engine are ready, you'll see output similar to this in your terminal:

![Figure 3.4: Commandbox Finishes Starting Up ](/assets/commandbox_ready.png)

Now navigate to http://cfswarm-simple.localtest.me and let's test our first application:

![Figure 3.5: A simple CF page accesses our MySQL Database ](/assets/cfswarm-simple-one.png)

Now navigate to http://cfswarm-two.localtest.me and let's test our second application:

![Figure 3.6: Coldbox downloaded and deployed the application based off our box.json](/assets/cfswarm-simple-two.png)

Look again in your **app-two** folder and you'll see an entire application that wasn't there before. When Commandbox started the server, it read **box.json**, downloaded all the dependencies, and deployed them without our having to do anything.

{% hint style='info' %}
### What happened in each of the containers?

If you want to see the startup process for your containers, use `docker logs [container name]` -- in this case, `docker logs cfswarm-mysql`, `docker logs cfswarm-cfml`, and `docker logs cfswarm-two-cfml` -- the last one will show you how Commandbox deploys the application from `box.json`!
{% endhint %}

In the next section, we'll go step-by-step through the file that coordinates all of our services with Docker: **docker-compose.yml**.
