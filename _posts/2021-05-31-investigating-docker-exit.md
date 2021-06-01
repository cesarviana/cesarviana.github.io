---
title: Docker exiting
tags: [Docker, Linux]
color: primary
description: Investigation of how an docker container is exiting
---
One website is exiting. We use docker for it. My idea here is to describe the process of searching for the cause of this problem and solving it. I'll write where follow the debugging process.

What I have: 
1. Error 500 on the browsers page. After running `docker-compose up --build -d`, all works fine.  
2. This is the second time it occurs. I have the time when the sites went down, because an automatic website monitor sent me and e-mail.

## Context
Fist, we have three containers for this site. Backend, Frontend and Database. I guess that the backend have the problem, because the favicon of frontend is still visible.

Steps for solving problem.
1. Using `history` command to see if my brother restarted the container earlier
To filter by date, use the `HISTTIMEFORMAT="%d/%m/%y %T"` command. After that, use grep to filter by 29/5. Result: No logs from this day, only today. 

Second try. Running `ncdu` to see if there is some storage problem... 11GB in use. We have 20. Near 7GB is used by archiva. So, this is not the problem. 

Third try. Follow logs for each container. Starting with backend. How see the logs? Google... `docker logs CONTAINER` is useful only to runtime logs. 

[Follow this tutorial](https://www.tutorialworks.com/why-containers-stop/). It says to follow logs.

`docker inspect CONTAINER_ID`
`docker stats`
`docker stats --format "table {{.Name}}\t {{.MemPerc}}"`

I can see that there is no current higher memory usage. Lets back to search for stored logs.

Searching on google, I found where the logs are stored:
`/var/lib/docker/containers/<container_id>/<container_id>-json.log`.
`docker ps` shows the containers IDs. The json.log has dates. Maybe I can filter by the date the container has stopped.
Or, we can use `docker logs <container_id> -t`.

I was yet unable to find the problem. But, with the simple `htop` command I can see that the backend.jar is consuming at least 15% memory. I'll change the name of the jar to be simple distinct this application from others.

## Second day
Today, a process was consuming 50% of server's memory. Almost no memory available...
![](https://i.ibb.co/PwqD8nm/Whats-App-Image-2021-06-01-at-08-47-49.jpg)
Docker `docker stats` show that the bad process id tvgaspar_frontend.
![](https://i.ibb.co/2kK6NF3/Whats-App-Image-2021-06-01-at-08-50-21.jpg)

Thinking about changes, the only one I can remember is adding the countdown banner. Maybe the `thickTimer` function is creating a memory leak.

```javascript
tickTimer() {
    // removed now = this.$moment()
    const now = new Date()
    if(this.parsedDate.isAfter(now))
    {
        const remainingTime = this.$moment.duration(this.parsedDate.diff(now));
        this.months = remainingTime.months();
        this.days = remainingTime.days();
        this.hours = remainingTime.hours();
        this.minutes = remainingTime.minutes();
        this.seconds = remainingTime.seconds();
    }
}
```