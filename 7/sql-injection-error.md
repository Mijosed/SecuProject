https://www.root-me.org/fr/Challenges/Web-Serveur/SQL-injection-Error

il faut utiliser sqlmap pour exploiter cette faille d'injection sql sur http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC

et ensuite utiliser req --dump pour recuperer la base de donnee complete

apres on a le mdp administrateur dans la table users

pour se proteger de ce genre d'attaque il faut desactiver l'affichage des erreurs sql (php.ini) sur le serveur et utiliser des requetes preparees avec des parametres lies