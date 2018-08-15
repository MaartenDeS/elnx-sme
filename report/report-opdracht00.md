# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>


Het opzetten van de basisintellingen in ansible voor alle servers.

## Test plan

De server kan getest worden doormiddel van de testscripts.

## Procedure/Documentation


Eerst voegde ik in het [all.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/all.yml) bestand de EPEL Repository en de packages toe. In dit bestand voegde ik ook de user Maarten toe met wachtwoord maarten. Deze user is een admin. De motd werd ook aangezet. En ten laatste voegde ik de rsa key toe. Dit deed ik met commando ``ssh-keygen``.


## Test report

Om dit te testen kieze we eerst welke van de servers(pu001,pu002,pu004,pr001 of pr011) moet getest worden. Daarna doen we ``vagrant  provision SERVER `` . Daarna gaan we naar de server via het commando ``vagrant ssh SERVER``. Eens we in de server zijn voeren we de tests uit met het commando `` sudo /vagrant/test/runbats.sh ``.

[basis](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/basis.png)

## Resources

- Rhbase rol: <https://galaxy.ansible.com/bertvv/rh-base/>
- Genereren van ssh key: <https://help.github.com/articles/connecting-to-github-with-ssh/>