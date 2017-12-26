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

## Cross-cutting concerns

- Must work on latest stable Ubuntu
