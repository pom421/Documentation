## Workflow

```sh
# on rapatrie sur son poste un repo git de github
git clone https://github.com/heroku/node-js-getting-started.git

cd node-js-getting-started

# permet d'ajouter un remote git nommé heroku
heroku create 

# Procfile définit la commande à lancer
cat > Procfile << EOF
web: npm run start
EOF

# ajout du Procfile au repo git général
git add .
git commit -m "ajout Procfile"
git push origin master

# push sur le remote heroku a pour effet de déployer l'app
git push heroku master 

# pour avoir un dyno web (si compte gratuit, 1 dyno max autorisé)
heroku ps:scale web=1 
# ouvre une page de navigateur
heroku open 


  
heroku logs --tail
heroku ps
```

## Troubleshooting : 

Si webpack est en devDependencies, ajouter : 

heroku config:set NPM_CONFIG_PRODUCTION=false. You should remove the webpack.NoErrorsPlugin
