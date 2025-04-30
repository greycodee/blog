---
title: 'Adjusting the Docker Container Time Zone and Host Time Zone'
description: 'Learn how to adjust the time zone of a Docker container and the host system to ensure they are in sync.'
tags: ['docker', 'time zone', 'container']
pubDate: 'Apr 30 2025'
heroImage: '/blog-placeholder-1.jpg'
---

## Check the Host Time Zone

```bash
(base) ➜  minio date
Fri Jan 24 11:50:19 AM CST 2025
(base) ➜  minio  cat /etc/timezone 
Etc/UTC
(base) ➜  minio cat /etc/localtime
TZif2UTCTZif2UTC
UTC0
```

It was found that the `date` command output is in the `CST` timezone but `/etc/timezone` and `/etc/localtime` are in UTC timezone. This indicates a problem with the timezone configuration. The following command can be used to changes the host timezone to `CST`:

```bash
sudo timedatectl set-timezone Asia/Shanghai
```

验证更改
Verify the changes:

```bash
date
cat /etc/timezone
```
The output timezone should be `Asia/Shanghai`
The current time should be displayed as `CST (UTC+8)`
If `/etc/localtime` and `/etc/timezone` don't match,you can regenerate the link.

```bash
sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo "Asia/Shanghai" | sudo tee /etc/timezone
```
## Adjusting the Docker Container timezone

Map the host's `/etc/timezone` and `/etc/localtime` to the container when it is started.
```bash
docker run -dt \
  -p 9000:9000 -p 9001:9001 \
  -v /etc/localtime:/etc/localtime:ro \
  -v /etc/timezone:/etc/timezone:ro \
  --name "minio_local" \
  minio/minio server --console-address ":9001"
```