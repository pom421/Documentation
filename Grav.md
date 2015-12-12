### Installation de Grav CMS sur OS X

Un Mac Book est préinstallé avec un apache et PHP. 

Allez dans le fichier /private/etc/apache2/httpd.conf pour récupérer le chemin de DocumentRoot. 
Pour OS X 10.10, c'est /Library/WebServer/Documents.

cd /Library/WebServer/Documents
ln -s /Users/user-name/Sites lab# remplacer user-name par le vrai nom et créer auparavant le répertoire Sites

vim private/etc/apache2/httpd.conf

Dans ce fichier, modifier : 
- le nom de l'utilisateur qui va lancer le process apache pour éviter les problèmes de permission
User _www devient User user-name
- décommenter les lignes 
LoadModule php5_module libexec/apache2/libphp5.so
LoadModule rewrite_module libexec/apache2/mod_rewrite.so

- Dans la configuration du répertoire DocumentRoot, modifier 
    AllowOverride None devient AllowOverride All
Ceci est nécessaire pour que le .htaccess de la distribution de Grav soit pris compte

Prendre le zip de Grav (avec l'admin) et le dézipper. 
Renommer ce fichier avec le nom que l'on veut. ATTENTION : ne pas déplacer les fichiers mais bien renommer le répertoire. 
Sinon les fichiers cachés (tel que .htacces) n'est pas pris en compte par le finder. 

