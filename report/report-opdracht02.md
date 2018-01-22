# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van DNS in het netwerk + configuratie hiervan met ansible op een Centos systeem.

## Test plan

Bijna alle testen deden we met de automatische bats testen.

1. Start alle vm's met het commando `vagrant up`
2. Log in op alle servers met `vagrant ssh ` en run de tests.
3. Alle tests zouden moeten werken.
4. Bij een test die niet werkt zie je meteen de rede hiervan.

## Procedure/Documentation

Allereerst heb ik de ansible role gezocht. Ik kwam uit op 1 rol
- DNS: `bertvv.bind`
### hosts
Eerst voegen we onze 2 nieuwe hosts toe in het vagrant-host file. We voegen ook het correcte ip addres + subnetmask toe. Deze kunnen we vinden in de readme.md.
```
- name: pu001
  ip: 192.0.2.10
  netmask: 255.255.255.0

- name: pu002
  ip: 192.0.2.11
  netmask: 255.255.255.0

```
Hierdoor worden deze rollen geïnstalleerd.
### site.yml
hierna voegen we ze toe in ons site.yml file. we maken een nieuwe host aan en voegen de nodige rollen toe.
```
- hosts: pu001,pu002
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.bind

```
### Stappenprogramma pu001.yml

1. Allereerst laten we weer de nodige services toe

  ```
  rhbase_firewall_allow_services:
    - ssh
    - dns
  ```
2. Daarna doen de basis instellingen.

```
bind_zone_name: avalon.lan

bind_zone_master_server_ip: 192.0.2.10
bind_zone_networks:
  - '192.0.2'
  - '172.16'

bind_listen_ipv4:
  - '127.0.0.1'
  - '192.0.2.10'
```

3. Hierna zetten we de recursieve lookup zones uit en maken de nodige records aan
```
bind_recursion: false


bind_zone_hosts:
  ## Router
  - name: r001
    ip:
      - 192.0.2.254
      - 172.16.255.254
    aliases:
      - gw
  ## Publiek
  - name: pu001
    ip: 192.0.2.10
    aliases:
      - ns1
  - name: pu002
    ip: 192.0.2.11
    aliases:
      - ns2
  - name: pu003
    ip: 192.0.2.20
    aliases:
      - mail
  - name: pu004
    ip: 192.0.2.50
    aliases:
      - www
  ## Privaat
  - name: pr001
    ip: 172.16.0.1
    aliases:
      - dhcp
  - name: pr002
    ip: 172.16.0.2
    aliases:
      - directory
  - name: pr010
    ip: 172.16.0.10
    aliases:
      - inside
  - name: pr011
    ip: 172.16.0.11
    aliases:
      - files
  # Mail server
    bind_zone_mail_servers:
      - name: pu003
        preference: 10
```
4. Dan zeggen welke dns-servers er aanwezig zijn in het LAN en naar welke slaves er mag gerepliceerd worden.
```
bind_zone_name_servers:
  - pu001
  - pu002

bind_acls:
  - name: slaves
    match_list:
      - 192.0.2.11

```
### Stappenprogramma pu002.yml
1. Bij puu02 heb ik gewoon de basisinstellingen geconfigureerd.  
```
bind_zone_name: avalon.lan

bind_zone_master_server_ip: 192.0.2.10

bind_zone_networks:
  - '192.0.2'
  - '172.16'

bind_listen_ipv4:
  - '127.0.0.1'
  - '192.0.2.11'

bind_zone_name_servers:
  - pu001
  - pu002

bind_allow_query:
  - 'any'


```

## Test report

Hier zijn de problemen die ik heb ondervonden.

### Standaardproblemen
Problemen die ik tegenkwam waren van geen groot kaliber. Meestal waren het gewoon syntaxerrors in de bind_zone_hosts.

Ook werkte eerst mijn pu002 niet. Maar dit kwam omdat ik er niet naar verwees in bind_acls. Toen ik dit deed werkte het wel.

## Resources

Hier zijn mijn recources:


- DNS rol: <https://galaxy.ansible.com/bertvv/bind/>
