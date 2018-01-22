# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het op zetten van SMB en FTP filservers + configuratie hiervan met ansible op een Centos systeem.

## Test plan

Bijna alle testen deden we met de automatische bats testen.

1. Start de vm's met het commando `vagrant up`
2. Log in op alle servers met `vagrant ssh pr011 ` en run de tests.
3. Alle tests zouden moeten werken.
4. Bij een test die niet werkt zie je meteen de rede hiervan.

## Procedure/Documentation

Allereerst heb ik de ansible role gezocht. Ik kwam uit op 2 rollen
- Samba: `bertvv.samba`
- VSFTPD: `bertvv.vsftp`
### hosts
Eerst voegen we onze  nieuwe host toe in het vagrant-host file. We voegen ook het correcte ip addres + subnetmask toe. Deze kunnen we vinden in de readme.md.
```
- name: pr011
  ip: 172.16.0.11
  netmask: 255.255.255.0
```
Hierdoor worden deze rollen geïnstalleerd.
### site.yml
hierna voegen we ze toe in ons site.yml file. we maken een nieuwe host aan en voegen de nodige rollen toe.
```
- hosts: pr011
  become: true
  roles:
    - bertvv.rh-base
    - bertvv.samba
    - bertvv.vsftpd

```
### Stappenprogramma pr011.yml samba

1. Allereerst heb ik de  gevraagde groepen en users toegevoegd. Met ook een user voor mezelf (`maarten`).
```
rhbase_user_groups:
  - management
  - technical
  - sales
  - it
  - public

rhbase_users:
  - name: maarten
    comment: 'Maarten De Smedt'
    groups:
      - public
      - users
      - wheel
      - it
    password: '$6$RFkrtDEm.zSaJo6M$ZgnKyLIWaOL3spMVq0YsK81R452rDuQpKKbphmMwdW71VJbyDA3wrU7lmk2FOAZUros5MEKWFc0roeKW.gok80'
  - name: alexanderd
    comment: 'Alexander De Coninck'
    groups:
      - public
      - technical
    password: '$6$bm1UvPdNpJHxcy0L$nf4GNlAEX1DFY25/KU0fQqVf.eTXVOG9Hw63wEgoTtG/FoxtK5xTw48yXeK93GvaPofnNE4qkzwasr/POGhT61'
    shell: /sbin/nologin
  - name: stevenh
    comment: 'Steven Hermans'
    groups:
      - public
      - management
    password: '$6$fdfpLFCgzSGwX0ZE$crRTu83.FdFqjhnVg101.DNqGm/LIr3i06SGk4O0R5GhApBck7TppzvAUn.4cG65rj3Hnd3f3hwBqjRMVrSEi1'
    shell: /sbin/nologin
  - name: stevenv
    comment: 'Steven Van Daele'
    groups:
      - public
      - technical
    password: '$6$4q7j0RCXCwv6t0ym$/sS9ACNMJH4vvvwNz0CxcGyi/y/MzDC59vnebu30Yvt4J/u3epbI0VVjZUaLBgCiYzZPUI7PQNK9Fk2JmOmE31'
    shell: /sbin/nologin
  - name: leend
    comment: 'Leen De Meester'
    groups:
      - public
      - technical
    password: '$6$n/Uqa/IY.5yufaxp$XPTaWwjL1x199ye7PMX1CviHCmxeOomx9nxBaYMoecusDGclysKdpPM28NvlzyTVm.pi45GcJwA1BdOSKlaTg.'
    shell: /sbin/nologin
  - name: svena
    comment: 'Sven Aerens'
    groups:
      - public
      - sales
    password: '$6$H/Mmq6Rsk0/Ntj99$jiyUFC4xkbrZtdhF5PNUQ7cfyo0pWQpmIQucEazqH/H7ks9jCINqGvYTmbsCSrGtAzxbFw.3vzglLTQTEHJkf/'
    shell: /sbin/nologin
  - name: nehirb
    comment: 'Nehir Başar'
    groups:
      - public
      - it
    password: '$6$m3pEGJ4TUthiUlmU$6ZwCoTL9diSYs2JDE2pCnwoCOsku2HYWJOQWAfke2.GEfQxjJEZRC1shqvY3JO/k4yQLgO3yQpn0R4TqSQ8IW/'
  - name: krisv
    comment: 'Kris Vanhaverbeke'
    groups:
      - public
      - management
    password: '$6$iqZN.N6nOPOU55KL$S4p/NuAXwIUqeXwNIGnmMs13ZvdmQfpTeoQMAURAWy7QJuYz8axO4rLQZkV5BUPgkrElvpGwXpg29MAUE6GY60'
    shell: /sbin/nologin
  - name: benoitp
    comment: 'Benoit Pluquet'
    groups:
      - public
      - sales
    password: '$6$AOJzOhSC6ih.JVIj$YE4fbJujzUfbHlBFG7VjHRD5nF2PQhfRSJAwM25LDFS0pR/4D322Aj6JZylB0WwWs45cYFWG7cdvTVQtHG/hM0'
    shell: /sbin/nologin
  - name: anc
    comment: 'An Coppens'
    groups:
      - public
      - technical
    password: '$6$Fa1I7PXWEQmaxDtD$TTsSlOqgAJm1B8jOn5s/zxypLJf5ubw6cS6/GZ8KVyebCPbf4OE9oVt/GOtBesqjR6FM58e3DVG7P2taQDJO90'
    shell: /sbin/nologin
  - name: elenaa
    comment: 'Elena Andreev'
    groups:
      - public
      - management
    password: '$6$n5OPm7ZknwmAT0pn$YrFDkOTOUfMnBholf/QMeM.ilu.npXWJTCvuEc4ZMt4QMwrpEYuSxcBTzbIJ9hMdl0aeIyyFx0.tyqkK9Xki4/'
    shell: /sbin/nologin
  - name: evyt
    comment: 'Evy Tilleman'
    groups:
      - public
      - technical
    password: '$6$boNaMhWKEuKiKbNa$meCQpmsCEl7ID0o0/Um.Ty3s28Mm3e0tlxedqNAuipb70FPeYJHiy.g8GgV9Lh477LKOn3g13CYpKAV4Yb93h.'
    shell: /sbin/nologin
  - name: christophev
    comment: 'Christophe Van der Meersche'
    groups:
      - public
      - it
    password: '$6$fNRlC9cjQqICdsA2$/q9bW.9sKgpeNdhBh3zVGULF04TYYYDxyEwMADq0H/0YQ8GvjoS/xFZrFiCxqRGIywzz2WkBOhWE78PQBKSIc0'
  - name: stefaanv
    comment: 'Stefaan Vernimmen'
    groups:
      - public
      - technical
    password: '$6$YF1SJIRvs9oacOvj$jBZXV67E2H9ll7h8VBOmms7mkS61QqRcC.q.ZnJbWUhwgNttBPBOa/.lgYHr9cwU9bLxOYY4zTI1s2oTkoRQC1'
    shell: /sbin/nologin
```

