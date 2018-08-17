# Opdracht Actualiteit - Speed Up Ansible Playbooks

## Inleiding

Als deel voor de eintaak van het vak Linux moest ik ook een actua opdracht kiezen. We hadden 2 opties zelf een bijdrage leveren aan een open source project of een tool toepasse, op onze hoofdopdracht.

Ik koos ervoor om een tool toe te passen op de mijn hoodfopdracht. Ik ga volgens het artikel van Johnson "Making Ansible a Bit Faster" (link in de bron), mijn ansible opstelling sneller proberen maken. In deze taak ga ik bekijken of zijn aanbevelingen die hij aantoonde klopte. Daarbij ook een optimale snelheid proberen te vinden voor mijn ansible opstelling.

In deze taak test ik alles telkens op alle servers. Ik voer ook altijd telkens een ``vagrant provision`` uit  en geen ``vagrant up `` aangezien we enkel het asnible playbook testen. De test computer is een HP Envy 700-014eb. De specificaties en foto zijn te vinden hieronder.

| HP ENVY 700-014EB |                |
| :---           | :---           |
| **Processor**     | Intel Core i7 4770              | 		
| **Processorkernen** | Quad core (4)              | 
| **Kloksnelheid** | 3400MHz              | 
|**Turbo Frequentie** | 3900 MHz            | 
| **RAM-Geheugen**   | 16 GB | 
| **Type RAM-Geheugen**  | DIMM DDR3 | 
| **Opslagtype**  | SSD | 
| **Moederbord** | MS-7826 (Kaili)  |

*Tabel 1: Specificaties HP Envy 700-014eb*

<img src="https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/Foto's/hp.png" width="500">

*Foto 1: HP Envy 700-014eb*


## Tijdsmeting Plugin

Het allereerste wat ik moet doen is een tijdsmeting plugin installeren zodat ik alles kan navolgen. Via het artikel van Johnson kwam ik op een Ansible plugin terecht. Deze plugin is gemaakt door jlafon en is voor Ansible 1.x en voor Ansible 2.0.

Ik clownde de repository en volgde de instructies. Er waren 2 verschillende mogelijkheden om deze plugin toe te voegen. De eerste was voor Ansible 2.0, de andere voor Ansible 1.x. 

Voor de manier van Ansible1.x  moest ik de map **callback_plugins** met daarin de file **profile_tasks.py** in de map naast mijn playbook. Deze map in mijn opstelling was de map Ansible. Toen ik hierna testte kon ik de tijdsmetingen zien.

De manier voor Ansible 2.0 leek werkte helemaal anders. Hierin moest ik in de **ansible.cfg** file de lijn `` pipelining = True`` zetten op zich gee probleem. De opstelling gebruikte de default locatie van deze file *etc/ansible/ansible.cfg*. Deze kan je overriden door in je home directory ook een **ansible.cfg** file te zetten. In de opstelling is de home directory */home/vagrant/*. Eens ik deze had gevonden maakte ik in Ansible een pre_task aan zodat er elke keer de machine opstart een **ansible.cfg** file wordt toegevoegd in die map. In deze file stond de juiste setting dus wel aan.


## Snelheid Normale Ansible

Nu kan ik de standaardtijden meten van Ansible. Ik doe 5 tijdsmetingen zodat ik een goed beeld heb van wat de normale tijd ongeveer is. Hieronder zijn de  de tijden een tabel gegooid voor een duidelijk overzicht. Ook ziet u een foto van hoe de weergave er uitzag. De tijden per server werden bij elkaar opgeteld.


![voorbeeld](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/Foto's/voorbeeld.png)
*Foto 2: voorbeeld tijdsweergave*



| Tijdsmeting |Tijd               |
| :---           | :---           |
| Tijdsmeting 1  | 0:01:34,948      | 		
|Tijdsmeting 2 | 0:01:33,628 | 
| Tijdsmeting 3 |0:01:33,963        | 
|Tijdsmeting 4 | 0:01:36,330     | 
| Tijdsmeting 5| 0:01:36,593    | 

*Tabel 2: Tijdsweergavenormale ansible*

Hieruit kunnen we afleiden dat de gemiddelde normale tijd  1:35,092 is, de mediaan is 1:34,948.



## SSH pipelining

SSH pipelining is volgens Johnson een makkelijke manier om Ansible te versnellen. Het is een ansible feature om het aantal verbindingen naar een host te verminderen. Ansible zou normaal een tijdelijke *~/.ansbible* (via SSH). Dan zulllen ze voor elke task de module kopiëren naar de directory met sftp of scp en dan deze module uitvoeren (weer met SSH). Als pipelining aanstaat zal Ansible maar 1 keer per taak connecteren met ssh namelijk met het uitvoeren met python. de module zal worden geschreven naar de stdin. Om deze functie aan te zetten heb je 2 mogelijkheden. Je kan dit doen via jouw **playbook** of via het **ansible.cfg** bestand.

Voor de manier met het **ansible.cfg** bestand gaan we naar het ansible.cfg bestand dat we kopiëren naar onze home directory. In dat bestand voegen we de regel ``pipelining = True`` toe onder de regel `` [ssh_connection]``.

De tweede manier met het **playbook** is een beetje anders. Daarbij voegen we bij ``- hosts: pr011`` eerst de regel ``vars: `` toe. Dan onder die regel met de juiste syntax voegen we de regel ``ansible_ssh_pipelining: yes``toe.

Ik heb de manier in het **ansible.cfg** bestand toegpast. Om te bekijken of pipelining aanstaat gebruiken we het commando `` ansible localhost -vvv -m shell -a 'echo ok' ``. Als pipelining aanstaat krijg ik deze output(zie foto 3). Als pipeling uitstaat krijg je een veel langere output(zie foto 4). Dan roept hij 3 tot 5 keer het **command.py** bestand op. Op de foto duidelijk maar 1 keer en daaruit blijkt dat pipelining aanstaat.

![pipeaan](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/Foto's/pipeaan.png)
*Foto 3: SSH pipelining output - Pipelining aan*

![pipeuit](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/Foto's/pipeuit.png)
*Foto 4: SSH pipelining output - Pipelining uit*


Ik doe weer 5 tijdsmetingen zodat ik weer een goed beeld heb van de gemiddelde tijd. Hieronder kan je de tijden vinden in een tabel


| Tijdsmeting |Tijd             |
| :---           | :---           |
| Tijdsmeting 1  | 0:01:32,605    | 		
|Tijdsmeting 2 | 0:01:30,442      | 
| Tijdsmeting 3 |0:01:32.551        | 
|Tijdsmeting 4 | 0:01:30.932     | 
| Tijdsmeting 5| 0:01:30,883    | 


*Tabel 3: Tijdsweergave SSH pipelinning*

Hieruit kunnen we afleiden dat de gemiddelde tijd met SSH pipelining 1:31,519  is, de mediaan is 1:30,932.


## ControlPersist

ControlePersist is een SSH-functie die de verbinding met de server open houdt, klaar om deze te hergebruiken tussen aanroepingen. Johnson vertelt in het artikel dat het default Control_path meer dan 102 karakters lang is. En door deze te veranderen naar de */tmp* een speed improvment zal krijgen.

