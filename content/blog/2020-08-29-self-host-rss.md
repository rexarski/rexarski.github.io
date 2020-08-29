---
title: "Self-host RSS"
date: 2020-08-29T16:52:40+10:00
slug: "self-host-rss"
description: "Three steps to self-host miniflux and rsshub."
keywords: ["rss", "miniflux", "rsshub", "self-host"]
draft: draft
tags: ["rss", "self-host"]
math: false
toc: true
---

**Edit:** According to Miniflux's [documentation](https://miniflux.app/docs/services.html), refreshing feeds is not possible with Reeder since no user information is sent. This means, I can only fetch something new when the server side refreshes (by default, it's once an hour), no matter what syncing frequency I set in the application. That's definitely a bummer. But I will keep an eye on this. Maybe it will eventually become a turning point when I finally quit information overdosing.

> Oceans rise, empires fall. RSS will be back.

My subscription to Inoreader will expire soon. It's been a good year with it, but it was in fact less useful than I expected. From a retrospective point of view, the possibility to subscribe to more than 150 feeds might be the only feature that still seems attractive to me. But hey, why not more than 250, or 500? Although at the same time, I truly believe in the philosophy of "less is more" and "I probably won't have enough time for that".

So, three steps to unleash the true power of a personal RSS subscription service.

1. Set up a Ubuntu server.
2. (Optional) Deploy RSSHub on that server.
3. Deploy Miniflux on that server.


## Set Up A New Ubuntu Server
I'm going to skip some introduction and simply paste the script here. Just follow Docker's [documentation](https://docs.docker.com/engine/install/) to install docker and docker-compose on Ubuntu.

```bash
apt-get update
apt-get dist-upgrade -y
update-alternatives --config editor

# set up docker repository
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
# Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# install docker engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

# install docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
# if failed, create a symbolic link
# sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# verified
docker-compose --version
```

## Deploy RSSHub

[RSSHub](https://docs.rsshub.app/en/) is an open source RSS feed aggregator.

```bash
# docker compose deployment
wget https://raw.githubusercontent.com/DIYgod/RSSHub/master/docker-compose.yml
docker volume create redis-data
docker-compose up -d

# update by removing old containers
# docker-compose down

# docker deployment
docker pull diygod/rsshub
docker run -d --name rsshub -p 1200:1200 diygod/rsshub

# stop container
# docker stop rsshub
# docker rm rsshub
# then repeat installation
```

## Deploy Miniflux

After reading a post from [Luke Singham](https://lukesingham.com/rss-feed-reader/), I've decided pick [Miniflux](https://github.com/miniflux/miniflux) as my self-hosted RSS service. It's simple, looks clean, and easy to deploy.

```bash
mkdir miniflux
cd miniflux
vim docker-compose.yml
```

Remember to keep a record of both pairs of usernames and passwords.

```yaml
version: '3'
services:
  miniflux:
    image: miniflux/miniflux:latest
    ports:
      - "80:8080"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://db_username:db_password@db/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=miniflux_username
      - ADMIN_PASSWORD=miniflux_password
  db:
    image: postgres:latest
    environment:
      - POSTGRES_USER=db_username
      - POSTGRES_PASSWORD=db_password
    volumes:
      - miniflux-db:/var/lib/postgresql/data
volumes:
  miniflux-db:
```

```bash
docker-compose up -d db
docker-compose up -d miniflux
```

So now I can access the Miniflux service by going to my ip address. Input the Miniflux username and password, then we are all good. In addition, I need to enable Fever api in Settings in order to read my feeds in other third party applications like [Reeder](https://reederapp.com/).

Note: the "server" blank to fill in should be my ip/url + `/fever`.