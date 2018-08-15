# Opdracht Actualiteit - Speed Up Ansible Playbooks

## Inleiding

Als deel voor de eintaak van het vak Linux moest ik ook een actua opdracht kiezen. We hadden 2 opties zelf een bijdrage leveren aan een open source project of een tool toepasse, op onze hoofdopdracht.

Ik koos ervoor om een tool toe te passen op de mijn hoodfopdracht. Ik ga volgens het artikel van Johnson "Making Ansible a Bit Faster" (link in de bron), mijn ansible opstelling sneller proberen maken. In deze taak ga ik bekijken of zijn aanbevelingen die hij aantoonde klopte. Daarbij ook een optimale snelheid proberen te vinden voor mijn ansible opstelling.

In deze taak test ik alles telkens alleen op de server pr011. Ik voer ook altijd telkens een ``vagrant provision pr011`` uit  en geen ``vagrant up pr011 `` aangezien we enkel het asnible playbook testen. De test computer is een HP Envy 700-014eb. De specificaties en foto zijn te vinden hieronder.

| HP ENVY 700-014EB |                |
| :---           | :---           |
| **Processor**     | Intel Core i7 4770              | 		
| **Processorkernen** | Quad core (4)              | 
| **Kloksnelheid** | 3400MHz              | 
|** Turbo Frequentie** | 3900 MHz            | 
| **RAM-Geheugen**   | 16 GB | 
| **Type RAM-Geheugen **  | DIMM DDR3 | 
| **Opslagtype **  | SSD | 
| **Moederbord ** | MS-7826 (Kaili)  |
*Tabel: Specificaties HP Envy 700-014eb*

![Computer]()
*Foto: HP Envy 700-014eb*


## Tijdsmeting Plugin
Het allereerste wat ik moet doen is een tijdsmeting plugin installeren zodat ik alles kan navolgen. Via het artikel van Johnson kwam ik op een Ansible plugin terecht. Deze plugin is gemaakt door jlafon en is voor Ansible 1.x en voor Ansible 2.0.

Ik clownde de repository en volgde de instructies. Er waren 2 verschillende mogelijkheden om deze plugin toe te voegen. De eerste was voor Ansible 2.0, de andere voor Ansible 1.x. 
Voor de manier van Ansible1.x  moest ik de map **callback_plugins** met daarin de file **profile_tasks.py** in de map naast mijn playbook. Deze map in mijn opstelling was de map Ansible. Toen ik hierna testte kon ik de tijdsmetingen zien.
De manier voor Ansible 2.0 leek werkte helemaal anders. Hierin moest ik in de **ansible.cfg** file de lijn `` pipelining = True`` zetten op zich gee probleem. De opstelling gebruikte de default locatie van deze file *etc/ansible/ansible.cfg*. Deze kan je overriden door in je home directory ook een **ansible.cfg** file te zetten. In de opstelling is de home directory */home/vagrant/*. Eens ik deze had gevonden maakte ik in Ansible een pre_task aan zodat er elke keer de machine opstart een **ansible.cfg** file wordt toegevoegd in die map. In deze file stond de juiste setting dus wel aan.


## Snelheid Normale Ansible

Nu kan ik de standaardtijden meten van Ansible. Ik doe 10 tijdsmetingen zodat ik een goed beeld heb van wat de normake tijd ongeveer is. Hieronder zijn de  de tijden een tabel gegooid voor een duidelijk overzicht. Ook ziet u een foto van hoe de weergave er uitzag
![voorbeeld]()

FTOO

| Tijdsmeting |Tijd (Plugin weergave - Seconden)               |
| :---           | :---           |
| Tijdsmeting 1  | 0:00:22 - 22 seconden     | 		
|Tijdsmeting 2 | 0:00:21 - 21 seconden         | 
| Tijdsmeting 3 |0:00:22 - 22 seconden        | 
|Tijdsmeting 4 | 0:00:21 - 21 seconden     | 
| Tijdsmeting 5| 0:00:21 - 21 seconden    | 
| Tijdsmeting 6  | 0:00:21 - 21 seconden     | 		
|Tijdsmeting 7 | 0:00:21 - 21 seconden         | 
| Tijdsmeting 8 |0:00:24 - 24 seconden        | 
|Tijdsmeting 9 | 0:00:20 - 20 seconden     | 
| Tijdsmeting 10| 0:00:21 - 21 seconden    | 

Hieruit kunnen we afleiden dat de gemiddelde normale tijd  21,4 seconden is, de mediaan is 21 seconden.



## SSH pipelining





## Bronnen
Johsnon - 2012: <https://adamj.eu/tech/2015/05/18/making-ansible-a-bit-faster>
jlafon - 2016: <https://github.com/jlafon/ansible-profile>
<http://itmithran.com/installation-configuration-ansible/>
<http://toroid.org/ansible-ssh-pipelining>