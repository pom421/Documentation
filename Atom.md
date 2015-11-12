
### Proxy

Preferences > Open config folder
Prendre le fichier .atom/.apmrc (et non pas .atom/.apm/.apmrc !!!)

et mettre les informations du type : 
  
````
https-proxy = http://10.154.61.3:3128
proxy = http://10.154.61.3:3128
strict-ssl = false
````

### Autocompletion 

- JS : avec autocomplete-plus (intégré) et atom-ternjs. 

Installer le package avec Preferences > Install > atom-ternjs

Puis Packages > Atom tern js > Configure project. 
Clic sur jQuery puis Save and restart server.

### Package à tester

https://atom.io/packages/atom-pair

http://elijahmanor.com/github-atom-packages/

### Problème raccourci Atom

- sélection multiple : le raccourci Ctl-Shift-Up ou Down ne fonctionne pas car il est pris par Mission control. Allez dans Preferences système > Missions control et désactiver les raccourcis Ctl-Up ou Ctl-Down (ou bien remapper un nouveau raccourci)

#### Packages

- install jshint
- install livereload
  - Packages > Livereload > Toggle server (ou Ctl-Shift R)
  - copier l'url de livereload en bas à droite sur Livereload:numéro-du-port
  - le coller en bas dans sa page html 
- ~~navigate-indent~~ (pb de raccourci) + browserplus pour naviguer dans les fichiers
- open-in-browser pour ajouter un item de menu Open in browser
- local-server-express : pour lancer le projet dans un mini serveur web (pour test XHR)
