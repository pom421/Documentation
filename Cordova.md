

### Installation

- installer git
- installer nodesj
- installer ripple-emulator : environnement pour tester une application (gestion de l'évènement deviceready, géoloc, etc..)
- Android studio : (optimisation) dans Android SDK > SDK Tools, cochez HAXM + dans AVD > Edit > Use Host GPU
- cordova create hello
- cd hello
- cordova platform add android
- (cordova run android) pour lancer sur un smartphone
- cordova platform add browser
- cordova emulate browser # lance un serveur sur le port 8000
- cordova serve # Idem. Juste pour tester rapidement (l'app cordova n'est qu'un simple navigateur mais n'est pas précis sur le rendu final natif) -> permet de tester en android, iOs, windowsPhone ou browser
- npm install -g ripple-emulator
- ripple emulate # lance un serveur sur le port 4000

### Proxies

- git config --global http.proxy http://...
- npm config set proxy http://...
- npm config set https-proxy http://...



````dos
npm init
npm install -g cordova
npm install -g ripple-emulator
````
### Divers
- telnet : Panneau de config> Programme > Telnet
  - telnet localhost 5554 -> point d'arrêt pour l'émulateur (geo, power, sms, gsm, etc..)
  - power ac off
  - power capacity 15

- ionic : angular + cordova
- ratchett: 
