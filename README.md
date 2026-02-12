
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
### Objectif principal

Garantir que le système qui démarre est authentique, signé et non modifié par un acteur malveillant.

### Chain of Trust 

La chain of trust est une série de vérifications où chaque composant vérifie l’authenticité du suivant avant de l’exécuter.
Chaque maillon valide le suivant, du bootloader jusqu’au système Android.

### Pourquoi l’intégrité au démarrage est critique ?

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

Matrice de Risques et Mesures Défensives — TP Rooting Android
## Matrice de Risques 

Intégrité non garantie → Conclusions biaisées sur la sécurité réelle

Surface d’attaque accrue si l’appareil sort du labo → Exposition à des menaces externes

Données sensibles exposées si présentes → Violation potentielle de confidentialité

Instabilité système → Tests non reproductibles et résultats incohérents

Mélange comptes perso/test → Fuite possible d’informations personnelles

Mauvais nettoyage fin de séance → Persistance de données sensibles

Réseau non isolé → Effets involontaires sur systèmes externes

Traçabilité insuffisante → Impossible de reproduire ou d’auditer les tests

Principe de sécurité : Chaque risque identifié doit être associé à une mesure d’atténuation correspondante. C’est le principe de la gestion des risques en cybersécurité.

## Mesures Défensives

Réseau isolé pour éviter toute communication non contrôlée

Données fictives uniquement pour éliminer tout risque de fuite réelle

Device / AVD dédié exclusivement aux tests de sécurité

Snapshots ou wipe en fin de séance pour ne laisser aucune trace

Journal de configuration détaillé pour assurer la reproductibilité

Aucun compte personnel pour éviter tout mélange de données

Contrôle strict des APK installées pour limiter les risques

Horodatage + captures des étapes pour une traçabilité complète
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
<img width="1295" height="825" alt="Screenshot 2026-02-11 192734" src="https://github.com/user-attachments/assets/ca3ce5f6-d37f-4101-a906-d9af4306dc21" />


## Analyse des logs via :

adb logcat | findstr diva
<img width="1876" height="425" alt="Screenshot 2026-02-12 184444" src="https://github.com/user-attachments/assets/561ff75f-a8e7-4d60-a0fb-929712ddb8ca" />

## Analyse des logs (Logcat)
### Objectif

Vérifier si l’application écrit des informations sensibles dans les logs système.

### Commande utilisée
adb logcat
<img width="1702" height="632" alt="image" src="https://github.com/user-attachments/assets/402c595f-db36-4fc6-88d3-135bc7e3cf9b" />

## Traçabilité
Scénarios testés (3) :

-Ouvrir écran d’accueil

-Rechercher un item

-Ouvrir fiche produit/profil

Observations factuelles :

Root actif : uid=0

Verified Boot : Yellow

Partition /system montée en lecture/écriture

Accès aux logs via logcat

Données sensibles dans shared_prefs 
Limites :

Application simplifiée de test

Environnement isolé

Données fictives uniquement
Reset effectué : Oui 

## Rooting Android — Livrables
### Définition Rooting 

Le rooting correspond à l’obtention des privilèges super-utilisateur (root) sur Android.
Il permet d’accéder aux partitions système normalement protégées et de modifier le système à bas niveau.
En laboratoire, il permet d’observer des comportements internes des applications et de tester la sécurité.
Cependant, le rooting est risqué et nécessite un environnement isolé, traçable et réinitialisable.

### Schéma simplifié Verified Boot / AVB
ROM signée
   ↓
Bootloader vérifié
   ↓
Kernel
   ↓
Système Android
   ↓
Applications sandboxées

AVB : vérification moderne + anti-rollback


Explication : chaque étape vérifie l’intégrité de la suivante avant exécution → "chaîne de confiance".


### Risques	Mesures défensives
Intégrité non garantie	Réseau isolé pour éviter toute communication non contrôlée
Surface d’attaque accrue	Données fictives uniquement
Données sensibles exposées	Device/AVD dédié aux tests de sécurité
Instabilité système	Snapshots ou wipe en fin de séance
Mélange comptes perso/test	Journal de configuration détaillé
Mauvais nettoyage	Aucun compte personnel utilisé
Réseau non isolé	Contrôle strict des APK installées
Traçabilité insuffisante	Horodatage + captures étapes pour traçabilité complète
### MASVS — 2 exigences

STORAGE-1 : Les données sensibles (mot de passe, token, API key) doivent être stockées de façon sécurisée (chiffrement recommandé).

NETWORK-1 : Toutes les communications réseau doivent être sécurisées via TLS avec validation correcte des certificats.



 ### Fiche environnement
Support : AVD Android Emulator
Version Android / API : API 24
Application + version : app-debug.apk v1.0

Scénarios testés :

Ouvrir écran d’accueil

Rechercher un item

Ouvrir fiche produit/profil

Observations factuelles :

Root actif : uid=0

Verified Boot : Yellow

Partition /system montée en lecture/écriture

Accès aux logs via logcat

Données sensibles dans shared_prefs (exemple fictif)

Limites : application simplifiée, environnement isolé, données fictives
Reset effectué : Oui 


adb emu avd wipe-data

<img width="892" height="576" alt="Screenshot 2026-02-12 184646" src="https://github.com/user-attachments/assets/f98fd078-dc4e-4b6a-96d2-39dc605b5fa1" />
Rooting Android — Livrables



 ## Checklist Finale 
### Début de séance (préparation)

 Périmètre écrit et défini

 AVD neuf / propre

 Application de test installée

 ### scénarios notés et documentés

 Versions Android / Application notées

### Fin de séance (nettoyage / traçabilité)

 Données de test supprimées

 Reset effectué (Wipe AVD ou Reset device)

 Preuve du reset (captures écran / commandes)

 Rapport + traçabilité sauvegardés

 Aucun compte personnel utilisé

### Méthodologie professionnelle

Cette checklist suit le principe PDCA (Plan – Do – Check – Act) :

Plan : définir le périmètre et préparer l’environnement

Do : exécuter les tests de façon contrôlée

Check : vérifier les résultats et documenter

Act : nettoyer, réinitialiser, et sauvegarder la traçabilité
