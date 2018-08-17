# Enterprise Linux Lab Report - Opdracht 5 - 2de Examen Kans

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van een webserver met Docker

## Test plan

Er zijn geen automatische test aanwezig.

Om te kijken of docker geïnstalleerd is op de server doen we het commando `` docker --version ``.

Om te bekijken of de containers zijn aangemaakt doen we het commando ``sudo docker ps `` of ``sudo docker container ls ``.

Om te bekijken of de webserver online staat surfen we naar 172.16.0.12:8181




## Procedure/Documentation

### Aanmaken machine
Ik voeg een nieuwe host toe in het vagrant host file. De naam is **pr010** met een ip **172.16.0.12** en een subnetmask **255.255.0.0**.

### Installatie Docker
Ik zocht een geschikte ansible role om docker te installeren. Ik kwam uit op ``geerlingguy.docker``. Ik downloade deze role met het script en voegde deze toe in [Site.yml](https://github.com/MaartenDeS/elnx-sme/blob/soluation/ansible/site.yml) bij pr010. 

Ook voegde ik de `` bertvv.rh-base`` role toe zodat de standaard instellingen van **all.yml** ook op de server stonden.

### Containers
De containers heb ik aangemaakt via **docker-compose**. Ik maakte een file aan genaamd [docker-compose.yml](https://github.com/MaartenDeS/elnx-sme/blob/soluation/ansible/docker-compose.yml), daarin maakte ik 2 containers.

Eerst een database container. De container noemt db, de image die ik neem is ``mysql:5.6``. Ook voeg ik 2 environment variabelen toe, namelijk: `` MYSQL_ROOT_PASSWORD: root`` en  ``MYSQL_DATABASE: wordpress``.

Als tweede een website container. De container noemt website en de image die ik neem is `` wordpress:4.9.8-php7.0-apache``. Bij links zet ik ook ``db`` en bij ports ``8181:80``.

Hieronder is de layout van mijn file.

![dockercompose](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010compose.png)

Dit bestand sla ik op in de map ansible. In het [site.yml](https://github.com/MaartenDeS/elnx-sme/blob/soluation/ansible/site.yml) bestand voeg ik een posttask toe bij **pr010**. De post_taks is het command ``command: /usr/local/bin/docker-compose up -d``. Ik verander ook naar de juiste directory waar het bestand zich bevindt met   ``chdir: /vagrant/ansible``. De naam van de task is ``name: Command docker-compose up``.


## Test report

### Docker
Ik bekijk of of docker geïnstalleerd is met het commando ``docker --version``.
![docker](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010docker.png)

### Containers
Ik check of de containers aangemaakt zijn met het commando ``sudo dokcer ps ``.
![container](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010container.png)

### Webserver
Ik surf naar het addres **172.16.0.12:8181**.

![home](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010home.png)

Hierna vul ik mijn database connection details in.

![details](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010details.png)

Dan run ik de installatie en vul daarnee deze info in.

![install](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010installeer.png)

Ik log nu in met mijn username en gekozen password

![login](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010login.png)

Hierna ben ik aangemeld op de webserver. Hieronder ziet u mijn admin scherm en ook de homepage van de webserver.

![admin](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010admin.png)


![web](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr010website.png)





## Resources

Hier zijn mijn recources:

- Docker Role: <https://galaxy.ansible.com/geerlingguy/docker>
- Docker Compose: <https://docs.docker.com/compose/compose-file/>
- Docker Container Wordpress & Apache: <https://hub.docker.com/_/wordpress/>
- Docker Container MySQL: <https://hub.docker.com/_/mysql/>
- Docker Introduction: <https://www.youtube.com/watch?v=YFl2mCHdv24>
- Docker Compose Introduction: <https://www.youtube.com/watch?v=Qw9zlE3t8Ko>