Dit doe ik door in het **ansible.cfg** bestand onder de lijn van pipelining het volgende toevoeg `` control_path = /tmp/ansible-ssh-%%h-%%p-%%r ``.

Ik test weer 5 keer de tijdsafmetingen, hieronder de tabel. 

| Tijdsmeting |Tijd               |
| :---           | :---           |
| Tijdsmeting 1  | 0:01:28,232    | 		
|Tijdsmeting 2 | 0:01:32,508          | 
| Tijdsmeting 3 |0:01:30,966       | 
|Tijdsmeting 4 | 0:01:30,951   | 
| Tijdsmeting 5| 0:01:30,722   | 

*Tabel 4: Tijdsweergave ControlPersist*

Hieruit kunnen we afleiden dat de gemiddelde tijd met SSH pipelining en ControlPersist 1:30,966 is, de mediaan is 1:30,951.

## Async

Johnson spreekt in zijn artikel ook nog over een 3de stap. Namelijk async. Met Async tasks kan je terwijl er 1 lange taak aan het runnen is, ook nog een andere klein uitvoeren. 

Bij mijn setup zou dit moeilijk werken aangezien je met post en pre tasks werkt. Bij pr011 zijn er de meeste tasks dus daar heb ik screenshots genomen van de tijd van de tasks. Aan deze tijden te zien is er misschien wel een window bij het kopieren van de ansible.cfg file waar je acl's kunt aanmaken. Maar aangezien het dat na het overlopen van het playbook moet gebeuren en het kopieren ervoor, gaat dit niet. Ook is dit meer voor tasks die een lange tijd duren. Deze beide tasks duren maar een halve second. 


![file](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/Foto's/file.png)

*Foto 5: Tijdsduur Kopie Tasks*


![acl](https://github.com/MaartenDeS/elnx-sme/blob/soluation/Actua/Foto's/acl.png)

*Foto 6: Tijdsduur ACL Tasks*


## Conclusie 

De opties die we hebben toegevoegd in Ansible werken en Ansible werkt nu sneller, zoals Johnson zei. Bij SSH Pipelining  is er een groter verschil in tijd dan ControlPersist. De beste optie om Ansible te versnellen is dus duidelijk SSH Pipelining. Al zorgt ControlPersist wel voor een kleine toename in snelheid. 

Maar wat anders is bij Johnson dan bij ons is dat daar de verschillen zeer merkbaar waren. Terwijl hier gaat het maar om een paar seconden. In zijn artikel spreekt hij over een toename in snelheid van 30% maar dat is hier nauwelijks te merken. In zijn artikel zelf staat nergens hoe groot zijn playbook was dus een vergelijking kunnen we moeilijk maken. Al zou het goed kunnen dat als je playbook groter is de verschillen in tijd ook groter worden, dit kon wel niet getest worden aangezien er geen ander playbook is.



| Versie		|Gemiddelde               | Laagste Waarde | Hoogste Waarde | Mediaan |
| :---           | :---           |:---           |:---           |:---           |
| Normaal  		| 1:35,092		| 	1:33,628		|  1:36,593				|1:34:948 			| 		
| SSH Pipelining  | 1:31,519		| 1:30,442 				| 1:32,605 				| **1:30,932** 			| 		
| ControlPersist  | **1:30,966**		| **1:28,232** 				| **1:32,508** 				|1:30,951 		| 		

*Tabel 5: Samenvatting (Bold: laagste waarde)*




## Bronnen
- Johsnon - 2015: <https://adamj.eu/tech/2015/05/18/making-ansible-a-bit-faster>
- jlafon - 2016: <https://github.com/jlafon/ansible-profile>
- Ansible.cfg file manage: <http://itmithran.com/installation-configuration-ansible/>
- Uitleg SSH pipelining: <http://toroid.org/ansible-ssh-pipelining>
- Check Pipelining Aan: <https://stackoverflow.com/questions/43438519/check-if-ansible-pipelining-is-enabled-working>
- Voorbeeld Ansible.cfg file: <https://github.com/opentable/ansible-examples/blob/master/ansible.cfg>
- Async: <http://acalustra.com/acelerate-your-ansible-playbooks-with-async-tasks.html>

