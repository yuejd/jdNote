### installation ###
```
sudo apt-get update
sudo apt-get install build-essential tcl
```
Download, Compile, and Install Redis
```
curl -O http://download.redis.io/redis-stable.tar.gz
tar xzvf redis-stable.tar.gz
mv redis-stable redis
cd redis
make
make test
sudo checkinstall
```

### configuration ###
Now that Redis is installed, we can begin to configure it.

To start off, we need to create a configuration directory. We will use the conventional /etc/redis directory, which can be created by typing:
```
sudo mkdir /etc/redis
```
copy over the sample Redis configuration file included in the Redis source archive:
```
sudo cp redis/redis.conf /etc/redis
```
Next, open the file to adjust a few items in the configuration:
In the file, find the supervised directive. Currently, this is set to no. Since we are running an operating system that uses the systemd init system, we can change this to systemd:
```
# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd
```
Next, find the dir directory. This option specifies the directory that Redis will use to dump persistent data. We need to pick a location that Redis will have write permission and that isn't viewable by normal users.

We will use the /var/lib/redis directory for this, which we will create in a moment:
```
# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir /var/lib/redis
```
Next, we can create a systemd unit file so that the init system can manage the Redis process.

Create and open the /etc/systemd/system/redis.service file to get started.
Inside, we can begin the [Unit] section by adding a description and defining a requirement that networking be available before starting this service;
In the [Service] section, we need to specify the service's behavior. For security purposes, we should not run our service as root. We should use a dedicated user and group, which we will call redis for simplicity. We will create these momentarily.

To start the service, we just need to call the redis-server binary, pointed at our configuration. To stop it, we can use the Redis shutdown command, which can be executed with the redis-cli binary. Also, since we want Redis to recover from failures when possible, we will set the Restart directive to "always";

Finally, in the [Install] section, we can define the systemd target that the service should attach to if enabled (configured to start at boot):
```
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=redis
Group=redis
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```
#### Create the Redis User, Group and Directories ####
Now, we just have to create the user, group, and directory that we referenced in the previous two files.

Begin by creating the redis user and group. This can be done in a single command by typing:
```
sudo adduser --system --group --no-create-home redis
```
Now, we can create the /var/lib/redis directory by typing;
give the redis user and group ownership over this directory;
Adjust the permissions so that regular users cannot access this location:
```
sudo mkdir /var/lib/redis
sudo chown redis:redis /var/lib/redis
sudo chmod 770 /var/lib/redis
```
Start up the systemd service by typing:
```
sudo systemctl start redis
sudo systemctl status redis
```
testing:
```
redis-cli
127.0.0.1:6397> ping
PONG
127.0.0.1:6397> set test "It's working!"
OK
127.0.0.1:6397> get test
"It's working!"
```
Enable Redis to Start at Boot
```
sudo systemctl enable redis
Created symlink from /etc/systemd/system/multi-user.target.wants/redis.service to /etc/systemd/system/redis.service.
```
