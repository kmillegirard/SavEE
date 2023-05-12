# SavEE
Codes des différentes parties du prototype du projet SavEE, dans le cadre des I-NOVGAMES

# Introduction
Pour répondre à la problèmatique : Comment l'IoT contribue à un monde plus durable ?, l'équipe de l'école ISEN Méditerranée présente son projet SavEE. Save, Eau, Electricité. 

Les étudiants ont décidé de s'intéresser à la consommation d'électricité dans un foyer ainsi qu'à la détection d'anomalies de consommation d'eau. Le projet se découpe en 3 parties. 

# IA pour la désintégration
Le but de cette partie est de pouvoir reconnaître la signature électrique de différents équipements pour en faire de la classification à partir des données de consommation fournir par le compteur LINKY à travers l'ERL. 

Ces données seront récupérées en décimale pour être traités par Nanoedge Studio AI en amont (le fonctionnement de chaque équipement pour en aval reconnaître les équipements qui fonctionnent). Et pour aller plus loin, on fera de l'anomalie détection pour reconnaître aussi les anomalies liées à l'usage de chaque équipement pour une maintenance prédictive. 

# Détection d'anomalies
L'objectif de cette partie est de détécter une anomalie de fonctionement d'appareils grâce à ses vibrations qu'on analysera avec un accelerometre (nucleo-WB55 et un iks01A3)

Nos cas d'usage sont une machine à laver ainsi qu'un tuyau.
 
Pour le tuyau on pourra détecter si il fonctione normalement ou non, c'est à dire qu'on sera capable de détecter une fuite sur le tuyau. L'objectif est de réduire les pertes d'eau.

Pour la machine, on pourra détecter un dysfonctionnement grâce aux 2 accelerometres qui y seront placés: un sur la machine et un sur le tuyau d'arrivée d'eau de la machine (voir photo). L'objectif est de réduire les pertes d'eau et les consomations d'électricité.


Pour permettre ceci nous avons collectés des datas de nos 2 cas d'usage: la machine et de son tuyau ainsi que de notre prototype de tuyau avec une fuite. Nous avons utilisé une nucleoL152RE ainsi que le logiciel unicleo-GUI pour collecter ces datas(vibrations sur 3 dimensions). Nous avons collectés plusieurs classes de datas: lors de fonctionement normal ansi que lors de fonctionement avec anomalie.

Ces datas ont ensuite été traitées par une IA: nanoedgeAI qui nous a donné des librairies associés. Ces librairies sont ensuite utilisées avec un code et une nucleo-WB55 afin de pouvoir détecter ces anomalies.

# TIC made in SavEE
Afin d'exploiter les données du Linky selon notre volonté, il est nécessaire d'accéder aux données brutes. Pour atteindre cet objectif, nous utilisons un schéma éléctronique simple nécessitant un optocoupleur et deux résistances. Nous récupérons ainsi les données clients. 

Ensuite, tout est traité en UART via un microcontroleur. Notre code traite aussi les données afin de garder seulement ce qui est utile à nos yeux (date + heure + consommation totale). Les données utiles sont stockées dans une carte SD puis le mobile peut y accéder lorsqu'il se connecte via BLE.

# Connectivité
Dans la continuité de maîtriser la consommation d'énergie, nous avons choisi comme technologie de communication sans fil le BLE. Il communique à faible consommation. Un problème lié à ce choix est qu'il faut que le mobile soit connecté à la carte STM32. Pour régler celà, nous avons implémenté un module de carte SD. A terme, nous souhaiterions utilisé la wifi.

Pour utiliser la carte SD avec le microcontrôleur STM32 via le bus SPI, en connectant les broces MOSI, MISO, SCK, CS de la carte SD (plus de détails /Schéma).

En cliquant sur ce lien, vous aureze accès à une briève présentation des possibles échanges que nous avons voulu rendre fonctionnels.
https://www.canva.com/design/DAFhGJYLjHA/FmaJW0r7sD-M4_MSMO0pXw/view?utm_content=DAFhGJYLjHA&utm_campaign=designshare&utm_medium=link&utm_source=publishsharelink   

# Application mobile
Présentation de l'application mobile et ses différentes parties :

- Page de connexion 
identifiant existant pour se connecter : 

    Identifiant : smartenergy@isen.fr
    
    Mot de passe : 12345678

- Page de création de compte 
Ici on crée son compte en mettant ses informations (mail, numero de compte elec, et mot de passe)
NB : il faut un mot de passe de 8 caractères minimum 

- Menu principal
Arrivé sur le menu , on accède à toute les fonctionnalités de notre app 

- Page "Ma consommation" 
On accède à un graphe de la consommation électrique de l'utilisateu

- Page "Bilan Carbone"
Ici on accède au taux d'emission de CO2 des machines que l'utilisateur possède dans son domicile 

- Page "Détection d'anomalies"
Sur cette page nous avons un boutton "détection" qui recherche une anomalie et qui envoie une notification d'alerte à l'utilisateur en cas de possible problème 

- Page "Maintenance predictive" 
Sur cette page , nous avons un boutton switch qui est soit en position "ok" ou "ko" et qui renvoit l'état de la machine 

- Page "Aide" 
Sur la page aide ,vous avez accès à nos adresses mails pour nous contacter possiblement 

- Page "BLE" 

En entrant dans la page BLE , on a un boutton "Se connecter au bluetooth". 
En clickant sur celui-ci, l'application nous requiert d'activer le bluetooth de l'appareil et de valider les permissions qui vont avec. En faisant cela, le BLE se met alors en marche. 
Un boutton "PLAY" apparait alors et en appuyant sur celui-ci, l'appareil scan les périphériques bluetooth se trouvant à proximité. Sur l'écran est alors affiché le nom des appareils se trouvant dans les alentours et leurs adresses correspondantes.
Pour notre projet, on se connecte alors à la carte STM32 avec le BLE du nom SAVEE
Ensuite on accède à plusieurs informations à savoir : 
   
   - l'accès générique

   - l'attribut générique

   - le service spécifique

   - le nom du périphérique

   - l'apparance

   - les custom characteristic 1, 2, 3, 4 qui est ce qui nous interesse ici 

Ici, on va pouvoir: 
1.  écrire en mode "write" dans la 1ère caractéristique

2.  ensuite lire en mode "read" la valeur précedemment écrite dans la 2e caractéristique 

3.  Puis dans la 3e caractéristique, le "notify" qui va demarrer un timer pour lire une heure coder en dure  (attention elle est en hexadécimale)
