# Opdracht Actualiteit - CRI-O

## Inleiding

Als een deel van de linux taak moesten we ook een actua taak kiezen. Ik heb gekozen voor de nieuwe technloglie CRI-O en ga deze wat meer toelichten. Dit is een nieuwe container oplossing waarbij je docker niet meer nodig hebt. Dit werd gemaakt door het Google en Redhat in functie van het Kubernetes project.

## Wat is CRI-O

Eerst een vooral ga ik toelichten waarom dit is geïntoduceerd. Hierbij gaan we moeten bekijken wat **docker** is en waarom het voor het **Kubernetes** project niet meer de beste oplossing was. Hierin gaan we ook bespreken wat een **container** is. En ook wat **CRI-O** is in functie van dat.

### Docker
**Docker** is een softwaretechnologie die containers voorziet. Een container kan je vergelijken met een virtuele machine maar is een principe veel beter. Docker is ook de populairste softwaretechnologie hiervoor.

### Container 
**Virtuele machine** softwaren zoals Hyper-V zijn gebaseerd op het nabootsen van virtule hardware. Ze hebben dus een groot aantal systeemvereisten als je deze wilt opzetten. 
Terwijl **containers** gedeelde besturingssystemen gebruiken. Ze zijn dus veel efficiënter dan virtuele machines. In plaats van al de hardware te virtualizeren, werkt een container op de top van een linux instance. Zodat je alleen de benodigde dingen hebt. 90 % heeft de container niet in vergelijking met een virtuele machine, maar dit zijn dingen die je niet nodig hebt en die ervoor zorgen dat de container veel performanter is.  Hierdoor kan je meer dan 4 of 5 keer meer containers hebben op een systeem dan VM's.

![docker-vm-container.png](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/pictures/docker-vm-container.png)

### Kubernetes
**Kubernetes** is een open-sources systeem voor het het automatiseren, beheren en schalen van containers. Het draait simpel uitgelegd clusters van containers. Dit is de technologie die google al  jaren gebruikt. Terwijl het pas 2,5 jaar(beginnen vanaf 01/2018) geleden werd publiek gemaakt.

Kubernetes maakte in het begin gebruik van het docker platform. Maar de docker engine verandert vrij veel. Dit komt door dat deze engine veel 'breakt'. Dit zorgt dus voor de nodige problemen. Dus werkte Google (eigenaar van het project) en Redhat samen om een nieuwe engine te maken namelijk **CRI-O**. 

### CRI-O
CRI-O is een open source project. Het zorgt ervoor dat je containers direct van Kubernetes kunt draaien zonder een bijgevoegde tool of code.

Het grootste probleem was de container runtime, door het gebruiken van CRI-O is dit opgelost. Waardoor alle users overschakelen op CRI-O omdat dit een standaard is die werkt en waar je geen verdere implementaties moet doen voor docker fouten.

#### Werking
Als Kubernetes een container wilt uitvoeren 'bespreekt' die dit met CRI en daarop bespreekt de CRI-O deamon dit met runc (soort van OCI-comppliant runtime). Als kubernetes de container wilt stoppen doet CRI-O dit. Het doet dit allemaal achter schermen zodat de gebruiker zich geen zorgen moet maken over dit cruciale stuk van de container orkestratie.

![CRI-Ov1_Chart_1.png](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/pictures/CRI-Ov1_Chart_1.png)

## Proof-of-Concept

1. Eerst een vooral heb ik een Ubuntu 16.10 machine nodig .Ik kies voor de bento/ubuntu-16.10 box. Ik voer deze commando's uit.

```
vagrant init bento/ubuntu-16.10
vagrant up

```
2. Daarna doe ik een vagrant ssh in de box en verander naar de root met het wachtwoord: vagrant.

```
vagrant ssh
[vagrant@localhost ~]$ su

```
    
3.  Eerst download ik runc

```
wget https://github.com/opencontainers/runc/releases/download/v1.0.0-rc4/runc.amd6

```
    
4.  Dan verander ik het excetuble bit and kopieer the runc binary naar mijn pad

```
chmod +x runc-linux-amd64 
sudo mv runc-linux-amd64 /usr/bin/runc

```
5.  Dan download ik de GO binary release
```
wget https://storage.googleapis.com/golang/go1.7.4.linux-amd64.tar.gz

```
6. Hierna installeer ik GO.
```
sudo tar -xvf go1.7.4.linux-amd64.tar.gz -C /usr/local/
mkdir -p $HOME/go/src
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```
7. Nu bouw ik ocid van de bron
```
sudo apt-get install -y libglib2.0-dev libseccomp-dev libapparmor-dev
go get -d github.com/kubernetes-incubator/cri-o/...
cd $GOPATH/src/github.com/kubernetes-incubator/cri-o
make install.tools
make
sudo make install
```
8. als we **sudo sh -c 'echo "[Unit]** doen moet het bestand er zo uitzien.

