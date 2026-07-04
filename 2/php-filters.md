https://www.root-me.org/fr/Challenges/Web-Serveur/PHP-Filters

pour ce challenge il fallait trouver le mdp administrateur caché

pour cela on a utilisé la fonction php://filter sur la route http://challenge01.root-me.org/web-serveur/ch12/?inc=accueil.php
 php://filter/convert.base64-encode/resource=config.php
-php://filter est une fonction de php qui precise au serveur de traiter le fichier d'abord avant de l'afficher  
- convert.base64-encode permet de convertir le fichier en base64
- on va sur la route http://challenge01.root-me.org/web-serveur/ch12/?inc=php://filter/convert.base64-encode/resource=config.php
il nous affiche le code en base64
- on decode le code en base64 pour avoir le code source du fichier config.php
- dans le code source on trouve le mdp administrateur

pour se proteger de ce genre d'attaque il faut desactiver la fonction php://filter dans le fichier php.ini ou mettre en place une liste blanche des fichiers autorisés