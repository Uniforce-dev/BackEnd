# BackEnd Symfony
Partie Back avec API et Base de données


Pré requis : 
 - Avoir un mysql de lancé (Wamp, Xamp, Docker, etc...) 
 - Avoir Composer d'installé
 - Installer le CLI de Symfony : https://symfony.com/download 
 
Ensuite vous pouvez cloner le projet dans le dossier de votre choix. 

Une fois le projet récupéré, et dans le dossier du projet, vous pouvez lancer le serveur avec la commande : ```symfony serve -d --no-tls ``` (le -d permet de détacher la commande et ne pas être obliger de garder la fenêtre du CLI ouverte ; et la commande --no-tls permet de désactiver l'encryption tls)

Avec le serveur lancé, vous aurez accès à l'API sur l'adresse : localhost:8000/api 

Pensez à verifier le port de mysql dans l'adresse de connexion à la BDD (fichier .env) 
