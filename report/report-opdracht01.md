# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van een LAMP stack + configuratie hiervan met ansible op een Centos systeem.

## Test plan

Het testen deden we met de automatische bats testen.

## Procedure/Documentation

Allereerst heb ik de ansible roles gezocht. Ik kwam uit op 3 rollen namelijk:
- Apache & PHP: `bertvv.httpd`
- MariaDB: `bertvv.mariadb`
- WordPress: `bertvv.wordpress`

### [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml)
Hierin voegen we gewoon de 3 nieuwe rollen toe. In deze volgorde:

- bertvv.mariadb
- bertvv.httpd
- bertvv.wordpress

Hierdoor worden deze rollen geïnstalleerd.

### Stappenprogramma [pu004.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pu004.yml)

1. Allereerst laten we de nodige services toe: http, https en ssh.


2. Hierna voeg vul ik mijn mariadb parameters in volgens het stappenplan online met een user Maarten en wachtwoord letmein. De naam van de DB is wordpress.


3. Daarna connecteren we de wordpress met mariadb

4. Als laatste voegen we een nieuwe SSL certificaat toe.
  - Eerst en vooral voeren we dit commando uit om een nieuwe key en cert file aan te maken:
    ``sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /vagrant/newkey.key -out /vagrant/newcert.crt ``
  - Daarna voegen we pre_tasks toe in het [site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml) toe, zodat de altijd bestanden gekopieerd worden naar de juiste locatie.

- Dan verwijzen ik naar de files in het [pu004.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pu004.yml) file

- Als laatste voeg ik ook php scripting toe.


## Test report

Om de opstelling te testen moeten we de server provisionnen. Dit doen we met het commando ``vagrant provision pu004`` Hierna connecteren we met de server via het commando ``vagrant ssh pu004``. Daarna voeren we de test scripts uit met het commando: `` sudo /vagrant/test/runbats.sh`` .


![LAMP](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pu004.png)

## Resources

Hier zijn mijn recources:

- Certificaten: <https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04>
- Mariadb rol: <https://galaxy.ansible.com/bertvv/mariadb/>
- Httpd rol: <https://galaxy.ansible.com/bertvv/httpd/>
- Wordpress rol: <https://galaxy.ansible.com/bertvv/wordpress/>
