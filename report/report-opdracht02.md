# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van DNS in het netwerk + configuratie hiervan met ansible op een Centos systeem.

## Test plan

Bijna alle testen deden we met de automatische bats testen.

## Procedure/Documentation

Allereerst heb ik de ansible role gezocht. Ik kwam uit op 1 rol. DNS: `bertvv.bind`

### hosts
Eerst voegen we onze 2 nieuwe hosts toe in het vagrant-host file. We voegen ook het correcte ip addres + subnetmask toe. Deze kunnen we vinden in de readme.md.


### [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml)
hierna voegen we ze toe in ons [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml) file. we maken een nieuwe host aan en voegen de rollen rhbase en bind toe. 


### Stappenprogramma [pu001.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pu001.yml)

1. Allereerst laten we weer de nodige services toe: ssh en dns.


2. Daarna doen de basis instellingen. Als naam geven we **avalon.lan**, het master server ip **192.0.2.10**, de zone networks zijn **172.16 en 192.0.2** en listen ip's zijn **172.0.0.1 en 192.0.2.10** .


3. Hierna zetten we de recursieve lookup zones uit en maken de nodige records aan.


4. Dan zeggen welke dns-servers er aanwezig zijn in het LAN en naar welke slaves er mag gerepliceerd worden.


### Stappenprogramma [pu002.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pu002.yml)
1. Bij puu02 heb ik gewoon de basisinstellingen geconfigureerd. Als naam geven we **avalon.lan**, het master server ip **192.0.2.10**, de zone networks zijn **172.16 en 192.0.2** en listen ip's zijn **172.0.0.1 en 192.0.2.10** .


## Test report
We maken de servers pu001 en pu002 aan. Dit doen we met de commando's `` vagrant up pu001 `` en `` vagrant up  pu002 ``. Daarna connecteren we met de servers doormiddel van ssh: ``vagrant ssh pu001`` en ``vagrant ssh pu002``. Bij beide voeren we de tests uit met het commando ``sudo /vagrant/test/runbats.sh``.

![01](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pu001.png)

![02](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pu002.png)
## Resources


- DNS rol: <https://galaxy.ansible.com/bertvv/bind/>

