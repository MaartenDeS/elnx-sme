# Enterprise Linux Lab Report - Opdracht 4

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van DHCP en een router + configuratie hiervan met ansible op een Centos systeem.

## Test plan
### dhcp
Bijna alle testen deden we met de automatische bats testen.

1. Start de vm's met het commando `vagrant up`
2. Log in op alle servers met `vagrant ssh pr011 ` en run de tests.
3. Alle tests zouden moeten werken.
4. Bij een test die niet werkt zie je meteen de rede hiervan.

### Router
1. login op de router met ssh `vagrant ssh router`
2. bekijk en controleer de configuaratie met: `show configuration`

## Procedure/Documentation

Allereerst heb ik de ansible role gezocht. Ik kwam uit op 1 rollen
- DHCP: `bertvv.dhcp`
### hosts
Eerst voegen we onze  nieuwe host toe in het vagrant-host file. We voegen ook het correcte ip addres + subnetmask toe. Deze kunnen we vinden in de readme.md.
```
- name: pr001
  ip: 172.16.0.2
  netmask: 255.255.0.0
```
### site.yml
hierna voegen we ze toe in ons site.yml file. we maken een nieuwe host aan en voegen de nodige rollen toe.
```
- hosts: pr001
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.dhcp


```
### Stappenprogramma pr001.yml dhcp

1. Allereerst ik dhcp en ssh verkeer toegelaten
```
rhbase_firewall_allow_services:
  - ssh
  - dhcp
```

2. nadien heb ik de globale instellingen van dhcp geconfigureerd.
  ```
  dhcp_global_default_lease_time: 14400
  dhcp_global_max_lease_time: 28800
  dhcp_global_routers: 172.16.255.254
  dhcp_global_domain_name: 'avalon.lan'
  dhcp_global_domain_name_servers:
    - 192.0.2.10
    - 192.0.2.11

  ```
3. Nadien heb ik elk subnet aangemaakt dat in de opgave stond.

```
dhcp_subnets:
  # Servers
  - ip: 172.16.0.0
    netmask: 255.255.0.0
    begin_range: 172.16.0.2
    end_range: 172.16.127.254
    routers: 172.16.255.254
    domain_name_servers:
      - 192.0.2.10
      - 192.0.2.11
    default_lease_time: 14400
    max_lease_time: 28800
  # mac
  - ip: 172.16.0.0
    netmask: 255.255.0.0
    range_begin: 172.16.128.1
    range_end: 172.16.191.254
    routers: 172.16.255.254
    domain_name_servers:
      - 192.0.2.10
      - 192.0.2.11
    default_lease_time: 14400
    max_lease_time: 28800
  # Dynamic IP
  - ip: 172.16.0.0
    netmask: 255.255.0.0
    range_begin: 172.16.192.1
    range_end: 172.16.255.253
    routers: 172.16.255.254
    domain_name_servers:
      - 192.0.2.10
      - 192.0.2.11
    default_lease_time: 14400
    max_lease_time: 28800
```
### Stappenprogramma router
ik heb bij elk todo puntje de voor mij gepaste waarde ingevuld

1. Bij ip dit:
```
set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth0 description 'WAN link'

# DMZ
set interfaces ethernet eth1 address 192.0.2.254/24
set interfaces ethernet eth1 description 'DMZ'

# Internal
set interfaces ethernet eth2 address 172.16.255.254/16
set interfaces ethernet eth2 description 'internal'
```
2. Dan heb ik NAT ingesteld.
```
set nat source rule 100 outbound-interface 'eth0'

set nat source rule 100 source address '172.16.0.0/16'

set nat source rule 100 translation address 'masquerade'



set nat source rule 200 outbound-interface 'eth1'

set nat source rule 200 source address '192.0.2.0/24'

set nat source rule 200 translation address 'masquerade'

```

3. nadien volgde timezone  
```
set system time-zone Europe/Brussels
```
4. en als laatste de dns forwarding  
```
set service dns forwarding domain avalon.lan server 192.0.2.10

set service dns forwarding name-server 8.8.8.8
set service dns forwarding name-server 8.8.4.4

set service dns forwarding listen-on 'eth2'
```
## Test report

Hier zijn de problemen die ik heb ondervonden.

### Router
De router box wou niet installeren met vagrant up. Maar door de box toe te voegen met het commando `box add ` heb ik dit opgelost.

## Resources

Hier zijn mijn recources:

- VyOS Cheat sheets: <https://github.com/bertvv/cheat-sheets/blob/master/docs/VyOS.md>
- DHCP rol: <https://galaxy.ansible.com/bertvv/dhcp/>
- VSFTPD rol: <https://galaxy.ansible.com/bertvv/vsftpd/>
