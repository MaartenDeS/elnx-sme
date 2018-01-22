# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>

Het opzetten van de basisintellingen in ansible voor alle servers.

## Test plan

1. Ga naar de directory waarin de repository zich bevind op het systeem
2. Voor het commando `vagrant status`
    - Er zou een vm moeten zijn, `pu004` met de  status `not created`. Als deze wel bestaat destroy deze eerst met het commando,`vagrant destroy -f pu004`
3. Voer het commando `vagrant up pu004`
    - Dit zou moeten slagen
4. Log in op de server met `vagrant ssh pu004` en run de eerste tests. Deze zouden moeten werken (Naam moet wel worden verandert).
5. Voer de acceptie testen uit.
  Dit zou je moeten krijgen:

    ```
    [vagrant@pu004 test]$ sudo /vagrant/test/runbats.sh
    Running test /vagrant/test/common.bats
    ✓ EPEL repository should be available
    ✓ Bash-completion should have been installed
    ✓ bind-utils should have been installed
    ✓ Git should have been installed
    ✓ Nano should have been installed
    ✓ Tree should have been installed
    ✓ Vim-enhanced should have been installed
    ✓ Wget should have been installed
    ✓ Admin user bert should exist
    ✓ Custom /etc/motd should be installed

    10 tests, 0 failures
    ```

    Lamp tests zouden kunne falen nu.

5. log uit ssh en in met `ssh maarten@192.0.2.50`. Als alles correct is geïnstalleerd zou je geen wachtwoord prompt moeten krijgen.

    ```
    $ ssh maarten@192.0.2.50
    Welcome to pu004.localdomain.
    enp0s3     : 10.0.2.15         fe80::a00:27ff:fe5c:6428/64
    enp0s8     : 192.0.2.50        fe80::a00:27ff:fecd:aeed/64
    [maarten@pu004 ~]$
    ```

## Procedure/Documentation

Allereerst heb ik de stappen in de sylabus gevolgd. Hierdoor leerde ik ook ook meteen werken met Ansible. Nadien heb ik de stappen gevolgd die bij de documentatie stonden van de role `bertvv.rhbase`. Dit deed ik door het bestand in `group-vars` aan te passen.

### Stappenprogramma

1. Allereerst heb ik de EPEL repository geïnstalleerd.

  ```
  rhbase_repositories:
    - epel-release
  ```
2. Dan heb ik de packages toegevoegd.

```
rhbase_install_packages:
  - bash-completion
  - vim-enhanced
  - bind-utils
  - git
  - nano
  - tree
  - wget
```

3. Dan heb ik de user toegevoegd met de nodige parameters en rechten.
```
rhbase_users:
    - name: maarten
      comment: 'Maarten De Smedt'
      groups:
         - wheel
         - users
      password: '$6$TL02RSYFDNZ.0Oy3$xnq84mvPkvYlFnc8O35UGV2O58ohiLcW.7WX8dHvqIHajYCsVD1tC7.HIUCUTiGCuf5zbRGG3VwnysLwFwwAo/'
```
4. De custom MOTD aanmaken.
```
rhbase_motd: true

```
5. Het aanmaken van de rsa key heb ik als volgt gedaan:
  - in git bash heb ik het commando `ssh-keygen` uitgevoerd.
  - en de inhoud van id_rsa.pub was dan mijn ssh_key:
  ```
  rhbase_admin_user: maarten
  rhbase_admin_ssh_key: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDmaqbeev+B54vgppVTV5qBpp5nMz+V8Qh99KTIXWmfSDQmn1isysZFzdz6SkQLQ8O92KdLfHDhM6UydpuPAqukODPO4S5J2TvH+VTGM5Ewu2mhevjaNn11RI35uPtC7G97sqoBG8LaZi98/g0avi21CSp7JJ1r/fHePzbWVy7yQns5lgI33WgsXmHHwuPYwutTCLiJ96aAq0XiTXI/zvMhBri3tQ7ODjOzmex14PT4yngnk9UTLn+AjIn1jP0l0p4RzbDj5d25Rb3MR5TaOww0bPxVdqxb0go+iTvxWsohmwSKqkQzoz9pZOYrUymIUMbc6rSCnrD5vsV2hSGsHUOp Maarten@Maarten-PC'


  ```


## Test report

Hier zijn de problemen die ik heb ondervonden.
### Beginnersproblemen
Ik had hier eigenlijk maar 1 groot probleem. Ik verstond ansible nog niet zo goed en had mijn parameters in de role folder gezit in plaats van in de group_vars folder. Hierdoor werkte wel het meeste maar sommige dingen niet.

#### Toelichting
Toen ik de lector om hulp vroeg bij 1 van de problemen, wees die er mij op dat ik verkeerd te werk ging. Toen ik alles verplaatste werkte alles wel.

## Resources

Hier zijn mijn recources:

- Rhbase rol: <https://galaxy.ansible.com/bertvv/rh-base/>
