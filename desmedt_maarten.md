# Evaluatie Enterprise Linux

| :---          | :---                                             |
| Student       | Maarten DE SMEDT                                 |
| Klasgroep     | aalst                                            |
| Email         | <mailto:maarten.desmedt.v5415@student.hogent.be> |
| Hoofdopdracht | SME                                              |
| Repo          | <git@github.com:MaartenDeS/elnx-sme.git>         |

## Hoofdopdracht

W4:

- Nog bezig met labo 0! -> tandje bijsteken!
    - software installeren
    - SSH-key
    - nog nooit testscript uitgevoerd

W9 (op basis van repo):

- Laatste commit dateert van 2w geleden. Push al je wijzigingen naar Github telkens je aan de labo's werkt!
- Nog bezig met LAMP-stack? Dit gaat niet vooruit.

Vraag tijdig hulp tijdens de lesmomenten! Graag komende les een update.

Eindevaluatie:

- Acceptatietests:
    - LAMP, DNS ok
    - File server NIET ok


### Eindbeoordeling

O1: Nog niet bekwaam

## Troubleshooting

### Eerste troubleshooting-opdracht

- Algemeen:
    - Proces is niet systematisch/grondig. Volg strikt de stappen van bottom-up troubleshooting. Elke fase is een laag van het TCP/IP-model
    - Je moet telkens verwachte en bekomen waarden opgeven!
- Datalinklaag:
    - Is de VM aangesloten op een HO-interface met correcte instellingen?
- Internetlaag:
    - Wat is het IP-adres? Kan je pingen van host naar VM? Het heeft geen zin om verder te gaan als dit niet werkt

Gebruik aub. de bottom-up-strategie die we in de les toegelicht hebben!

Voor deze opgave: nog niet bekwaam

### Tweede troubleshooting-opdracht

- Netwerkconfiguratie ok
- Service start niet op

Beoordeling: nog niet bekwaam

### Derde troubleshooting-opdracht

- Thuis afgewerkt, tijdens demo is toch nog een fout naar boven gekomen
- Fout opgelost, resultaat getoond

### Eindbeoordeling

O2: Bekwaam

## Opdracht Actualiteit

Niet afgewerkt

### Eindbeoordeling

O3: Nog niet bekwaam

## Rapportering

### Laboverslagen

- Laboverslagen bevatten vooral copy/paste van de broncode. Dat is weinig zinvol
- Er zijn geen testverslagen met transcripts van de acceptatietests en/of screenshots van het eindresultaat

R1: Nog niet bekwaam

### Demonstraties

Geeft blijk van onvoldoende inzicht in basiskennis Linux

R2: Nog niet bekwaam

### Cheat sheet

Niet bijgehouden. Nochtans zou je dit goed kunnen gebruiken!

R3: Nog niet bekwaam

## Tweede zittijd

- Hoofdopdracht afwerken
- Actualiteitsopdracht uitwerken
- Extra taak (details op Chamilo): Webserver gebaseerd op Docker


Aanwijzingen ivm hoofdopdracht:

- Samba:
    - Filepermissies moeten overeenkomen met permissies in smb.conf
    - SELinux context toekennen: public_content_t of public_content_rw_t
    - Alle tests uitvoeren!
    - Als tests falen moet je uitzoeken waarom en proberen manueel het commando uit te voeren
- DHCP:
    - Slechts één range definieren
    - DNS server voor werkstations mag niet 192.0.2.10 zijn. De router moet ingesteld worden als een forwarder die zowel voor interne als publieke hostnamen werkt
- Router:
    - NAT regel fixen
    - Forwarden van externe namen naar de DNS-server op de NAT interface ipv 8.8.8.8

