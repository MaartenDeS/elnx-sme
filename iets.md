# Enterprise Linux - Troubleshooting

- Student name: Maarten De Smedt
- Class/group: TIN2-TI-3B

## Instructions



## Report

### default gateway

Eerste probleem dat ik tegen kom is dat mijn default gateway niet goed is geconfigureerd

Ik doe het commando 
**ip route** 
en zie dat effectief op op en 10. adres staat en niet op 172.20.0.0/24

in de **/etc/sysconfig/network-scripts/ifcfg-enp0s8** ga ik kijken of dhcp aan staat
deze staat aan.

Het is dus een dhcp probleem.

Op de machine server ga ik naar mijn **dhcpp.conf** file in /etc/dhcp/ als sudo. Aangezien hier de dhcp server draait

1. ik zet de subnetmask naar 255.255.255.0
2. de routers naar 172.20.0.254
3. en domain-name-server naar 172.20.0.2

Aangezien dit de waarde zijn in de pdf file.

Ik restart de dhcpd service met
** systemctl restart dhcpd**

Als ik nu ip route doe op de workstation is het de waarde.

### Linuxlab
ik doe een nslookup vwww.linuxlab.lan en vind deze niet.

op mijn server ga ik kjiken naar mijn dns service = dnsmasq.
** systemctl status dnsmasq**

Deze draait niet dus ik zet ze aan.
**systemctl start dnsmasq**


een nslookup op de workstation werkt nog steeds niet.

op de server zet ik in de dnsmasq.conf file in etc de regel
**interfaces=enp0s8**

een nslookup werkt nog steeds niet op de workstation


ik kijk of firewall dit toelaat op de server
**firewall-cmd --list-all**
dns staat er niet bij dus we doen
**firewall-cmd --add-service=dns**

als ik nu **nslookup www.linuxlab.lan** doe werkt het.
met **curl www.linuxlab.lan** krijg ik een welkomboodschap


### icanhazip

als ik een **nslookup www.icanhazip.com** doe op de workstation werkt dit niet. ook een ping naar 8.8.8.8 werkt niet.

Ik ga naar de router.

allereerst doe ik een **configure**
ik doe **show interfaces**

ik zie dat bij eth1 het adress 172.22.0.254 is ipv 172.20.0.254. 

ik verander dit met het commando
**set interfaces ethernet eth1 address 172.20.0.254/24**

ik kijk weer en nu staan ze er allebei dus doe ik
**delete interfaces ethernet eth1 address 172.22.0.254/24**

ze staan er nog steeds allebei

ik commit het en save.
vanop de router kan ik pingen naar 8.8.8.8

ik ga weer naar de werkstation
een ping 8.8.8.8 werkt
als ik **nslookup www.icanhazip.com** werkt dit ook
ook ** curl www.icanhazip.com** werkt .







