docs: https://redis.io/docs/getting-started/

# Menu
- Getting started
- Data types
- Using Redis
- Managing Redis
- Reference
- Redis Stack

# Quickstart
## install
source: https://redis.io/docs/getting-started/installation/install-redis-on-mac-os/
```
brew install redis
```

Starting and stopping Redis in the foreground
```
redis-server
```

Starting and stopping Redis in background(using launchd)
```
brew services start redis
// check the status of a launchd managed Redis
brew services info redis
```

## Securing Redis
- default Redis binds to all the interfaces and has no authentication at all

use in:
- controlled environment(separated from the external internet and in general from attackers)
  - fine
- unhardened Redis is exposed to the internet
  - big security concern

If you are not 100% sure your environment is secured properly, please check the following steps in order to make Redis more secure(enlisted in order of increased security)
- Make sure the port Redis uses to listen for connections
  - default 6379 
  - additionally 16379 
  - Redis cluster mode
    - 26379 for Sentinel) is firewalled 
      - -> not possible to contact Redis from the outside world.
- Use a configuration file where the bind directive is set in order 
  - to guarantee that Redis listens on only the network interfaces you are using
    - For example 
      - only the loopback interface (127.0.0.1) if you are accessing Redis just locally from the same computer, and so forth.
- Use requirepass option 
  - in order to add an additional layer of security
  - clients will require to authenticate using the AUTH command
- Use SSL tunneling software in order to encrypt traffic between Redis servers and Redis clients if your environment requires encryption.

> Redis instance exposed to the internet without any security is very simple to exploit
> make sure you understand the above and apply at least a firewall layer
> After the firewall is in place, try to connect with redis-cli from an external host in order to prove yourself the instance is actually not reachable

## Redis persistence
default configuration:
- Redis will spontaneously save the dataset only from time to time 
  - for instance after at least five minutes if you have at least 100 changes in your data
  -  if you want your database to persist and be reloaded after a restart (you need to do one of below)-> Redis will make sure to save the data on disk before quitting
     - call the SAVE command manually every time you want to force a data set snapshot
     - to shutdown the database using the SHUTDOWN command

## Installing Redis more properly
actual application to run on a real server. For this kind of usage you have two different choices:
- Run Redis using screen.
- Install Redis in your Linux box in a proper way using an init script, so that after a restart everything will start again properly.

# Redis data types
source: https://redis.io/docs/data-types/

Redis provides a collection of native data types that help you solve a wide variety of problems
- caching
- quene
- event processing

## Core
- Strings
- Lists
- Sets
  - Java HashSets
  - Python sets
- Hashes
- Sorted sets
- Streams
- Geospatial indexes
- Bitmaps
- Bitfields