```
    Description=OCI-based implementation of Kubernetes Container Runtime Interface
    Documentation=https://github.com/kubernetes-incubator/cri-o

    [Service]
    ExecStart=/usr/bin/ocid --debug
    Restart=on-failure
    RestartSec=5

    [Install]
    WantedBy=multi-user.target" > /etc/systemd/system/ocid.service'
```
    
9. start nu het ocid system daemon
```
sudo systemctl daemon-reload
sudo systemctl enable ocid
sudo systemctl start ocid
```
10. Download nu de cni source tree
```
go get -d github.com/containernetworking/cni
cd $GOPATH/src/github.com/containernetworking/cni
```
11. Bouw de cni binaries
```
./build
```
12. Installeer de cni binaries
```
sudo mkdir -p /opt/cni/bin
sudo cp bin/* /opt/cni/bin/
```
13. Configureer nu cni
```
sudo mkdir -p /etc/cni/net.d
```

```
sudo sh -c 'cat >/etc/cni/net.d/10-mynet.conf <<-EOF
{
    "cniVersion": "0.2.0",
    "name": "mynet",
    "type": "bridge",
    "bridge": "cni0",
    "isGateway": true,
    "ipMasq": true,
    "ipam": {
        "type": "host-local",
        "subnet": "10.88.0.0/16",
        "routes": [
            { "dst": "0.0.0.0/0"  }
        ]
    }
}
EOF'
```

```
sudo sh -c 'cat >/etc/cni/net.d/99-loopback.conf <<-EOF
{
    "cniVersion": "0.2.0",
    "type": "loopback"
}
EOF'
```

14. Donwload de docker binary releases
```
wget https://get.docker.com/builds/Linux/x86_64/docker-1.12.4.tgz
```
15. extract en installeer de docker binaries
```
tar -xvf docker-1.12.4.tgz
sudo cp docker/docker* /usr/bin/
```
16. als we **sudo sh -c 'echo "[Unit]** doen moet het bestand er zo uitzien.

```
	Description=Docker Application Container Engine
	Documentation=http://docs.docker.io

    [Service]
    ExecStart=/usr/bin/docker daemon \
      --iptables=false \
      --ip-masq=false \
      --host=unix:///var/run/docker.sock \
      --log-level=error \
      --storage-driver=overlay
    Restart=on-failure
    RestartSec=5

    [Install]
    WantedBy=multi-user.target" > /etc/systemd/system/docker.service'
```    
    
17. start de docker daemon    
```    
sudo systemctl daemon-reload
sudo systemctl enable docker
sudo systemctl start docke    
```

18. Creër nu een pod
```
    cd $GOPATH/src/github.com/kubernetes-incubator/cri-o
    POD_ID=$(sudo ocic pod create --config test/testdata/sandbox_config.json)
```
    
19. Creëer een redis container in de pod.     
```
 CONTAINER_ID=$(sudo ocic ctr create --pod $POD_ID --config test/testdata/container_redis.json)

```
20. start de redis container.     
```
    sudo ocic ctr start --id $CONTAINER_ID
```
21. test nu de redis container. Connecteer naar de pod op poort 6379
```
telnet 10.88.0.2 6379
```
22. Typ **Monitor** in de prompt
```
Trying 10.88.0.2...
Connected to 10.88.0.2.
Escape character is '^]'.
MONITOR
+OK
```    
23. Stop de sessie door **CTRL+C**  te doen.

24. de redis logs kan je bekijken via
```
sudo journalctl -u ocid --no-pager
```
    
    
    
## Bronnen

Zdnet article docker: <http://www.zdnet.com/article/what-is-docker-and-why-is-it-so-darn-popular/>
Docker Wiki: <https://en.wikipedia.org/wiki/Docker_(software)>
Docker en Kubernetes Forbes: <https://www.forbes.com/sites/mikekavis/2017/04/28/docker-and-kubernetes-friends-or-foes/#5a82d3be3394>
Kubernetes: <https://kubernetes.io/>
Docker: <https://www.docker.com/>
CRI-O: <http://cri-o.io/>
CRI-O Redhat: <https://www.redhat.com/en/blog/introducing-cri-o-10>
cri tutorial: <https://github.com/kelseyhightower/cri-o-tutorial>
