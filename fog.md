#   DEPLOIEMENT AVEC FOG

## Schéma réseau

![ScreenShot_20230627160024](https://github.com/baaldees/Projet_Fog/assets/97484980/d67cc80c-0f95-4ab5-8455-76a6eec5dec9)


## Configuration des machines virtuels

### PFSENSE

![pfsense](https://github.com/baaldees/Projet_Fog/assets/97484980/6879d5bb-f537-4ae4-afee-bf8d8c9bc2bd)

### SERVEUR FOG

![ScreenShot_20230627100750](https://github.com/baaldees/Projet_Fog/assets/97484980/a890fc18-9ef2-4297-a3af-ad0992700af5)


### INSTALLATION DE FOG

Pour commencer, il faut télécharger l'archivecontenant les fichiers du serveur sur notre Debian

Pour cela en root tapez

    `cd /root`

    `wget https://github.com/FOGProject/fogproject/archive/1.5.10.tar.gz`

**Attention le lien de téléchargement peut être obtenu en allant sur la page :**

[http://fogproject.org/](http://fogproject.org/ "http://fogproject.org (nouvelle fenêtre)")

puis en cliquant sur le bouton Download, vous aurez le lien en faisant un clic droit sur le lien "TAR.GZ"

Ensuite décompressez l'archive

    `sudo -i`

    `tar -xvzf 1.5.10.tar.gz`

Ensuite allez dans le dossier bin du répertoire d'extraction

    `cd fogproject-1.5.9/bin`

listez le contenu du dossier

Ensuite lancez le script d'installation de fog
  
    `./installfog.sh`

vous obtiendrez l'écran suivant

![ScreenShot_20230627103306](https://github.com/baaldees/Projet_Fog/assets/97484980/77da0467-5727-4353-8593-7ecf3ab93708)

Pour commencer, sélectionnez votre version de linux

Puis on vous demandera confirmation sur différents paramètres :

* Type d'installation (ici c'est un serveur normal donc N)
  

* IP du serveur Fog
  
* Changement de l'interface réseau ens33 : N
  
* IP du routeur pour le serveur DHCP
  
* IP du DNS pour le serveur DHCP
  
* Le nom de la carte réseau affectée à FOG
  
* FOG sera t'il utilisé comme serveur DHCP (répondre oui car pas de DHCP sur le réseau, sinon il va falloir indiquer les chemins de boot des images réseau sur votre serveur DHCP)
  
* Installation de langage pack supplémentaires
  
* Activation de HTTPS
  
* Choix du hostname
  

A la fin vous aurez un récapitulatif de la configuration à valider.

![ScreenShot_20230627110653](https://github.com/baaldees/Projet_Fog/assets/97484980/fdcabec7-52df-4980-8e65-cc156d448e71)

![ScreenShot_20230627110807](https://github.com/baaldees/Projet_Fog/assets/97484980/ce9d9d54-1676-40f6-97b8-0df10075bfd9)


Pour accéder à FOG, je me connecte via http://192.168.1.2/fog
Login = fog
Mot de passe = password

### Désinstaller FOG

En cas d'erreur, ou de besoin de reconfiguration.

      `mv /opt/fog/.fogsettings /opt/fog/fogsettings-firstInstall`


### Enregistrement et inventaire d'une VM

Pour pouvoir ce connecter en PXE, il faut deactiver le secure boot de la VM sur l'ESXi.

Il faut aller dans:

📂 Actions > Modifier les paramètres > Options VM > Options de démarrage
  - Décocher activer le démarrage sécurisé UEFI
  - Décocher activer le démarrage sécurisé UEFI

![ScreenShot_20230627120514](https://github.com/baaldees/Projet_Fog/assets/97484980/f95d8580-2b60-4963-964f-dcb32e94b671)

### Enregistrement et inventaire d'un poste de travail

il faut démarrer le windows_maitre en PXE

![ScreenShot_20230627121430](https://github.com/baaldees/Projet_Fog/assets/97484980/c9fc69cd-b4bf-47a3-8131-398e9705e182)

Sur la page d'enregistrement FOG, sélectionnez "Quick Registration and Inventory" 

![ScreenShot_20230627133904](https://github.com/baaldees/Projet_Fog/assets/97484980/1d365fe4-ea97-41f1-8e2b-44ed5a635677)

Vous pouvez vérifier l'enregistrement dans l'interface web de fog dans la partie "host management"

Cliquez sur "list all hosts", vous pouvez voir le poste qui est bien présent dans la liste

![ScreenShot_20230627134049](https://github.com/baaldees/Projet_Fog/assets/97484980/caa242bf-2484-45f1-957b-f6c2ad9a8d4c)

En cliquant dessus, vous pouvez changer le nom du poste.

![ScreenShot_20230627134135](https://github.com/baaldees/Projet_Fog/assets/97484980/41afe09a-bc05-42e8-bad7-a5598e3ecf67)

### Capture d'une image

Pour capturer l'image d'un poste de travail, il faut tout d'abord créer le container sur FOG.

Cliquez sur :

📂image management"
Create new image
❗pour Windows10, choisir l'option 2 pour l'image type

![ScreenShot_20230627134433](https://github.com/baaldees/Projet_Fog/assets/97484980/0c00404d-bbd3-48de-9b32-2d2906d3929e)

Créer ensuite la tâche de capture de l'hôte :

📂 Host management
List all hosts
cliquez sur le poste à capturer
affectez un container à host image

![ScreenShot_20230627134742](https://github.com/baaldees/Projet_Fog/assets/97484980/67b8a9d8-3b23-4724-8dd4-1ce81ead9174)


Maintenant il nous reste a lui attribuer une tâche de capture (Upload), cliquez sur "Basic Task" dans le menu du haut.

Puis sur "capture"

![ScreenShot_20230627134844](https://github.com/baaldees/Projet_Fog/assets/97484980/7270abd1-8159-4c3c-9151-a82edb81b6f7)


La tâche est bien créée

![ScreenShot_20230627134941](https://github.com/baaldees/Projet_Fog/assets/97484980/dc432099-aa09-40ab-ba38-59b19ceda5d0)

Installe l'image en PXE

![ScreenShot_20230627135152](https://github.com/baaldees/Projet_Fog/assets/97484980/e55c4493-c946-4b06-ab8d-3527888327ce)


Je peux voir son avancement dans le menu "Task management"

![ScreenShot_20230627135237](https://github.com/baaldees/Projet_Fog/assets/97484980/9498a6ba-1b47-47c0-84fd-22f3a6df9f7e)


### Déploiement d'une image

Pour créer une tâche de déploiement cliquez dans le "Host management" sur "list all hosts"

Puis cliquez sur le poste de travail a déployer,


![ScreenShot_20230627135601](https://github.com/baaldees/Projet_Fog/assets/97484980/24513589-b532-4633-aee3-31c0c27ad7a9)

Basic task, Deploy, Create deploy task for ...

![ScreenShot_20230627135639](https://github.com/baaldees/Projet_Fog/assets/97484980/a733580f-9678-4f0e-a0f9-d5cb5dfee190)

Je relance le poste à déployer en le faisant booter en PXE :

![ScreenShot_20230627160202](https://github.com/baaldees/Projet_Fog/assets/97484980/90ad2243-3b39-4472-82f1-0370af8ba862)

Je peux ensuite suivre son avancement sur le serveur :

![ScreenShot_20230627160255](https://github.com/baaldees/Projet_Fog/assets/97484980/d8a643c9-8238-4505-a27d-5681b24924d2)

### Installation - Agent FOG
 
Je télécharge le client FOG en bas à gauche de l'interface web :

![ScreenShot_20230627160355](https://github.com/baaldees/Projet_Fog/assets/97484980/b4c64a24-dc94-4bb5-b302-079c67fba471)


Puis, cliquez sur "New client and Utilities"

![ScreenShot_20230627160421](https://github.com/baaldees/Projet_Fog/assets/97484980/6bb474c9-a877-4597-9a60-2bc3d01597b9)


choix entre deux options :

- MSI - Network Deployment (pour un déploiement réseau, notamment via GPO)
- Smart installer (pour une installation sur tout type d'OS)


Choisir le Smart Installer.
Téléchargez et installez sur la machine cliente. Pendant l'installation, notez l'adresse IP de mon serveur FOG pour lier le poste au serveur.

![ScreenShot_20230627160726](https://github.com/baaldees/Projet_Fog/assets/97484980/fb393fde-9844-4a8e-9c6e-073ba2572d4a)

On peut ensuite accéder aux fichiers de log de FOG à la racine du C:\

### Déploiement de logiciel

📂 Snapin
Snapin management
Create new snapin

On peux :
- nommer le snapin
- commenter
- uploader le fichier
- spécifier les arguments de l'installation silencieuse
- choisir le template de déploiement de scripts / logiciels

Dans le Batch script, et j'ajoute l'argument /S pour l'installation silencieuse et /L=1036 la langue .

![ScreenShot_20230627161121](https://github.com/baaldees/Projet_Fog/assets/97484980/e1ffef42-1e26-4135-b240-622611030bf7)

Dans Membership et j'ajoute l'hôte sur lequel je souhaite déployer le snapin :

![ScreenShot_20230627161158](https://github.com/baaldees/Projet_Fog/assets/97484980/1a2b06f0-4a04-4786-b053-0a13aa6bd741)


📂 cliquez sur l'hôte à sélectionner:

Basic task,
Advanced,
Single snapin,

L'installation a bien été prise en compte par ma machine Master

![ScreenShot_20230627161402](https://github.com/baaldees/Projet_Fog/assets/97484980/0972cd19-b145-4af6-b5c2-fcd12dc41a99)


Je le vois aussi apparaitre dans le fichier de log de fog :


![ScreenShot_20230627161442](https://github.com/baaldees/Projet_Fog/assets/97484980/b52da4af-5c28-48f3-b097-3c1d8cd0ffc3)


### Changer le nom d'un poste de travail

Il n'est pas nécessaire d'avoir l'agent installé sur le client.

📂Service configuration
Hostname changer
cocher la case

![ScreenShot_20230627161527](https://github.com/baaldees/Projet_Fog/assets/97484980/2e1a83fb-4669-4e05-9529-bf131458be31)

Aprés avoir changer le nom du poste, celui-ci redémarre automatiquement pour prendre en compte la mise à jour.

Pour que le poste client puisse redémarrer alors que l'utilisateur est logué, il faut cocher
l'option "Name change / AD force reboot" dans le menu active directory, sinon le renommage
se fera au prochain démarrage du poste.

### Déploiement de script

📂 Snapin
Snapin management
Create new snapin


ON peux :

- nommer le snapin
- commenter
- uploader le fichier
- spécifier les arguments de l'installation silencieuse
- choisir le template de déploiement de scripts / logiciels

![ScreenShot_20230627162209](https://github.com/baaldees/Projet_Fog/assets/97484980/71e10ee3-7bd1-4c96-9b3e-2f4b4e8008e7)

Cela a bien fonctionné sur le poste client :

![ScreenShot_20230627162234](https://github.com/baaldees/Projet_Fog/assets/97484980/2b6f0588-9c5e-435b-bd3c-07f651fcd57b)


### Plugin

📂 FOG configuration
FOG settings
Plugin system

cocher "pluginsys enabled"

Un nouveau menu "Plugin" est apparu.

📂 choisir le plugin souhaité "PushBullet"
Cliquez dessus pour l'activer
Install plugins
Cliquez sur "pushbullet"
Install
Le plugin apparait dans le menu

Après la connexion à mon compte et l'installation de l'application, j'ai accès aux notifications.


![ScreenShot_20230627162403](https://github.com/baaldees/Projet_Fog/assets/97484980/3f6c99ad-3972-4520-8286-be22b0b6d9f6)

### Installation Office via ODT

### créer un partage réseau



        `cd /opt/fog`

        `mkdir OfficeDeployment`

        `chmod 777 OfficeDeployment`

![ScreenShot_20230627164842](https://github.com/baaldees/Projet_Fog/assets/97484980/5081e019-6d45-4fa5-9318-4cc4fb56f282)

        `apt install samba`

        `nano /etc/samba/smb.conf`


### ajouter les lignes suivantes à la fin du fichier smb.conf

[OfficeDeployment]

path = /opt/fog/OfficeDeployment

browsable = yes

writable = yes

guest ok = yes

read only = no

### redémarrer le service

systemctl restart smdb

On peut accéder à ce partage par le chemin \\192.168.1.253\OfficeDeployment
J'y copie le fichier Office365.xml

![ScreenShot_20230627165447](https://github.com/baaldees/Projet_Fog/assets/97484980/f005fc6b-e39f-4b12-b5ce-995127dd315c)


### création d'une tâche sous FOG

![ScreenShot_20230627165532](https://github.com/baaldees/Projet_Fog/assets/97484980/24e03df6-2389-4abf-970a-558146185dea)


![ScreenShot_20230627165547](https://github.com/baaldees/Projet_Fog/assets/97484980/f1c72b79-a701-451c-9334-747717961a10)


Lancement du déploiement, et tout est ok :

![ScreenShot_20230627165628](https://github.com/baaldees/Projet_Fog/assets/97484980/cc5b1438-a805-4946-9d0b-c0b5a803c4c0)


### Serveur FOG - Nœud distant

Configuration du deuxième serveur FOG :

![ScreenShot_20230627162707](https://github.com/baaldees/Projet_Fog/assets/97484980/1fe89cec-1d3a-4c90-a08e-11f71f68bb79)


Mon serveur est bien installé :

![ScreenShot_20230627162738](https://github.com/baaldees/Projet_Fog/assets/97484980/497f3298-791a-4616-a54a-01d0870a1720)


On peux ensuite y accéder via le dashboard si j'active l'option "graphique autorisée"

![ScreenShot_20230627162815](https://github.com/baaldees/Projet_Fog/assets/97484980/a6661617-a845-47b7-818b-a287edf61bc7)











    
