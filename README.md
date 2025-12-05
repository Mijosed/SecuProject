1. Fait (Mijosé/Hugo/Malick)
2. Fait (Hugo)
3. Fait (Hugo)
4.
5. Fait (Mijosé)
6.
7.
8.
9.
10.
11. Fait (Mijosé)

# 2. PHP Filters

## URL : https://www.root-me.org/fr/Challenges/Web-Serveur/PHP-Filters

### Les étapes de découverte de la vulnérabilité :

Utilisation du filtre PHP `php://filter/convert.base64-encode/resource=` pour lire le contenu des fichiers source en contournant l'exécution PHP.

Payload :

```

```

![Payload/Screenshot](image.png)
![Solved](chall2.solved.png)
#### Recommandations en terme de sécurité :

- Valider strictement tous les paramètres d'entrée utilisateur
- Implémenter une whitelist des propriétés modifiables par l'utilisateur

# 3. CSRF - contournement de jeton

## URL : https://www.root-me.org/fr/Challenges/Web-Client/CSRF-contournement-de-jeton

### Les étapes de découverte de la vulnérabilité :

Analyse du mécanisme de validation du token CSRF et identification d'une faille permettant de le contourner par prédiction ou réutilisation.

Payload :

# 4. CSRF where token is not tied to user session

## URL : https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-not-tied-to-user-session

### Les étapes de découverte de la vulnérabilité :

Le token CSRF n'est pas lié à la session utilisateur, permettant de réutiliser un token valide d'un autre utilisateur.

Payload :

# 5. CSRF where Referer validation depends on header being present

## URL : https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses/lab-referer-validation-depends-on-header-being-present

### Les étapes de découverte de la vulnérabilité :

La validation du Referer ne se fait que si l'en-tête est présent. En supprimant l'en-tête Referer, on peut contourner cette protection.

Payload :

# 6. JWT - Jeton révoqué

## URL : https://www.root-me.org/fr/Challenges/Web-Serveur/JWT-Jeton-revoque

### Les étapes de découverte de la vulnérabilité :

Le système ne vérifie pas correctement la révocation des tokens JWT, permettant l'utilisation de tokens révoqués.

Payload :