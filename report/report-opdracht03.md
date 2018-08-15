# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van SMB en FTP filservers + configuratie hiervan met ansible op een Centos systeem.

## Test plan

Bijna alle testen deden we met de automatische bats testen.

## Procedure/Documentation

Allereerst heb ik de ansible role gezocht. Ik kwam uit op 2 rollen;
- Samba: `bertvv.samba`
- VSFTPD: `bertvv.vsftp`
### hosts
Eerst voegen we onze  nieuwe host toe in het vagrant-host file. We voegen ook het correcte ip addres + subnetmask toe. Deze kunnen we vinden in de readme.md.

### [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml)
hierna voegen we ze toe in ons [Site.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/site.yml) file. we maken een nieuwe host aan en voegen de nodige rollen toe: rh-base, samba en vsftpd.

### Stappenprogramma [pr011.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pr011.yml) samba

1. Allereerst heb ik de  gevraagde groepen en users toegevoegd. Met ook een user voor mezelf (`maarten`). Ook heb ik aan alles users die niet in it zitten geen toegang gegeven tot de shell.

2. nadien heb ik alle shares aangemaakt.
  
3. Daarna weer de juiste services toelaten bij de firewall: ssh en samba.

4. Hierna voegde ik de vereiste parmaters toe.

### Stappenprogramma [pr011.yml](https://github.com/MaartenDeS/elnx-sme/tree/soluation/ansible/pr011.yml) vsftpd
1. eerst en vooral voeg ik een firewall regel toe voor ftp.

2. Dan heb ik anonymous users verboden en registered users toegestaan.

3. de root paste ik ook aan.
  
4. daarna heb ik ook nog schrijftoegang gegeven


## Test report

De server wordt aangemaakt met het commando ``vagrant up pr011``. We loggen in met het commando ``vagrant ssh pr011``). Hierna voeren we tests uit met het commando ``sudo /vagrant/test/runbats.sh``

![file](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/pr011.png)

Ook heb ik dit getest via filezilla.

![zilla](https://github.com/MaartenDeS/elnx-sme/blob/soluation/report/screen/filezilla.png)

## Resources

Hier zijn mijn recources:



- Aanmaken ACL's: <http://docs.ansible.com/ansible/latest/acl_module.html>
- Samba rol: <https://galaxy.ansible.com/bertvv/samba/>
- VSFTPD rol: <https://galaxy.ansible.com/bertvv/vsftpd/>
