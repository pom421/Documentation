### React native

```sh

# installation de la dernière version de NPM

$ npm i npm@latest -g
$ npm -v
5.0.3

# installation de create-react-native-app
# Référence : https://github.com/react-community/create-react-native-app/

$ npm install -g create-react-native-app
$ mkdir -p /Users/pom/lab/react-native/
$ cd /Users/pom/lab/react-native/
$ create-react-native-app fodmap
$ cd fodmap
$ npm start

# utilisation de yarn car npm 5 n'est pas supporté (par contre, il faut Node > 5)
$ brew install yarn
$ yarn global add create-react-native-app

# on revient sur npm car erreur dtrace avec yarn
$ npm i -g npm@4.6.1

$ create-react-native-app fodmap
$ cd fodmap
$ npm run ios
```