ook heb ik aan alles users die niet in it zitten geen toegang gegeven tot de shell.

2. nadien heb ik alle shares aangemaakt.
  ```
  samba_shares:
    - name: public
      comment: 'Toegan iedere afdeling'
      write_list: +public
      public: no
    - name: management
      comment: 'Toegang Management'
      group: management
      write_list: +management
      valid_users: +management
      public: no
    - name: technical
      comment: 'Toegang Technical'
      group: technical
      write_list: +technical
      public: no
    - name: it
      comment: 'Toegan IT'
      group: it
      write_list: +it
      valid_users: +it,+management
      public: no
    - name: sales
      comment: 'Toegant Sales'
      group: sales
      write_list: +sales
      valid_users: +management,+sales
      public: no

  ```
3. Daarna weer de juiste services toelaten bij de firewall

```
rhbase_firewall_allow_services:
  - ssh
  - samba
```
### Stappenprogramma pr011.yml vsftpd
1. eerst en vooral voeg ik een firewall regel toe voor ftp.
```
rhbase_firewall_allow_services:
  - ssh
  - samba
  - ftp
```
2. dan heb ik anonymous users verboden en registered users toegestaan.
```
vsftpd_anonymous_enable: false
vsftpd_local_enable: true

```

3. de root heb ik ook aangepast  
```
vsftpd_local_root: /srv/shares
```
4. daarba heb ik ook nog schrijftoegang gegeven
```
vsftpd_write_enable: true
```

hierna werken alle testen.

## Test report

Hier zijn de problemen die ik heb ondervonden.

### readwriteacces problemen
De read en write acces van de public groep werkte niet. De users wouden geen acces krijgen tot het readen writen in de groep public.


## Resources

Hier zijn mijn recources:


- Samba rol: <https://galaxy.ansible.com/bertvv/samba/>
- VSFTPD rol: <https://galaxy.ansible.com/bertvv/vsftpd/>
