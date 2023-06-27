#   DEPLOIEMENT AVEC FOG

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

📂 Host managemen
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

