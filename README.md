# Mycroft Docker Development Environment

[![Codefresh build status]( https://g.codefresh.io/api/badges/build?repoOwner=MycroftAI&repoName=docker-mycroft&branch=master&pipelineName=docker-mycroft&accountName=btotharye&type=cf-1)]( https://g.codefresh.io/repositories/MycroftAI/docker-mycroft/builds?filter=trigger:build;branch:master;service:599b524e9accf20001956e6d~docker-mycroft) [![Build Status](https://travis-ci.org/MycroftAI/docker-mycroft.svg?branch=master)](https://travis-ci.org/MycroftAI/docker-mycroft)

## Running image from dockerhub
This repo is updated on dockerhub at https://hub.docker.com/r/mycroftai/docker-mycroft/ and you can run it without building by simply running:

Just replace the directory_on_local_machine with where you want the container mapped on your local machine, IE /home/user/mycroft for example if you created a mycroft folder in your home directory.  This is so the pairing file is stored outside the container.

`docker run -itd -p 8181:8181 -v directory_on_local_machine:/root/.mycroft mycroftai/docker-mycroft`

## How to build and run

1. Git pull this repository - ```git clone https://github.com/MycroftAI/docker-mycroft.git```

2. Build the docker image with 
   ```docker build -t ${USER}/mycroft .``` in the directory that you have checked out.
   
3. Run the following to start up mycroft:
   ```docker run --device /dev/snd:/dev/snd -itd ${USER}/mycroft```
   
4. Want a interactive cli session to register the device and test things, then run the following and type pair my device to start, we are mounting a local filesystem into the container so we can store our Identity file to reuse this same device over and over on new containers:
   ```docker run -it -p 8181:8181 -v /path_on_local_device:/root/.mycroft ${USER}/mycroft /bin/bash /mycroft/ai/mycroft.sh start -d```

5. Confirm via docker ps that your container is up and serving port 8181:


```
docker ps
CONTAINER ID        IMAGE                                                COMMAND                  CREATED             STATUS              PORTS                                            NAMES
692219e23bf2        user/mycroft                                    "/mycroft/ai/mycro..."   3 seconds ago       Up 1 second         8181/tcp                                         amazing_borg
```

### CLI Access
You can interact with the CLI of the container by running the following command, this will connect you to the running container via bash:

```
docker exec -it container_name /bin/bash
```

You can exit this container safely and leave it running by hitting cntrl + p + q, otherwise you can just hit cntrl+c to exit the cli and it will exit the container.  If you exit with cntrl + p + q it will leave the session open and running, still seeing issues attaching to sessions with previously running cli sessions though, so be advised.


You can get the container name via:

```
docker ps
```



```
Quickly start, stop or restart Mycroft's esential services in detached screens

usage: /mycroft/ai/mycroft.sh [-h] (start [-v|-c]|stop|restart)
      -h             this help message
      start          starts mycroft-service, mycroft-skills, mycroft-voice and mycroft-cli in quiet mode
      start -v       starts mycroft-service, mycroft-skills and mycroft-voice
      start -c       starts mycroft-service, mycroft-skills and mycroft-cli in background
      start -d       starts mycroft-service and mycroft skills in quiet mode and an active mycroft-cli
      stop           stops mycroft-service, mycroft-skills and mycroft-voice
      restart        restarts mycroft-service, mycroft-skills and mycroft-voice

screen tips:
            run 'screen -list' to see all running screens
            run 'screen -r <screen-name>' (e.g. 'screen -r mycroft-service') to reatach a screen
            press ctrl + a, ctrl + d to detace the screen again
            See the screen man page for more details
```


# Known Issues
There is currently a issue where skills are not being installed correctly on load and is being investigated, for now you will need to `docker run exec -it container_name /bin/bash` to get into the container and run msm/msm default to try and get them to load, you can also just install the skills from the main location at https://github.com/MycroftAI/mycroft-skills
