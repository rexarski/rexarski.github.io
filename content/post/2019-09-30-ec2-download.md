---
title: "How to download files from AWS EC2"
date: 2019-09-30T23:38:21+10:00
slug: "ec2-download"
description: ""
draft: false
tags: ["EC2", "AWS"]
math: false
---

<strike>SCP</strike> `scp` is always our friend.

<!--more-->

```bash
scp -i /path/to/permission/file username@ec2.url:/path/to/remote/file /path/to/local/directory
```

**Update on 2019-10-14.** _What if I need to copy files that need root access?_

```bash
ssh -i /path/to/permission/file username@ec2.url "sudo cat /var/log/nginx/access.log" > ~/Downloads/access.log
```

![img](/image/ec2-root-download.png)

Generally, if it is a log that could be printed out. Just print and save it to local.
