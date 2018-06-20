## Installation
* Install build tools
  > `$ sudo yum install -y gcc make`

* Get the required redis version
  > `$ wget http://download.redis.io/releases/redis-4.0.10.tar.gz`

* Extract the file
  > `$ tar xvzf redis-4.0.10.tar.gz`

* Build from source
  > `$ cd redis-4.0.10`  
  > `$ make`  
  > `$ make install`

## Install Redis Properly as a server
Use Installation script

  * `$ sudo utils/install_server.sh`


## Disable redis persistance (If plan to use as cache only)  
  Edit the following configuration in `/etc/redis/6379.conf`
  * Disable AOF by setting `appendonly` to `no`
  * Disable RDB snapshotting by commenting out all `save` directives
  * Restart the redis server
    > `$ sudo service redis_server restart`

## Few other things for performance
* Disable Transparent Huge Pages
  > `$ echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled`

* Also add this on /etc/rc.local so that it takes affect on reboot
  > `$ echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled" | sudo tee -a /etc/rc.local`

* Set linux kernel overcommit_memory to 1
  > `$ sudo sysctl vm.overcommit_memory=1`








