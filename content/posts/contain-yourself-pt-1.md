+++
title = 'Contain Yourself Pt 1'
date = 2022-07-31T00:00:00-00:00
draft = false
+++

When a fellow developer at previous job first recommended that we use containers for our product, I almost spit out my drink. "Preposterous!" I wagged my finger in the air. "We have a perfectly good solution for building and shipping our software. What's wrong with devs spending 3-5 days getting their environment to match the other devs 3-5 times a year, or manually deploying by SSHing in and pulling from the git repository on the live server?!"

(insert meme here of me in 2016 laughing at fellow dev for suggesting we use docker)

Needless to say, things have changed and containers are in vogue. Although they're not ["new"](https://www.redhat.com/en/blog/history-containers) by any means, we have finally reached that threshold where more web developers are actively using containers to build and ship software than not, and even a lot of non-web software is benefiting from containerization, especially in the world of cybersecurity. The vast majority of these deployments are using [Docker](https://insights.stackoverflow.com/survey/2021#most-popular-technologies-tools-tech-prof), though some other containerized variants are also sometimes used. Containers are one of the most useful and standardized tools available to software developers today, right up there with [`git`](https://git-scm.com/).

(insert meme here of me in 2022 laughing at fellow dev for suggesting we use anything other than docker)

This is meant to be a very cursory primer on Docker, and it will be the foundation for a lot of other topics, since the tools I use are often distributed in the form of Docker images, and the technologies that power cloud infrastructure often revolve around the management of containers. If you want to skip to the good stuff where we ~~hack things~~ run Docker, head to the section below: (insert skip link)

## Why containers?

To answer this question, I think it's helpful to understand where we were before containers were widely adopted. A long time ago (10 years might as well be a century in tech), engineers were left to their own devices when shipping their software. Building it was a part of the core competency of being a developer; deploying it was not. If they were lucky, they had an operations team that they could hand their software to for managing the later stages of the development lifecycle. Either way, offices were sometimes filled with screaming matches between engineering and operations, project management, and executive teams as bugs popped up in production that could not be reproduced locally because "it works on my machine!" Getting a development environment stood up was usually a week-long task at minimum, assuming that the process was even documented to begin with. There were very few consistently updated manifests outlining all dependencies and configuration needed for a project to work, and updates were slow and often produced disastrous consequences. Sometimes, when servers in production failed, the configuration for those servers died as well, so they would have to be (often manually) re-provisioned. Engineers would simply hope that they remembered the right sequence of commands to get a server back online. Looking back on it, I don't know how the hell anyone got anything done.

I also can't tell you how many times I have almost ragequit because I could not install multiple versions of the same package in whatever language I was working in on my developer machine, or updating a specific dependency just broke everything for seemingly no reason. I sometimes labored and toiled for days dealing with issues related to working on multiple projects. Each would end up cross-contaminating every other project, leading to a mess that made me question why I ever got into software in the first place. The solutions that we had before containers were inconsistent and imperfect at best, and painful and counterproductive at worst. Spinning up a full blown Linux virtual machine and installing dependencies there was extremely resource-intensive, slow, ate up a ton of disk space, and even then could be wildly variable. Although there were some tools available for managing [multiple environments for python](https://m.xkcd.com/1987/) and other languages, the tools have often been messy and difficult to manage. Containers solve these problems by packaging the software needed to run your container within the container image itself, unaffected by the host that runs the container, so that you're not constantly banging your head against your desk at 3am resolving (insert some package management horror story).

Docker was not the hero we deserved, but it was the one we needed right then. With Docker, engineers were able to much more easily and repeatably build and test their containers locally, and sometimes ship those exact container images directly to production. This meant more consistency between local, test, and production environments. Standing up servers became a much less painful task; all that was needed was a Docker agent, a way to access the container images, and the ability to talk to and follow the instructions of a container orchestrator.

