**Ajouter un proxy**

@see http://stackoverflow.com/questions/7559648/is-there-a-way-to-make-npm-install-the-command-to-work-behind-proxy

````
npm config set proxy http://10.154.61.3:3128
npm config set https-proxy http://10.154.61.3:3128
npm config set strict-ssl false
npm config set registry "http://registry.npmjs.org/"
````

Remarque : notez bien que le https-proxy pointe vers une url en http et pas https



