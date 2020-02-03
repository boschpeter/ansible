# ansible

Met Ansible kunt u een hele omgeving voorzien van een enkele opdracht en enkele eenvoudige SSH-opdrachten automatiseren. Automatische implementaties zijn tegenwoordig in zwang. Het lijkt steeds relevanter te worden om te weten hoe automatisering moet worden aangepakt. Ansible is een Zwitsers zakmes voor DevOps, in staat om veel krachtige automatiseringstaken uit te voeren met de flexibiliteit om zich aan te passen aan vele omgevingen en workflows. Aan de hand van voorbeelden uit de echte wereld deelt dezer repo hopelijk "de essentie" voor alle Ansible-gebruikers. Of je nu gloednieuw bent bij Ansible of op zoek bent naar een opfriscursus, de wiki  biedt de best practices die je moet weten. (dat is tenminste mijn gedachte)


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
 
 

