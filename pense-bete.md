	#Linux #Java #Eclipse #Android #Tomcat #SQL #SOAP #Reseau #Oracle #SVN

**Afficher les fichiers/répertoires triés par date rétroactive**

```sh
ls -lrt
```

**Trouver un fichier d'un certain nom**

```sh
find / -name tnsnames.ora
```

**Trouver les fichiers contenant un certain motif (recherche récursive)**

```sh
find . -type f -print | xargs grep motif-a-trouver
```

**Trouver les fichiers contenant un certain motif (récursivement) et contenant un autre motif**

```sh
# rechercher dans tous les fichiers ayant le suffixe .txt ceux qui contiennent le mot beau
find . -print0 | grep  -z '\.txt' | xargs -0 grep 'beau'
```

**Comment supprimer tous les fichiers qui datent de plus de 30 jours ?**

```sh
find . -mtime +30 -exec rm -rf {} \;

# ou pour effacer seulement les fichiers de sauvegardes (terminés par un tilde)
find . -mtime +30 -name '*~' -print -exec rm  \;
```

**Recherche d'éléments communs entre deux fichiers**

```sh
sort liste1 > liste1.tmp
sort liste2 > liste2.tmp
comm -1 -2 liste1.tmp liste2.tmp
```

**Recherche motif par balise xml**

```sh
cat test.xml | awk 'BEGIN { FS = "</?NIR>" } { for (i=2; i<=NF;i=i+2) print $i }' | sort -n | uniq
```

**Recherche et remplacement en vi**

```sh
Search STRING forward :   / STRING.
Search STRING backward:   ? STRING.
Repeat search:   n
Repeat search in opposite direction:  N  (SHIFT-n)

Replace: Same as with sed, Replace OLD with NEW:
First occurrence on current line:      :s/OLD/NEW
Globally (all) on current line:        :s/OLD/NEW/g
Between two lines #,#:                 :#,#s/OLD/NEW/g
Every occurrence in file:              :%s/OLD/NEW/g
```

**Lancer Tomcat et afficher interactivement son fichier de log**

```sh
service tomcat7 restart && tail -f /var/log/tomcat7/catalina.log
# auparavant, on peut vouloir vider le fichier de log sans avoir à le supprimer/recréer
> /var/log/tomcat7/catalina.log
```

**Compresser/décompresser en tar.gz**

```sh
# c pour compresser, z = zip, v = verbeux, f = fichier donné en paramètre
tar czvf archive.tar.gz /mon/repertoire
# x pour décompresser
tar xzvf archive.tar.gz
# -C permet de décompresser dans un autre répertoire
tar xzvf archive.tar.gz  -C /home/toppub/autre/repertoire
```

**Connaître l'encodage d'un fichier**

```sh
file - i toto.txt
```

**Convertir l'encodage de certain fichier dans un répertoire**

```sh
# migration de ISO-8859-1 en UTF-8 de tous les fichiers Java sous le répertoire TRANSVERSE
for filename in $(find . -type f -name *.java)
do
    # sauvegarde du fichier source
    mv $filename $filename.save
    # ecriture du fichier encode
    iconv -f ISO-8859-1 -t UTF-8 $filename.save -o $filename
    # suppression éventuelle
    # rm $filename.save
done
```

**Afficher le contenu d'archives**

```sh
# affiche les droits, groupe, taille et repertoire (chemin en dur vers le rpm)
rpm -qilpv /produit/puppet-appli-conf-refg-topad-20.21-rpm.rpm

# affiche les fichiers du rpm correspondant (le rpm n'a pas besoin d'être présent)
rpm -ql tomcat55-vespa.noarch

tar -tf TOPADMAS_ASYNC-ConfDynamique.tar

# si tar.gz
tar -ztvf file.gz

# fonctionne pour jar/war/ear
jar tvf ARTEMIS_A_GESTION-IHM-9.99.war | grep socle
```

**Comment zipper en ignorant le fichier .DS_Store (Mac) ?**

```sh
zip -r foo.zip foo/ -x "*.DS_Store"
```

- -r pour descendre récursivement
- foo/ est le répertoire à zipper
- -x spécifie une liste de ressources à ignorer

