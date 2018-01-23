# Opdracht Actualiteit - CRI-O

## Inleiding

Als een deel van de linux taak moesten we ook een actua taak kiezen. Ik heb gekozen voor de nieuwe technloglie CRI-O en ga deze wat meer toelichten. Dit is een nieuwe container oplossing waarbij je docker niet meer nodig hebt. Dit werd gemaakt door het Kubernetes project.

## Wat is CRI-O

Eerst een vooral ga ik toelichten waarom dit is geïntoduceerd. Hierbij gaan we eerst moeten bekijken wat docker is en waarom het Kubernetes project dit niet meer de beste oplossing vond

### Docker
Docker is een softwaretechnologie die containers voorziet. Een container kan je vergelijken met een virtuele machine maar is een principe veel beter. Docker is ook de populairste softwaretechnologie hiervoor

#### Container 
Virtuele machine softwaren zoals Hyper-V zijn gebaseerd op het nabootsen van virtule hardware. Ze hebben dus een groot aantal systeemvereisten als je deze wilt opzetten. 
Terwijl containers gedeelde besturingssystemen gebruiken. Ze zijn dus veel efficiënter dan virtuele machines. In plaats van al de hardware te virtualizeren, werkt een container op de top van een linux instance. Zodat je alleen de benodigde dingen hebt. 90 % heeft de container niet in vergelijking met een virtuele machine, maar dit zijn dingen die je niet nodig hebt en die ervoor zorgen dat de container veel performanter is. (Gevisualisserd in de afbeelding)

![docker-vm-container.png](.\docker-vm-container.png)




## Bronnen

Zdnet article: <http://www.zdnet.com/article/what-is-docker-and-why-is-it-so-darn-popular/>
Docker Wiki: <https://en.wikipedia.org/wiki/Docker_(software)>