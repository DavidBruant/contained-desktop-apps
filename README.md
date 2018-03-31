# Contained desktop apps

An attempt at finding a practical low-cost large-scale solution that helps with containing applications/processes in desktop OSes.
The default state of applications on desktop is that any application can: 
- **interfere with any other app**
- **read/write any file** x **anytime** (as much as the user who started the process)
- use the network arbirarily (write to **any MAC address** x **any IP address** x **any domain name** x **anytime**)

Any easy-to-use solution that helps reducing any of these element would be welcome.

Files are the most problematic.

[Docker](http://docker.io/) seems like a promising foundation for Linux Desktop.

Progress tracked here: https://github.com/DavidBruant/contained-desktop-apps/projects/1

## Doc

### Container with Wayland user interface

```sh
docker-compose build ui
docker-compose run ui gnome-calculator
# The calculator with a UI should show
```

### Container that can read/write files as current user

```sh
docker-compose run as-user touch yo.yo

ll
# the file yo.yo should be created with correct user
```

### Container with restricted network

#### Container with no network

```sh
docker-compose run no-net ping google.com
# ping: bad address 'google.com' 
# this is because DNS resolution is not possible
docker-compose run no-net ip n
# nothing
```


#### One container network-distanced from host

```sh
ip neighbor
# Shows which MAC addresses the host can reach

# Exercise the network, then see what MAC addresses are available
docker-compose run defnet bash -c "ping -c 1 google.com && ip neighbor"
# Shows which MAC addresses the container can reach, which should be different than the host 
# It should be the docker-compose project's ip
# The container has then no access to the host neighbor's MAC addresses
```


#### Two containers which are unaware of each other's MAC address and can communicate only through the host

```sh
# in one console
docker-compose run defnet bash -c "ping -c 1 google.com && ip neighbor && ip -4 addr && sleep infinity"
# ip -4 addr shows the IP & MAC address of this container

# in another console
docker-compose run defnet bash -c "ping -c 1 <other container IP address> && ip neighbor && sleep infinity"
# the ip neighbor command shows the MAC address of the other container proving it's within reach

# Result : the default bridge doesn't MAC-isolate containers (expected since they share the same bridge)
```



## Cross-cutting concerns

- Must work on latest stable Ubuntu