**Connaitre l'origine rpm d'un fichier**

```sh
rpm -qf nomFichier
```

**Voir tous les rpms installés**

```sh
rpm -qa | grep perl
yum list | grep tomcat7
```

**Connaître les dépendances d'un fichier rpm**

```sh
rpm -qpR {.rpm-file}
rpm -qR {package-name}
```

**Groupe yum**

```sh
yum grouplist -v hidden  # Liste tous les groupes
yum grouplist -v hidden | grep desktop
yum groupinstall "KDE Plasma Workspaces"
```

**Connaitre l'état d'un rpm via Yum**

```sh
# voir les infos d'un rpm (pour connaitre sa version ou s'il est installé)
yum info puppet-appli-conf-refg-topad.noarch

# supprimer un rpm
yum erase puppet-appli-conf-refg-topad.noarch

# pour installer la dernière verison
yum install puppet-appli-conf-refg-topad.noarch

# pour installer une autre version que la dernière
yum install puppet-appli-conf-refg-topad-20.48-rpm.rpm
```

**Voir les messages du système Linux**

```sh
tail -f /var/log/messages  # affiche les messages système
```

**Puppet**

```sh
# (à lancer sur le serveur puppet appli topdmsp001). Pour voir la liste des minions d'un serveur de déploiement puppet
func "*" list_minions
```

**Puppet : avoir les variables facter des serveurs gérés par le serveur puppet**

```sh
# (à lancer sur le serveur puppet appli topdmsp001). Exemple d'appel sur l'ensemble de la plateforme topad
sudo func @topad call command run 'facter | grep instance; facter | grep artemis; facter | grep topad'

# exemple d'appel sur une machine en particulier
sudo func artdgsa002 call command run 'facter | grep instance; facter | grep artemis; facter | grep topad'
```

**Puppet : voir le socle installé sur les serveurs gérés par le serveur puppet**

```sh
# (à lancer sur le serveur puppet appli topdmsp001)
sudo func topdgsa001 call command run 'cat /master.release' | tail -1 | awk 'BEGIN{FS=","}{print substr($2, 3, length($2)-5)}'
```

**Lister les variables puppet**

```sh
# cf. /usr/lib/ruby/site_ruby/1.8/facter/dgfip.rb pour voir les variables valorisées par le projet
facter
```

**Lancer une synchronisation Puppet system**

```sh
# sur le serveur d'application
[root] # service puppetSystem verbose
```

Les variables facter sont récupérées dynamiquement via les fichiers du répertoire

- **/usr/lib/ruby/site_ruby/1.8/facter**

**Rotation de log**

Il est parfois utile de gérer la rotation de certains fichiers, comme des fichiers de log.
Cela est configurable par logrotate.

```sh
cd /etc/logrotate.d # répertoire de tous les fichiers prise en charge par logrotate
vim tomcat6
/var/log/tomcat6/catalina.out {
    copytruncate
    weekly			# 1x semaine
    rotate 52 		# garder 52 fichiers max
    compress		# compresser les fichiers quand ils sont historisés
    missingok		# ok si le fichier est manquant
    create 0644 tomcat tomcat # création d'un nouveau fichier avec des droits particuliers
}

# permet de lancer manuellement logrotate. Sinon, il est lancé par cron (/etc/cron.daily/logrotate)
logrotate -f /etc/logrotate.d/tomcat6
```

**Liste des scripts lancés par cron pour un utilisateur**

```sh
# Edition
crontab -e
# Liste
crontab -l
```

**Services en cours**

```sh
service --status-all | grep run
service --status-all | grep cours
```

**Scripts lancés au démarrage**

```sh
/etc/rc.d/init.d/
```

**Mettre son environnement bash en UTF-8**

```sh
# à ajouter à son .bash_profile
export LC_ALL=fr_FR.UTF-8
export LANG=fr_FR.UTF-8
export LANGUAGE=fr_FR.UTF-8

source ~/.bash_profile
```

**Lancer un script de façon persistante**

```sh
nohup echo "texas.sh -log" | at now &
nohup echo "./initTopad3.sh" | at now && tail -f log/out_inittTopad3.log
```

