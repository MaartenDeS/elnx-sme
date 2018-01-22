# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van een LAMP stack + configuratie hiervan met ansible op een Centos systeem.

## Test plan

Bijna alle testen deden we met de automatische bats testen.

1. Voer het commando `vagrant up pu004`
2. Log in op de server met `vagrant ssh pu004` en run de tests.
3. Alle tests zouden moeten werken.
4. Bij een test die niet werkt zie je meteen de rede hiervan.

## Procedure/Documentation

Allereerst heb ik de ansible roles gezocht. Ik kwam uit op 3 rollen namelijk:
- Apache & PHP: `bertvv.httpd`
- MariaDB: `bertvv.mariadb`
- WordPress: `bertvv.wordpress`
### Site.yml
Hierin voegen we gewoon de 3 nieuwe rollen toe. In deze volgorde:
```
- bertvv.mariadb
- bertvv.httpd
- bertvv.wordpress
```
Hierdoor worden deze rollen geïnstalleerd.

### Stappenprogramma pu004.yml

1. Allereerst laten we de nodige services toe

  ```
  rhbase_firewall_allow_services:
    - http
    - https
    - ssh
  ```
2. Hierna voeg vul ik mijn mariadb parameters in volgens het stappenplan online.

```
mariadb_databases:
  - name: wordpress
mariadb_users:
  - name: maarten
    password: letmein
    priv: wordpress.*:ALL
    state: present
mariadb_root_password: letmein
```

3. Daarna connecteren we de wordpress met mariadb
```
wordpress_database: wordpress
wordpress_user: maarten
wordpress_password: letmein
```
4. Als laatste voegen we een nieuwe SSL certificaat toe.
  - Eerst en vooral voeren we dit commando uit om een nieuwe key en cert file aan te maken:
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /vagrant/newkey.key -out /vagrant/newcert.crt
  - Daarna voegen we deze pre_tasks toe in het site.yml toe.
```
pre_tasks:
  - name: Copy new key
    copy:
      src: keycert/newkey.key
      dest: /etc/pki/tls/private/newkey.key
  - name: Copy new cert
    copy:
      src: keycert/newcert.crt
      dest: /etc/pki/tls/certs/newcert.crt

```
- Als laatste verwijzen naar de files in het pu004.yml file
```
httpd_SSLCertificateFile: '/etc/pki/tls/certs/newcert.crt'
httpd_SSLCertificateKeyFile: '/etc/pki/tls/private/newkey.key'

```
- Voeg ook php scripting toe.
```
httpd_scripting: 'php'
```

## Test report

Hier zijn de problemen die ik heb ondervonden.
### Certificaatprobleem
Alles verloopte traag maar goed buiten de certificaten. Ik had ze aangemaakt maar kreeg nog meer fouten dan ervoor.

Na het te vragen aan een klasgenoot bleek dat ik de files niet op de juiste plaats had gezet. Toen ik de pre tasks uitvoerde en de locatie veranderde werkte alles wel.


## Resources

Hier zijn mijn recources:

- Certificaten: <https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04>
- Mariadb rol: <https://galaxy.ansible.com/bertvv/mariadb/>
- Httpd rol: <https://galaxy.ansible.com/bertvv/httpd/>
- Wordpress rol: <https://galaxy.ansible.com/bertvv/wordpress/>
