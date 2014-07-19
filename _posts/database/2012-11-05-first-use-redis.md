---
layout: post
category: database
tag: [redis]
description: Install and run redis on my MacBook.
---

## Install Redis

```bash
lch@localhost:~ $ brew install redis
######################################################################## 100.0%
==> make -C /private/tmp/brew-redis-2.4.16-OlCz/redis-2.4.16/src CC=cc
==> Caveats
If this is your first install, automatically load on login with:
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/Cellar/redis/2.4.16/homebrew.mxcl.redis.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

If this is an upgrade and you already have the homebrew.mxcl.redis.plist loaded:
    launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
    cp /usr/local/Cellar/redis/2.4.16/homebrew.mxcl.redis.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

  To start redis manually:
    redis-server /usr/local/etc/redis.conf

  To access the server:
    redis-cli
==> Summary
/usr/local/Cellar/redis/2.4.16: 9 files, 456K, built in 2.6 minutes
```

## Start Redis Service

```bash
lch@localhost:~ $ redis-server /usr/local/etc/redis.conf
[21932] 30 Nov 13:40:58 * Server started, Redis version 2.4.16
[21932] 30 Nov 13:40:58 * DB loaded from disk: 0 seconds
[21932] 30 Nov 13:40:58 * The server is now ready to accept connections on port 6379
[21932] 30 Nov 13:40:58 - DB 0: 34 keys (3 volatile) in 96 slots HT.
[21932] 30 Nov 13:40:58 - 0 clients connected (0 slaves), 926832 bytes in use
[21932] 30 Nov 13:41:03 - DB 0: 34 keys (3 volatile) in 64 slots HT.
[21932] 30 Nov 13:41:03 - 0 clients connected (0 slaves), 926832 bytes in use
[21932] 30 Nov 13:41:08 - DB 0: 34 keys (3 volatile) in 64 slots HT.
[21932] 30 Nov 13:41:08 - 0 clients connected (0 slaves), 926832 bytes in use
```

## Run as a daemon

Edit /usr/local/etc/redis.conf on line 17 to set 'daemonize yes', then start.
