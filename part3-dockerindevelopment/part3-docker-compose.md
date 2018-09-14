# docker-compose.yml: A Closer Look

{% hint style='tip' %}
A line-by-line breakdown of our sample Docker-in-Development Stack (Time Required: 10 Minutes)
{% endhint %}

{% hint style='info' %}
Anyone working with Docker should bookmark the official reference document for `docker compose`: https://docs.docker.com/compose/compose-file/
{% endhint %}

Open up the `docker-compose.yml` file in the root of the repository, or view it online: 

https://github.com/inLeagueLLC/simple-cfml-complete/blob/master/docker-compose.yml
{% hint style='info' %}
### What's a YML?

A YML (or properly, 'YAML' for **Y**et **A**nother **M**arkup **L**anguage) file is just a text file with strict formatting rules that govern how the file is divided into sections and how one line relates to the next. YAML files use two spaces (not tabs) for each level of indentation, and Docker is very particular about YAML syntax. 

Your IDE should have a plugin to facilitate YAML syntax, like Visual Studio's [YAML](https://marketplace.visualstudio.com/items?itemName=adamvoss.yaml) by Adam Voss or Sublime Text's [Pretty YAML](https://packagecontrol.io/packages/Pretty%20YAML). 
{% endhint %}