Ainsi, même en cas de control-C, le processus continuera (faire un kill du pid si besoin)

**Connaitre l'ip d'un serveur sur une interface donnée**

```sh
/sbin/ifconfig | grep -A1 eth0 | tail -n1 | sed s/adr:/' '/ | awk '{print $2}'
/sbin/ifconfig | grep -A1 eth   # pour toutes les adresses
```

**Adresse MAC**

```sh
/sbin/ifconfig eth0 | grep HWaddr | tr -s [' ''\t'] | cut -d' ' -f 5 |tr -d ':'
```

**Construire un répertoire daté d'aujourd'hui**

```sh
rep_archivage_log=${REP_LOG}/`date '+%Y%m%d%H%M%S'`
mkdir $rep_archivage_log
```

**Recherche dans l'historique SVN**

Depuis Subversion 1.8, il est possible de rechercher dans la log SVN (auteur, date, log message et liste de chemins modifiés

```sh
# rechercher sur un auteur de commit
svn log --search pomaugue

# rechercher sur une date (ici Février 2011)
svn log --search 2011-02
```
**Revenir à une révision SVN précédente**

```sh
# par exemple pour revenir à la version 53670 du répertoire commun
svn merge -r53671:53670 $SVN_URL/trunk/TOPAD2/commun
svn commit -m "retour arrière tentative UTF-8, retour à ISO-8859-1"
```

**Supprimer en SVN tous les fichiers manquants**

```sh
svn st | grep ^! | awk '{print " --force "$2}' | xargs svn rm
```

**Revenir sur tous les fichiers modifiés localement**

```sh
svn st | grep ^M | awk '{print " --force "$2}' | xargs svn revert
```

**Exécution ligne à ligne d'un script**

```sh
# afficher ligne à ligne l'exécution du service tomcat6
sh -x /etc/init.d/tomcat6 start

# il possible aussi de lancer set -x ou set -v pour visualiser les transformations (développements) du bash (puis set +x et set +v pour revenir en mode normal)
```

**Md5sum**

Pour savoir si un fichier n'est pas corrompu lors de son transfert, il est parfois utile de comparer les sommes MD5.

```sh
md5sum toto.rpm
```

En particulier utile pour comparer des fichiers qui portent le même nom mais qui viennent de versions différentes (ex : ojdbc14.jar)

 ---
**Arrêter une instance Oracle**

```sh
srvctl stop instance -d TOPAD2 -i TOPAD21 -o immediate (avec orasys ou sqlplus )
sql > shutdown immediate
```

**Démarrer une instance Oracle**

```sh
srvctl start instance -d TOPAD2 -i TOPAD21 (avec orasys ou sqlplus )
sql > shutdown immediate
sql > startup
```

**Pour se connecter sur un serveur Oracle**

```sh
[root] # su - oracle
[]oracle@topdgsd002:oracle > base TOPAD2
ADR_BASE=/u02/oracle
ADR_HOME=/u02/oracle/diag/rdbms/topad2/TOPAD2
ORACLE_BASE=/u02/oracle
ORACLE_HOME=/u01/oracle/11g/db_1
ORACLE_SID=TOPAD2
TNS_ADMIN=/u02/oracle/network/admin
[TOPAD2]oracle@topdgsd002:oracle > orasys
SQL*Plus: Release 11.2.0.3.0 Production on Wed Apr 15 17:45:20 2015

Copyright (c) 1982, 2011, Oracle.  All rights reserved.

Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.3.0 - 64bit Production
With the Partitioning and Automatic Storage Management options

SQL>
```

**Voir si la base est accessible**

```sh
lsnrctl status
```

**SQLPlus**

_Prérequis_: ajouter les exports de variables nécessaire pour l'accès (si besoin)

```sh
#confOracle.sh pour le socle 2012 v2
export ORACLE_BASE=/u01/oracle
export LD_LIBRARY_PATH=/u01/oracle/11g/client_1/lib
export PATH=/u01/oracle/11g/client_1/bin:/u01/oracle/11g/client_1/OPatch:$PATH
export ORACLE_HOME=/u01/oracle/11g/client_1
export TNS_ADMIN=/u02/oracle/network/admin

#confOracle.sh pour le socle 2007
export ORACLE_BASE=/oracle
export LD_LIBRARY_PATH=/oracle/10g/cli_1/lib
export PATH=/oracle/10g/cli_1/bin:$PATH
export ORACLE_HOME=/oracle/10g/cli_1/
export TNS_ADMIN=/oracle/10g/cli_1/network/admin

chmod u+x confOracle.sh
source confOracle.sh # ou bien à mettre dans .bash_profile
```

Ensuite se connecter sur un schéma avec un user (ex : FW_ASYNC)

```sh
sqlplus FW_ASYNC@TOPAD2_FWASYNC # exemple schéma technique
sqlplus TOPAD2@TOPAD2_CONSULT # exemple schéma fonctionnel
```

En supposant que le client lourd d'oracle existe et que le fichier tnsnames.ora définit une entrée pour TOPAD2.

Sinon :

```sh
sqlplus FW_ASYNC/FW_ASYNC@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=myhost.mydomain)(PORT=1521))(CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=TOPAD2)))
```

---
**Ouverture port pare-feu**

```sh
iptables -A INPUT -p udp --dport 1660 -j ACCEPT
```

**Où se trouve les fichiers de configuration du réseau sur Centos?**

```sh
/etc/sysconfig/network-scripts # la conf réseau
/etc/services # spécifie les ports des différents services réseau
/etc/xinet.d/ # les services à lancer à la demande par xinet
```

**Quels sont les commandes réseaux les plus utiles? **

```sh
ifconfig
netstat -a # pour voir les sockets actives
netstat -r # pour voir les routes
```

**Résolution DNS**

```sh
vim /etc/resolv.conf
ajouter les lignes suivantes
  nameserver 10.156.0.2
  nameserver 10.156.0.3
```

---

**Pour passer du JRE 5 au JRE 7**

Pour avoir la liste des jre installés sur un serveur (et pouvoir switcher sur une autre version)

```sh
alternatives --config java
```

**Lister les jvm actuellement en cours d'exécution et de récupérer leur pid**

```sh
jps					  # les jvm en cours d'exécution
jps -v                # les arguments donnés aux différentes jvm lancées
jinfo ${pid}          # info exhaustive sur l'environnement java de la jvm
jstack ${pid}         # voir la stack trace d'un processus java
jmap -histo ${pid}    # voir des informations sur le GC avec historique
jmap -heap ${pid}     # voir des informations sur le GC
jstat -gc -h 20 ${pid} 1000 # envoie les informations du GC toutes les secondes

	Les colonnes de jstat -gc
	 +-------+-------------------------------------------+
     |Column |                Description                |
     +-------+-------------------------------------------+
     |SOC    | Current survivor space 0 capacity (KB).   |
     |S1C    | Current survivor space 1 capacity (KB).   |
     |S0U    | Survivor space 0 utilization (KB).        |
     |S1U    | Survivor space 1 utilization (KB).        |
     |EC     | Current eden space capacity (KB).         |
     |EU     | Eden space utilization (KB).              |
     |OC     | Current old space capacity (KB).          |
     |OU     | Old space utilization (KB).               |
     |PC     | Current permanent space capacity (KB).    |
     |PU     | Permanent space utilization (KB).         |
     |YGC    | Number of young generation GC Events.     |
     |YGCT   | Young generation garbage collection time. |
     |FGC    | Number of full GC events.                 |
     |FGCT   | Full garbage collection time.             |
     |GCT    | Total garbage collection time.            |
     +-------+-------------------------------------------+
```

Oracle fournit [une page](http://www.oracle.com/technetwork/java/javase/matrix6-unix-137789.html) qui liste les commandes spécifiques à la JVM et au diagnostic.

Une [version plus détaillée](http://www.oracle.com/technetwork/java/javase/toc-135973.html).

**Serveur apache**

```sh
[root] # service httpd start
Démarrage de httpd :                                     [  OK  ]

# ajout de webapp
	vi /etc/httpd/conf/mod-jk.conf
# conf apache
 view /etc/httpd/conf/httpd.conf
 view /etc/httpd/conf/mod-jk.conf   => conf de demarrage
```

**Voir ponctuellement l'état de la JVM**

```sh
[root] # jmap -heap ${pid} > /var/log/tomcat5/JVM/jmap-`date +%Y-%m-%d-%H-%M-%S`.txt
```

**Suivre l'activité du GC dans Tomcat**

À ajouter dans le fichier tomcat5.conf (peu d'overhead donc il est possible de le laisser en production mais penser à purger les fichiers) :

```sh
JAVA_OPTS=$JAVA_OPTS" -verbose:gc -Xloggc:/var/log/tomcat5/JVM/JVM_GC.log.`date +%Y-%m-%d-%H-%M-%S` -XX:+PrintGCDetails -XX:+PrintGCTimeStamps"
```

Pour lire ce fichier, voici un [lien](http://scn.sap.com/people/desiree.matas/blog/2011/07/19/how-to-read-the-garbage-collector-output-for-sun-jvm) utile.

**Créer un dump de l'état de la JVM en cas de OutOfMemoryError**

Dans tomcat5.conf :

```sh
JAVA_OPTS=$JAVA_OPTS" -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/var/log/tomcat5/JVM/"
```

Remarque : à ne pas laisser en continu en production car crée un fichier de 2 Go!

Une autre façon de créer un dump est de faire

```sh
kill -3 ${pid}
```

**Cacul de la taille de la mémoire prise par le process de Tomcat**

Avec le configuration suivante dans tomcat5.conf :

```sh
JAVA_OPTS="-server -Xms1536m -Xmx1536m -XX:PermSize=64M -XX:MaxPermSize=64M"
```

La mémoire prise par Tomcat est : Heap (1536 Mo) + PermGen (64 Mo) + threads Tomcat (400 * 512 Ko = 200 Mo). À voir si les librairies JNI (SSA, oracle, etc..) s'ajoutent en plus.

**Lister les jar utilisées par une JVM**

```sh
# n'affiche que les jar réellement chargé par les JVM
lsof -p {pid} | grep jar
```

**Autre commande pour voir le classpath affecté à Tomcat**

```sh
ps -ef | grep tomcat
```

*A noter que la classe de prise de Tomcat s'appelle Bootstrap (et affiché comme tel avec ps)*

**Performance Java**

- [ce lien](http://www.fromdev.com/2008/12/debugging-java-on-unixlinux-my-favorite.html) liste différentes commandes Linux pour monitorer les performances de Java.

- [Joops](https://code.google.com/p/joops/) permet de trouver les dépendances manquantes. Avec la class Which, il peut même donner le jar utilisé pour le chargement de telle classe

- L'option -verbose:class dans la JVM, permet d'avoir dans la log chaque chargement de classe avec son nom complet et le jar d'où il provient

**Que lance la commande service tomcat5 start?**

Le script /etc/init.d/tomcat6

**Comment voir le classpath d'une JVM?**

Debugger l'objet class.getClassLoader et voir les repositoryURL

**Voir des informations de consommation de ressource de la JVM**

jconsole.exe dans le répertoire bin du JRE (p. ex. le JRE 7)

**Ajouter la gestion des composants JMX dans Eclipse**

```sh
-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.port=9004
-Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.authenticate=false
```

**Remote Debugging**

```sh
# à ajouter à tomcat5.conf
JAVA_OPTS=$JAVA_OPTS" -Xdebug -Xrunjdwp:transport=dt_socket,address=8998,server=y"
```

Dans Eclipse, Debug configuration..., créer une nouvelle entrée dans Remote Java Application.
Remplir les informations suivantes :

- nom du projet
- connection type (standard - socket attach)
- host/port du serveur d'appli

Dans l'onglet source, mettre les sources sur lesquels on voudra un debug et poser des points d'arrêt.

**Maven**

```sh
# pour avoir l'effective pom d'un module et de ses éventuels modules enfants
mvn help:effective-pom

# lancer une install dans le repo local en ignorant les tests
mvn install -Dmaven.test.skip=true
```

**Comment faire un snapshot d'un Android via adb?**

```sh
adb shell screencap -p /sdcard/screencap.png && adb pull /sdcard/screencap.png
````

**Comment faire une vidéo de l'écran d'un Android via adb?**

```sh
adb shell screenrecord /sdcard/FILENAME.mp4
```

**Comment accéder en shell à un device Android?**

```sh
# passage dans un shell (^d pour sortir)
$ adb shell 
$ run-as fr.gouv.finances.smartphone.android

# ou bien passer en shell interactif
$ adb devices

>List of devices attached
>05d3b5d000638abd	device

$ adb -s 05d3b5d000638abd shell

# lancement d'une commande en shell
$ adb shell ls -al /data/data/fr.gouv.finances.smartphone.android/files/files/11OutilsWeb.pdf

# copie un fichier pdf dans le répertoire local du mac
$ adb pull /data/data/fr.gouv.finances.smartphone.android/files/files/11OutilsWeb.pdf .
```

**Comment installer un apk en ligne de commande?**

Prérequis : le périphérique Android doit avec les options développeur activé et être en Debogage USB.

```sh
$ adb install -l impots-mobile-SMART_V2.4.0_5-IA.apk 
```

**Comment décompiler un apk / dex en .java ?**

Utilisation de https://github.com/skylot/jadx.
Dexdump existe aussi mais c'est un outil de bas niveau peu intuitif.

- récupération du zip sur https://github.com/skylot/jadx
- dézip
- cd jadx-master
- ./gradlew dist
- cd build/jadx/bin/jadx-gui/bin
- ./jadx-gui
- choisir le fichier apk


**Comment exporter un projet Git sans les fichiers .gitignore, etc.. (comme svn export)?**

```sh
git archive --format zip --output ~/mon-projet.zip master
```

**Q : SQL sur fichier csv **

[Q](https://github.com/harelba/q) est un outil permettant d'utiliser la syntaxe SQL sur un fichier csv. Grâce à cela, toutes les fonctions du SQL sont disponibles (count, order by, etc..).

	```sh
	# Récupération des code_sages sur le fichier Etape1_FN2.csv
	python q.python -H -d; "select distinct(code_sages) from C:/AtelierSODA_3620/workspaceREPRISE/REPRISE/JAVA/src/test/resources/fichiers/in/Etape1_FN2.csv"

	python q.python -H -d# "select count(*) from C:/AtelierSODA_3620/workspaceREPRISE/REPRISE/JAVA/src/test/resources/fichiers/in/Etape1_FN2.new.csv where LENGTH(code_sages) = 3"

Remarques :

- l'option -H  utilise la première ligne comme ligne de définition des colonnes
- l'option -d suivi d'un caractère permet de spécifier un séparateur (l'espace est utilisé par défaut)

**SOAP**

Pour donner une date dans un message SOAP, il faut respecter le format du type Calendar de SOAP.

Celui-ci repose sur la syntaxe suivante : yyyy-MM-ddTHH:mm:ss (notez le caractère T qui sépare le jour de l'heure).

Exemple :

```xml
<Calendar_2>2015-01-02T00:00:00</Calendar_2>
```

**SOAP UI**

Si l'on veut importer une wsdl qui contient une pièce jointe, SOAP UI fait une erreur du type :

	Error loading [file:\C:\AtelierSODA\...\SERVICE_WAR\...\wsdl\WS-ISwA.xsd]:

Cela est dû à la WSDL qui fait un import du schéma des pièces jointes.

```xml
<import namespace="http://ws-i.org/profiles/basic/1.1/xsd" schemaLocation="WS-ISwA.xsd"/>
```

La solution est de créer en local ce fichier pour que l'import se passe correctement.

Créer un fichier WS-ISwA.xsd (à déposer à l'endroit indiqué par l'erreur SOAP UI) avec le contenu suivant :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsd:schema xmlns:tns="http://ws-i.org/profiles/basic/1.1/xsd" xmlns:xsd="http://www.w3.org/2001/XMLSchema"
elementFormDefault="qualified" targetNamespace="http://ws-i.org/profiles/basic/1.1/xsd">
	<xsd:simpleType name="swaRef">
		<xsd:restriction base="xsd:anyURI"/>
	</xsd:simpleType>
</xsd:schema>
```

