# Ionic installation :

- Installer NodeJS
	- Ajouter le chemin C:\Users\XXX\AppData\Roaming\npm au variable d'environnement PATH ( XXX = nom d'utilisateur windows)
- Lancer le cmd en administrateur
- Vérifier que NodeJs est bien installé en ouvrant un terminal et lançant node --version et npm --version
- Installer ionic en lançant la commande suivante dans le terminal 
	>npm install -g @ionic/cli

# Créer une application

- Ouvrir le cmd en Administrateur
- Se déplacer dans le répertoire voulu avec cd
- Créer l'application en lançant la commande suivante dans le cmd
	> ionic start --type=angular nomApplication blank
- Répondre N lorsque le terminal demande d'installer le Capacitor ( si vous l'installer cela prend beaucoup de temps)
	
# (Option 1) Configuration pour l'application sur android
<details>
<summary>
<a><em class="fas fa-chevron-circle-down"></em>&nbsp;&nbsp;Cliquez pour agrandir</a>    
</summary>

- Installer JAVA JDK 8
	- Ajouter le chemin C:\Program Files\Java\jdk1.8.0_241\bin au variable d'environnement PATH

##### #Installation de sdk-android command line tools
- Télécharger le sdk-android (command line tools)
- Créer un répertoire Android dans C:\
- Créer un répertoire android-sdk dans C:\Android\
- Dézipper le contenu du zip command line (tools) dans C:\Android\android-sdk\

- Créer la variable d'environnement : ANDROID_HOME avec comme valeur : C:\Android\android-sdk\
- Ajouter les chemins suivant dans la variable PATH
					C:\Android\android-sdk\tools 
					C:\Android\android-sdk\platform-tools

##### L'étape suivante doit être fait si un dossier _ existe dans lib sinon ignorer le
- Aller dans C:\Android\android-sdk\tools\lib\\_ s'il existe et Copier/Coller le contenu du dossier C:\Android\android-sdk\tools\lib\\_ dans C:\Android\android-sdk\tools\lib

##### Si vous avez ignorer l'étape précédente car le dossier n'existe pas alors reprenez à partir d'ici
- Ouvrir le cmd dans C:\Android\android-sdk\tools\bin
- Lancer les deux commandes suivantes 
	> sdkmanager.bat --sdk_root=%ANDROID_HOME% "platform-tools" "platforms;android-28" "build-tools;29.0.2"
		
	> sdkmanager.bat --sdk_root=%ANDROID_HOME% --update
- Créer un dossier avd dans C:\Users\XXX\\.android\  --> XXX : nom d'utilisateur
--> C:\Users\XXX\.android\avd
			
##### #Installation de Gradle		
- Télécharger Gradle binary-only
- Créér un répertoire Gradle dans C:\
- Dézipper le dossier gradle-6.3 de gradle-6.3-bin.zip dans C:\Gradle\

- Ajouter le chemin suivant dans la variable PATH
	> C:\Gradle\gradle-6.3\bin 
- Si le chemin de votre projet contient des caractères non ASCII il faut ajouter la ligne suivante dans votre fichier gradle.properties situé dans nomApplication\platforms\android\gradle.properties
	> android.overridePathCheck=true

##### #Ajout des prérequis npm
- Lancer les commandes suivantes pour installer les dépendances :
```
 npm i -g cordova
 npm i -g native-run
 npm i @babel/compat-data@7.8.0
```
- Fermer le CMD (important) puis le relancer si vous faîtes les étapes suivantes

</details>

# (Option 2) Configuration pour lancer l'appli sur téléphone depuis le réseau local :
<details>
<summary>
<a><em class="fas fa-chevron-circle-down"></em>&nbsp;&nbsp;Cliquez pour agrandir</a>    
</summary>
	
### A faire pour chaque application
- Modifier le contenu du fichier nomApplication\resources\android\xml\network_security_config.xml en ajoutant la ligne contenant l'adresse IP (remplacer 192.168.1.30 par l'adresse IP de votre ordinateur)
```
<?xml version="1.0" encoding="utf-8"?>
	<network-security-config>
		<domain-config cleartextTrafficPermitted="true">
			<domain includeSubdomains="true">localhost</domain>
			<domain includeSubdomains="true">192.168.1.30</domain>
		</domain-config>
	</network-security-config>
```
- Modifier le fichier nomApplication\config.xml  
	- Modifier la ligne suivante : 
	```
	<widget id="io.ionic.starter" version="0.0.1" xmlns="http://www.w3.org/ns/widgets" xmlns:android="http://schemas.android.com/apk/res/android" xmlns:cdv="http://cordova.apache.org/ns/1.0">
	```
- Ajouter la ligne suivante comme dans le bloc en dessous (environ ligne 24)
```
<application android:usesCleartextTraffic="true" />
```
```
	<edit-config file="app/src/main/AndroidManifest.xml" mode="merge" target="/manifest/application" xmlns:android="http://schemas.android.com/apk/res/android">
            <application android:usesCleartextTraffic="true" />
            <application android:networkSecurityConfig="@xml/network_security_config" />
    </edit-config>
```

</details>

## Start App on local computer browser
- Ouvrir le cmd en Administrateur
- Se déplacer dans le projet avec la commande
	> cd nomApplication
- Puis lancer :
	> ionic serve

## Lancer son appli web sur téléphone
#### Nécessite la réalisation de [Option 1](#option-1-configuration-pour-lapplication-sur-android)
- Ouvrir le cmd en Administrateur
- Se déplacer dans le projet avec la commande
	> cd nomApplication
- Lancer les commandes suivantes pour lancer l'application
```	
 ionic cordova platform add android
 npm i cordova-res@latest --save
 ionic cordova resources android --force
 ionic cordova prepare android
```
- Connecter son téléphone android en USB à son ordinateur 
-- N'oubliez pas d'activer : Déboguage USB sur votre téléphone
### Lancer son appli web sur téléphone branché en USB
- Lancer la commande suivante pour lancer son appli sur le téléphone connecté 
	> ionic cordova run android
- Lancer la commande suivante pour être en déploiement continu
	> ionic cordova run android -l
	
### Lancer son appli web sur téléphone à distance
#### Nécessite la réalisation de [Option 1](#option-1-configuration-pour-lapplication-sur-android) + [Option 2](#option-2-configuration-pour-lancer-lappli-sur-t%C3%A9l%C3%A9phone-depuis-le-r%C3%A9seau-local-)
- Le téléphone doit être branché lors de l’exécution de la commande.
- Lancer en déploiement continu en reseau local (débranché)
	> ionic cordova run android -l --host=x.x.x.x (x.x.x.x = adresse ip de ton pc)
