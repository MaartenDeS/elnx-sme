# Enterprise Linux Lab Report - Opdracht 4

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van DHCP en een router + configuratie hiervan met ansible op een Centos systeem.
We maken ook een client systeem.

## Test plan

### DHCP en Client
Hiervoor zijn geen automatische tips om de server te controleren.
We voeren het commando ip a uit op de client om te bekijken of de ip adressen correct zijn.


### Router
1. login op de router met ssh `vagrant ssh router`
2. bekijk en controleer de configuaratie met: `show configuration`

En we bekijken of de client een internet connectie heeft.


## Procedure/Documentation
Allereerst heb ik de ansible role gezocht. Ik kwam uit op 1 rollen
- DHCP: `bertvv.dhcp`
### hosts

Eerst voegen we onze  nieuwe host toe in het vagrant-host file. We voegen ook het correcte ip addres + subnetmask toe. Deze kunnen we vinden in de readme.md.

### [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml)
Hierna voegen we ze toe in ons [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml) file. we maken een nieuwe host aan en voegen de nodige rollen toe: rh-base en dhcp.

### Stappenprogramma Client

1. We maken een nieuwe machine in VirtualBox.

2. We voegen 2 netwerkadpaters toe. We zorgen ervoor dat dit host-only adapters zijn met de netwerkinstellingen 172.16.0.0 255.255.0.0.

3. Bij een netwerkadpater bekijken we het mac adres en schrijven we dit ergens op (dit hebben we later nodig).


### Stappenprogramma [pr001.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pr001.yml) dhcp

1. Allereerst ik dhcp en ssh verkeer toegelaten.

2. Nadien heb ik de globale instellingen van dhcp geconfigureerd.

3. Hierna heb ik het subnet aangemaakt.

4. Hierna voegen we een dhcp host toe. We kiezen een ip adres binnen de grenzen van de opgave en we gebruiken het mac adres dat we hebben opgeschreven.

### Stappenprogramma [router](https://github.com/MaartenDeS/elnx-sme/blob/soluation/scripts/router-config.sh)
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
set service dns forwarding dhcp 'eth0'
set service dns forwarding listen-on 'eth1'
set service dns forwarding listen-on 'eth2'
```

## Test report

### DHCP
We starten de machine pr001. Hierna starten we de client en gaan we naar de terminal. Hier voeren we het commando ``ip a`` uit. Hiermee controleren we of DHCP de juiste ip adressen heeft meegegeven.
![LAMP](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/ip.png)

### Router
We starten de machine router. Hierna starten we de client en gaan we naar firefox en browsen we naar google.com.
![LAMP](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/internet.png)

## Resources

Hier zijn mijn recources:

- VyOS Cheat sheets: <https://github.com/bertvv/cheat-sheets/blob/master/docs/VyOS.md>
- DHCP rol: <https://galaxy.ansible.com/bertvv/dhcp/>
- VSFTPD rol: <https://galaxy.ansible.com/bertvv/vsftpd/>
