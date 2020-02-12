![panda](  https://github.com/boschpeter/ansible/blob/master/pictures/08B0E82E-5268-4BFD-96F6-99527EC96885.jpeg)

# ansible

Met Ansible kunt u deen hele omgeving voorzien van een enkele opdracht en enkele eenvoudige SSH-opdrachten automatiseren. Automatische implementaties zijn tegenwoordig in zwang. Het lijkt steeds relevanter te worden om te weten hoe automatisering moet worden aangepakt. Ansible is een Zwitsers zakmes voor DevOps, in staat om veel krachtige automatiseringstaken uit te voeren met de flexibiliteit om zich aan te passen aan vele omgevingen en workflows. Aan de hand van voorbeelden uit de echte wereld deelt dezer repo hopelijk "de essentie" voor alle Ansible-gebruikers. Of je nu gloednieuw bent bij Ansible of op zoek bent naar een opfriscursus, de wiki  biedt de best practices die je moet weten. (dat is tenminste mijn gedachte)
Lessons-learned 

Ansible is een stukje agentloze configuratiebeheersoftware die via SSH verbinding kan maken met unix-achtige (bijv. Linux) en Windows-gebaseerde computers om instructies te leveren. Deze instructies zijn normaal geschreven in YAML-bestanden (wat staat voor YAML Ain't Markup Language) -bestanden genaamd playbooks. Playbooks bevatten rollen (toneelstukken), die taken bevatten (die opeenvolgend worden uitgevoerd) en taken kunnen modules aanroepen (die in mijn favoriete taal, Python kunnen worden geschreven).


Ansible is een radicaal eenvoudig IT-automatiseringsplatform dat uw applicaties en systemen eenvoudiger inzetbaar maakt. Vermijd het schrijven van scripts of aangepaste code om uw applicaties te implementeren en bij te werken - automatiseer in een taal die gewoon Engels benadert, met behulp van SSH, zonder agenten om op externe systemen te installeren

# git init

````
git init
git config --global credential.helper store
git config --global user.email "bosch.peter@icloud.com"
git config --global user.name "boschpeter"
git config --global user.password "

git config --list
`````
 De echte magie van infrastructuur als codetools, zoals Ansible, ligt in het vermogen om gegevens en code te scheiden. In het voorbeeld is het bestand default.conf een configuratiebestand dat specifiek is voor een Nginx-webserver. De configuratieparameters, zoals poorten, gebruikers, paden, enzovoort, blijven te allen tijde generiek en constant, ongeacht wie ze installeert en configureert. Wat niet constant is, zijn de waarden die deze parameters aannemen. Dat is wat specifiek is voor onze organisatie. Dus, hiervoor zouden we het volgende beslissen: Welke poort moet Nginx gebruiken? Welke gebruiker moet eigenaar zijn van het webserverproces? Waar moeten de logboekbestanden naartoe? Hoeveel werkprocessen moeten worden uitgevoerd? Ons organisatiespecifieke beleid kan ook vereisen ons om verschillende waarden door te geven aan deze parameters op basis van de omgeving of geografie waarin de hosts draaien. Kan splitsen deze in twee delen: De code die generiek is De gegevens die specifiek zijn voor een organisatie Dit heeft twee voordelen; een voordeel is dat het ons probleem van statische gegevensexplosie oplost. Nu we de code en gegevens hebben gescheiden, kunnen we configuratiebestanden flexibel en dynamisch maken. Het tweede voordeel, u zich misschien realiseren, is nu dat de code en gegevens zijn gesplitst, er is niets in de code dat specifiek is voor een bepaalde organisatie. Dit maakt het gemakkelijk om de site met de wereld te delen voor iedereen die het nuttig vindt. Dat is precies wat je zou vinden op Ansible-Galaxy of zelfs op GitHub, wat de groei van tools, zoals Ansible, stimuleert. In plaats van het wiel opnieuw uit te vinden, kunt u de code downloaden die iemand anders heeft geschreven, deze aanpassen, de gegevens voor de code invullen en het werk doen
 
Hoe staat deze code los van de gegevens? Het antwoord is dat Ansible twee primitieven heeft: Jinja-sjablonen (code) De variabelen (gegevens) In het volgende diagram wordt uitgelegd hoe het resulterende bestand wordt gegenereerd op basis van sjablonen en variabelen: Sjablonen bieden plaatshouders in plaats van parameterwaarden, die vervolgens worden gedefinieerd in variabelen. Variabelen kunnen vervolgens vanuit verschillende plaatsen worden ingevoerd, waaronder rollen, playbooks, inventarissen en zelfs vanaf de opdrachtregel wanneer u Ansible start.

![j2](https://github.com/boschpeter/ansible/blob/master/pictures/file_generated_from_template_and_variables.PNG)

# Ninja2 templates 
Waar gaat Jinja over? Jinja2 is een zeer populaire en krachtige op Python gebaseerde sjabloon-engine. Aangezien Ansible in Python is geschreven, wordt het de standaardkeuze voor de meeste gebruikers, net als andere op Python gebaseerde configuratiebeheersystemen, zoals Fabric en SaltStack. De naam Jinja is afkomstig van het Japanse woord voor tempel, wat qua fonetiek vergelijkbaar is met de woordsjabloon. Enkele belangrijke kenmerken van Jinja2 zijn: het is snel en precies op tijd gecompileerd met de Python-bytecode Het heeft een optionele sandbox-omgeving Het is gemakkelijk naar debugIt ondersteunt template-overervingThe template-formatie Templates lijken erg op normale tekstgebaseerde bestanden behalve de occasionele variabelen of code die de speciale tags omgeeft. Deze worden geëvalueerd en worden meestal vervangen door waarden tijdens runtime, waardoor een tekstbestand wordt gemaakt dat vervolgens naar de doelhost wordt gekopieerd. Dit zijn de twee soorten tags die Jinja2-sjablonen accepteren: {{}} sluit variabelen in een sjabloon in en drukt de waarde ervan af in het resulterende bestand. Dit is het meest voorkomende gebruik van een sjabloon. Bijvoorbeeld: {{nginx_port}}
 {%%} sluit code-instructies in een sjabloon in, bijvoorbeeld voor een lus worden de if-else-instructies ingesloten, die tijdens runtime worden geëvalueerd maar niet worden afgedrukt
 
 # Facts
 Veel gegevens in onze systemen worden automatisch ontdekt en beschikbaar gesteld aan Ansible door de beheerde hosts tijdens het handshake-proces. Deze gegevens zijn zeer nuttig en vertellen ons alles over dat systeem, zoals: De hostnaam, netwerkinterface en IP-adres De systeemarchitectuur Het besturingssysteem De schijven De gebruikte processor en de hoeveelheid geheugen Of het nu een VM is; zo ja, is het een virtualisatie- / cloudprovider? TipFacts worden verzameld aan het begin van een Ansible-run. Herinner je je de regel in de uitvoer die VERZAMELENDE FEITEN ******* zegt? Dat is precies wanneer dit gebeurt.
 
# Variabelen 

![var_precedence](https://github.com/boschpeter/ansible/blob/master/pictures/code_tree_nginx_role.PNG)


 ## Variablen de default-directory  binnen een rol 
Waar en hoe een variabele kan worden gedefinieerd, is een complex fenomeen, omdat Ansible in dit opzicht overvloedige keuzes biedt. Dit biedt gebruikers ook veel flexibiliteit om delen van hun infrastructuur verschillend te configureren. Alle Linux-hosts in een productieomgeving moeten bijvoorbeeld lokale pakketrepository's of webservers gebruiken voor staging en moeten op poort 8080 worden uitgevoerd. Dit alles zonder de code te wijzigen, en alleen aangestuurd door gegevens, gebeurt door variabelen. plaatsen waar Ansible variabelen accepteert: de default-directory  binnen een rol 

## Inventory variabelen 
De parameters host_vars en group_vars gedefinieerd in afzonderlijke mappen. De parameter host / group vars gedefinieerd in een inventarisbestand Variabelen in playbooks en rolparameters De map vars in een rol en variabelen gedefinieerd in een verstrekte play 

##  Extra-variabelen met de optie -e tijdens runtime

tot zover  feiten, variabelen en sjablonen.

# Templating the Nginx configurations
Laten we nu onze Nginx-rol transformeren tot gegevensgestuurd. We beginnen met het sjablonen van het default.conf-bestand voor Nginx dat we eerder hebben gemaakt. De aanpak voor het converteren van een bestand naar een sjabloon zou als volgt zijn: M

## 1 maak de mappen die nodig zijn om sjablonen en standaardvariabelen in een rol te houden: 

|create directories|
|--------------------------------|
|mkdir rollen / nginx / templates|
|mkdir rollen / nginx / defaults| 

````ansible-galaxy init --init-path roles/ nginx````


## 2 Begin altijd met het uiteindelijke configuratiebestand, 
Begin altijd met het eigenlijke configuratiebestand, ons eindresultaat van dit proces, om alle parameters te kennen die het zou kosten. relax. De configuratie voor het bestand default.conf op ons systeem is bijvoorbeeld als volgt: 

````
server {
       listen 80; 
       servername localhost; 
     location / {
     root / usr / share / nginx / html; 
     index index.html; 
     }
 } 
````

## 3 Identificeer de configuratieparameters die u dynamisch wilt genereren,

verwijder de waarden voor die parameters, noteer ze afzonderlijk en vervang ze door sjabloonvariabelen: 

Template Snippets: 
 listen {{nginx_port}}
 root {{ nginx_root }}; 
 index {{ nginx_index }}; 
 
 Variables: 
 nginx_port: 80 
 nginx_root: /usr/share/nginx/html 
 nginx_index: index.html
 
De waarden voor een van de configuratieparameters worden verondersteld  afkomstig te zijn van feiten, meestal systeemparameters of topologie-informatie, zoals de hostnaam, het IP-adres, enzovoort, zoek dan de relevante feiten met behulp van de volgende opdracht: Bijvoorbeeld: ````$ ansible -i customhosts www -m setup | less ````

## 4 Gebruik het  ontdekte feit in de sjabloon in plaats van een door de gebruiker gedefinieerde variabele. 

Bijvoorbeeld: servernaam {{ansible_hostname}}, Om de hostnaam van het systeem te achterhalen: 
````ansible -i customhosts dingofarm_workers-m setup | grep -i hostname ````

"ansible_hostname": "ubuntuVBX2, 
"ansible_hostname": "ubuntuVBX", 

## 5 sla het resulterende bestand op in de map van het sjabloon template,  idealiter met de extensie .j2. 

**rollen / nginx / templates / default.conf.j2 **

Voor rollen / nginx / templates / default.conf.j2 wordt het resulterende bestand bijvoorbeeld
rollen / nginx / templates / default.conf.j2 
 
 ````
 server {
    listen {{nginx_port}}; 
    servername {{ansible_hostname}}; 
 
 location / {
   root {{nginx_root}}; 
    index {{nginx_index}}; 
   }
  } 
 ````
 
# 6 en sla de normale standaardwaarden op als volgt: ---
create roles / nginx / defaults / main.yml 
 

#file:

**rollen / nginx / defaults / main.yml **
````
nginx_port: 80 
nginx_root: / usr / share / nginx / html 
nginx_index: index.html
````

Zodra de sjabloon is gemaakt, wijzigt u de taak in het configure.yml-bestand om de sjabloon te gebruiken in plaats van de kopieermodule: 

![tree](https://github.com/boschpeter/ansible/blob/master/pictures/code_tree_nginx_role.PNG)

![templating](https://github.com/boschpeter/ansible/blob/master/pictures/role_nginx_task_configuration.PNG)
ten slotte is het tijd om eventueel het statische bestand te verwijderen dat we eerder met de kopieermodule hebben gebruikt

![j2_templating](https://github.com/boschpeter/ansible/blob/master/pictures/A0DEB38D-DAF9-45ED-ACBF-EE2418E57D43.jpeg)

#  aanbevelingen voor de beste praktijken bij het gebruik van variabelen: 

begin met standaardwaarden in een rol. Dit heeft de laagste prioriteit van allemaal. Dit is ook een goede plek om de normale standaardwaarden van uw toepassing te bieden, die later op verschillende plaatsen kunnen worden overschreven. Groepsvariabelen zijn erg handig. Vaak zullen we regiospecifieke of omgevingsspecifieke configuraties uitvoeren. We zouden ook bepaalde rollen toepassen op een bepaalde groep servers, bijvoorbeeld voor alle webservers in Azië passen we de Nginx-rol toe. Er is ook een standaardgroep met de naam "alle", die alle hosts voor alle groepen zal bevatten. Het is een goede gewoonte om de variabelen die voor alle groepen gebruikelijk zijn in "alle" (group_vars / all) te plaatsen, die dan door meer specifieke groepen kunnen worden overschreven. Als er host-specifieke uitzonderingen zijn, gebruik dan hosts_vars, bijvoorbeeld host_vars / specialhost. example.org.Als u variabelen in verschillende bestanden wilt scheiden, maakt u mappen met de naam van de hosts en plaatst u de variabele bestanden erin. Alle bestanden in die mappen worden geëvalueerd: group_vars / asia / webhost_vars / specialhost / nginxhost_vars / specialhost / mysqlAls u uw rollen generiek en deelbaar wilt houden, gebruikt u standaardwaarden in de rollen en geeft u vervolgens organisatiespecifieke variabelen op uit playbooks. Deze kunnen worden gespecificeerd als rolparameters. Als u wilt dat rolvariabelen altijd voorrang hebben op voorraadvariabelen en playbooks, geef ze dan op in de vars-directory binnen een rol. Dit is handig voor het leveren van rolconstanten voor specifieke platforms. Ten slotte, als u een van de voorgaande variabelen wilt overschrijven en wat gegevens wilt opgeven tijdens runtime, biedt u een extra variabele met Ansible-opdrachten met de optie -e.

![variable_precedence](https://github.com/boschpeter/ansible/blob/master/pictures/variabe_precedence.PNG)