## So what is a container anyway? 
From the [Docker website](https://www.docker.com/resources/what-container/):
```
A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.
```
In other words, this gives us a way to run the software that we build anwhere Docker can run, and it can work exactly the way it does in the cloud that it does locally. No more "it works on my machine!" A container is nothing more than a self-contained file system. That's it. It is a "box" that "contains" a file system. It shares resources with the host that it runs on for memory and compute capabilities. Volumes *can* be mounted for shared access; otherwise the file system inside the container is completely independent of the host.

## Advantages of containers
Docker acolytes will revel you in the litany of benefits that shall be bestowed upon you from the Linux gods by opening your eyes and following the Docker way!  And I, um, mostly agree with them.

### Portability
Docker containers can run anywhere that the Docker engine can be installed. Docker can now run on nearly any modern computer with nearly any architecture, so this means that containers can run nearly everywhere.

### Disposability
Containers are meant to run for a little while, die, then get replaced by another container. This is the heart of their value; their specific function is to perform a single unit of work for as long as they're needed, and then go away. This means that if a [server dramatically explodes](https://www.youtube.com/watch?v=ZhUnSOBTbM8) , a container can be spun up on a new server without delay and without needing to fully re-provision all of its dependencies. 

### Scalability
All of the software that is needed to run within a container is already in the container. Scaling up a service is typically as easy as telling your container orchestrator (come up with an explanation here) that you need more containers, and as long as you have the compute resources, voila! You have more container minions to do your bidding. Thanks to certain technologies like [AWS Fargate](https://aws.amazon.com/fargate/), we don't even need to provision servers to run containers anymore if we decide we don't want to; you can simply tell AWS how many Fargate containers and in what size you need and they will handle the infrastructure for you.

## Isolation
Changes to a container image only impact that image, and not the other containers you have running on a host. You could theoretically run two completely separate versions of Ruby on Rails on two different containers on the same host machine without either impacting the other.

## Disadvantages of containers

I know what you're thinking: "Wait, shouldn't I always run containers?" Definitely not. You have to be aware of some the pitfalls before you switch over to a containerized environment. Also, there are still ways that you can use containers badly. Containers do not replace the need for good engineering practices, and in fact, they can make it harder to deal with bad engineering practices if you do not have the foundations in place. None of these are reasons to avoid containers altogether, it's just important to know what you're getting into before you dive in.

### Complexity
Containers have their own considerations for things like network topology. The hosts needs to be secured in tandem with the containers that run them. An orchestrator is needed to manage the containers, and sometimes a service mesh is also required to properly segment and route traffic. Deploying containers into production for the first time can be an extremely daunting task.

### Monitoring
If you run containers, you need to come up with some solution for monitoring containers in addition to the hosts they run on, which includes application and system logs as well as resource utilization. This kind of telemetry can be expensive to instrument and adds a moderate amount of complexity to the deployment.

### Storage
The stateless nature of containers often means that you need to manage some solution to persist data that your containers interact with, whether that's a volume that is mounted to the container or an external database that you connect to. Separating compute and storage resources is a good tenant of stateless design; however, it is something that needs to be accounted for when shipping containers.

Containers can also take up a lot of space, especially if you do not regularly prune unused containers and images, or if you don't use lightweight images such as [Alpine Linux](https://www.alpinelinux.org/). Since the idea is that individual pieces of software get their own operating system, this can lead to needing to handle tens or hundreds of gigabytes of container images for multiple tools and projects. This is no small investment, even considering that storage is getting more accessible.

### Debugging
Since the code that's being written is being executed within the container itself, this means that attaching a debugger to step through the code is sometimes an exercise in patience and [creativity](https://stackoverflow.com/questions/40233928/how-to-remote-debug-python-code-in-a-docker-container-with-vs-code).  Typical options often involve attaching directly to the TTY of the container, installing dev-specific dependencies and attaching through ports that are only open on the "dev" version of that container, or creating a local virtual environment outside of the container to debug.

# Cool!  How do I get started?
If none of that scared you away, let's jump in!  Perhaps my favorite part about containers is that I can just run something that someone else built without the possibility of bricking my machine in the process of compiling binaries and resolving package dependencies. 

To get started, install Docker. You should be able to do this without needing to sign up for anything; just follow the instructions on the Docker website for the operating system that you're running: https://docs.docker.com/get-docker/ 

For this example, we're going to run two containers; one to stand up an extremely minimal web server, and another to run a scan on your local IP to detect that web server.

#### Simple python web server

This will spin up a very simple web server on port 8080.  The web server doesn't "do" anything except listen for incoming connections, but we will be able to momentarily detect it using another tool.

Once you have docker installed, run this command in your terminal. Although it may seem like nothing has happened, leave this terminal running.
`docker run -p 8080:8111 python:slim python -m http.server 8111`

Here is the breakdown of what this command does:
`docker` - this is the Docker engine binary
`run` - we want to run a container
`--rm` - will remove the container when the process exits
`-p 8080:8111` - binds port 8080 of the host to port 8111 of the container while it's running
`python:slim` - we want to start a container from this image; it is a very small but current image containing python
`python -m http.server 8111` - starts a python web server on port 8080 of the container.  Since we mapped port 8080 of the host to port 8111 of the container, any requests to port 8080 on the host will reach the web server on port 8111 of the container

#### Recon on the webserver

We're cheating, I know, I know. We can accurately predict that if we run an nmap scan on our local host, that we'll find a web server running on port 8080, right? That's right.  `nmap` is one of the most prolific security scanning tools out there. There are so many cool things that you can do with `nmap`, though we will stick to the basics for the sake of this example. In this case, we are going to use the default options in order to scan our local machine for some of the most common open ports. 

In a separate terminal, run the following command:
`docker run --rm instrumentisto/nmap:7 $(ipconfig getifaddr en0)`

Here's the breakdown:
`docker` - Docker engine binary
`run` - run a container
`--rm` - will remove the container when the process exits
`instrumentisto/nmap:7` - run a container using this image; this is the public image containing the `nmap` utility. We are running with the last image tagged `7`, which runs the latest version of nmap where the major version number is 7
`$(ipconfig getifaddr en0)` - on Mac OSX and Linux, this should grab your local IPV4 address.  This value is supplied as an argument to `nmap` . On Windows Powershell, you may be able to replace this with  `$localIpAddress=((ipconfig | findstr [0-9].\.)[0]).Split()[-1]` . If that does not work, you should be able to replace this with the local IP of the host that you're running these commands on and see similar results.


You will see something similar to the following:

```
$  docker run --rm instrumentisto/nmap:7 $(ipconfig getifaddr en0)
Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-30 06:33 UTC
Nmap scan report for 192.168.1.101
Host is up (0.0013s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE SERVICE
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 0.15 seconds
```

Use `ctrl + c`  to stop the container in the first terminal. There, you can see that we just successfully detected that our webserver is running on our local IP. I want you to consider that no matter where you ran this example, the only application that you specifically had to install was the Docker engine, and that was a one-time investment. Also, if you were to run these commands again, it would not require you to re-download the images because they are cached locally. 

# Closing Thoughts 
I love containers. They have objectively changed my life for the better by handling the abstraction that allows me to focus on the parts of software engineering that give me energy instead of frustrating me. I am certain that many other engineers feel the same way. All that said, do *not* go out and try to replace every piece of software in your company with the containerized version of it; you will experience pain and suffering, and you will regret it.  Containers are a journey, and it takes a radical shift in thinking to adopt them successfully. As with all things that are worth it, this takes time, patience, and persistence.

Most of my code examples will likely use Docker containers. Most of these will be some flavor of python. I want to set you up for success, because there is very little more frustrating than trying to learn a concept while fighting the technology the powers the example. I have spent much of my career diagnosing issues related to tools that I was not yet familiar with. The inability to access a working example quickly has steered me away from certain career paths because of the complexity involved, and I want to help others avoid that if at all possible. Later, we will revisit containers and go deeper into how containers are built, how they work under the hood, how they get orchestrated, and so much more.

For some additional resources, check out the links below:
* Comprehensive Docker tutorial - https://takacsmark.com/getting-started-with-docker-in-your-project-step-by-step-tutorial/
* Official Docker Getting Started Guide - https://docs.docker.com/get-started/
* Play with Docker - https://labs.play-with-docker.com/