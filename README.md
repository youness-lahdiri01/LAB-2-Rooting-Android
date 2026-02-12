
# LAB-2-Rooting-Android


##  Objectif

Ce TP vise à étudier l'impact du rooting sur Android et à observer le comportement d'une application vulnérable (**DIVA**) dans un environnement contrôlé.  
L'objectif est de comprendre les risques liés à l'accès super-utilisateur et les mécanismes de protection du système.

---


##  Vérification ADB


adb devices
## Activation du mode root

Commandes :

adb root
adb remount
adb shell id


<img width="1919" height="1017" alt="Screenshot 2026-02-11 190706" src="https://github.com/user-attachments/assets/49ea43e9-1099-4093-b8a1-fa6067794982" />

## Définition du Rooting

Rooting = obtention des privilèges administrateur sur Android

Permet d’accéder aux partitions protégées (/system, /data)

Modifie le modèle de confiance du système

Doit être utilisé uniquement dans un environnement laboratoire

 ## Android Security 
Android repose sur plusieurs couches de sécurité.
Chaque application fonctionne dans une sandbox, ce qui l’isole des autres applications.
Le modèle de permissions contrôle l’accès aux ressources sensibles comme la caméra ou les fichiers.
Le système protège aussi son intégrité globale contre les modifications non autorisées.
Le sandboxing empêche une application malveillante d’accéder aux données d’une autre.
Ces mécanismes fonctionnent ensemble pour sécuriser l’appareil.


## Verified Boot
🔹 Objectif principal

Garantir que le système qui démarre est authentique, signé et non modifié par un acteur malveillant.

🔹 Chain of Trust 

La chain of trust est une série de vérifications où chaque composant vérifie l’authenticité du suivant avant de l’exécuter.
Chaque maillon valide le suivant, du bootloader jusqu’au système Android.

🔹 Pourquoi l’intégrité au démarrage est critique ?

Si le démarrage est compromis, toutes les protections ultérieures peuvent être contournées.
Un système compromis dès le boot peut masquer des malwares et désactiver les mécanismes de sécurité.

<img width="1911" height="1021" alt="image" src="https://github.com/user-attachments/assets/5b3ca208-2d3b-4deb-a1fa-9b2a5eb7e657" />


## AVB (Android Verified Boot)

AVB est la version moderne de Verified Boot.
Il vérifie l’intégrité cryptographique des partitions.
Il protège également contre le rollback vers des versions vulnérables.

## Intérêt du Root en Laboratoire

En laboratoire autorisé uniquement, un environnement rooté permet :

Observer des artefacts système normalement inaccessibles

Tester la robustesse du stockage

Analyser les comportements runtime avanc

## Cloner le projet DIVA depuis GitHub

Ouvre PowerShell ou Terminal et tape :

cd C:\Users\youne\Downloads
git clone https://github.com/xAltmime/diva-apk-file.git
Installer l’APK directement

adb install DivaApplication.apk
<img width="1791" height="958" alt="Screenshot 2026-02-11 191730" src="https://github.com/user-attachments/assets/acc85a81-d463-42eb-aeac-8472787e3491" />

## Observations sur DIVA

Accès au dossier privé : /data/data/jakhar.aseem.diva/

<img width="838" height="108" alt="Screenshot 2026-02-11 192452" src="https://github.com/user-attachments/assets/618fa67f-5e14-4b1f-8d2f-8aebca655d1a" />

Vérification des SharedPreferences et fichiers en clair

## Analyse des logs via :

adb logcat | findstr diva
<img width="1876" height="425" alt="Screenshot 2026-02-12 184444" src="https://github.com/user-attachments/assets/561ff75f-a8e7-4d60-a0fb-929712ddb8ca" />

## Analyse des logs (Logcat)
🎯 Objectif

Vérifier si l’application écrit des informations sensibles dans les logs système.

🔧 Commande utilisée
adb logcat
<img width="1702" height="632" alt="image" src="https://github.com/user-attachments/assets/402c595f-db36-4fc6-88d3-135bc7e3cf9b" />
![Uploading image.png…]()

## Reset 



adb emu avd wipe-data

<img width="892" height="576" alt="Screenshot 2026-02-12 184646" src="https://github.com/user-attachments/assets/f98fd078-dc4e-4b6a-96d2-39dc605b5fa1" />
