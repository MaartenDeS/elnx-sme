# Cheat sheets and checklists

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

## Basic commands

| Task                | Command |
| :---                | :---    |
| Query IP-adress(es) | `ip a`  |

## Git workflow

Simple workflow for a personal project without other contributors:

| Task                                         | Command                   |
| :---                                         | :---                      |
| Current project status                       | `git status`              |
| Select files to be committed                 | `git add FILE...`         |
| Commit changes to local repository           | `git commit -m 'MESSAGE'` |
| Push local changes to remote repository      | `git push`                |
| Pull changes from remote repository to local | `git pull`                |


## Vagrant

| Task                       | Command                |
| :---                       | :---                   |
| Status                     | `vagrant status`       |
| Turn on virtual machine   | `vagrant up VM`        |
| Reload ansible playbook of a machine | `vagrant provision`    |
| Remove a VM                | `vagrant destroy VM`   |
| Execute roles script       | `./scripts/roles-deps.sh` |


## DNS - pu001/pu002

| Task                       | Command                |
| :---                       | :---                   |
| Primary configuration                     | `/etc/bind/named.conf`       |
| Describes the root name servers in the world.   | `/etc/bind/db.root`        |
| DNS resolver conf file | `/etc/resolv.conf`    |
|  Tool for querying DNS nameservers for information                | `dig`   |
| Pingen to an adres       | `ping` |
| DNS lookup       | `host` |
| Look up a network name server, return ip address      | `nslookup` |


## FILE Server - pr011
| Task                       | Command                |
| :---                       | :---                   |
| Check Permissions                     | `ls -l`       |
| Check Permissions   | `ls -lan`        |


## DHCP Server - pr001
| Task                       | Command                |
| :---                       | :---                   |
| Check DHCP                     | `nmap`       |


## VyOS - router

| Task              | Command           |
| :---              | :---              |
| IP Configuration  | `show interfaces` |
| Show configuration|`show`             |
| Routing           |`show ip route`    |
| Show log          |`show log tail`    |


## Docker

| Task              | Command           |
| :---              | :---              |
| Docker Versie  | `docker --verison` |
| Docker info |`docker info`             |
| List Docker Containers           |`docker ps` / `docker container ls`   |
| Start Docker Container          |`docker up`    |
| Stop Docker Container           |`docker stop`    |
| Kill Docker Container          |`docker kill`    |
| Start Docke Containers met docker-compose          |`docker-compose up`    |




## Checklist network configuration

1. Is the IP-adress correct? `ip a`
2. Is the router/default gateway correct? `ip r -n`
3. Is a DNS-server available? `cat /etc/resolv.conf`

