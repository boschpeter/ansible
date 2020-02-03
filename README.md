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

