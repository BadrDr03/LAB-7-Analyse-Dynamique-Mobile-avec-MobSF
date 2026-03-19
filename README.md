# LAB-7-Analyse-Dynamique-Mobile-avec-MobSF

Objectifs du lab
Comprendre en profondeur l’analyse dynamique (runtime) d’une application Android avec MobSF (Mobile Security Framework).
Configurer un émulateur propre sans Play Store.
Installer/lancer MobSF via Docker.
Tester l’APK vulnérable DIVA (Damn Insecure and Vulnerable Android App) en dynamique : logs runtime, trafic réseau, instrumentation Frida, proxy HTTPS, etc.
Apprendre à détecter des vulnérabilités en temps réel (stockage insecure, intents, hard-coded secrets, etc.).

---

Nature de l’émulateur (AVD sans Play Store)
L’émulateur est un Android Virtual Device (AVD) créé avec Android Studio. C’est une machine virtuelle qui simule parfaitement un vrai smartphone Android sur votre PC (CPU, mémoire, écran, capteurs, etc.).
Pourquoi « sans Play Store » ?
Quand vous créez un AVD, vous avez le choix entre :
Images Google Play → Play Store + Google Services préinstallés (bloat, processus en arrière-plan, interférences réseau).
Images Android pur (ou Google APIs sans Play) → environnement clean, sans aucun service Google.
Avantages pour l’analyse de sécurité (raison pour laquelle on l’utilise ici) :
Pas de bruit de fond (Google Play Services, Firebase, etc.) → les logs et le trafic réseau sont purs.
MobSF peut appliquer son proxy HTTPS global sans conflit.
L’émulateur est non-production et facilement compatible avec le rooting/Frida (MobSF exige un émulateur rooté pour l’analyse dynamique complète).
Performances optimales sur x86_64 (votre PC).
Limite : jusqu’à Android API 30 (MobSF ne supporte pas les versions plus récentes pour l’instant car /system n’est plus writable).
MobSF lance automatiquement Frida Server, installe son certificat CA pour intercepter le HTTPS, et capture tout ce que fait l’application en temps réel.

---

Prérequis :
Android Studio (dernière version) + SDK Platform-Tools (ADB inclus).
Docker Desktop (installé et en cours d’exécution).
Git.
Minimum 8 Go RAM + processeur 64-bit.
Connexion internet (pour les premiers downloads).

---

Étape 1 : Création de l’émulateur AVD sans Play Store (10 min)
Ouvrez Android Studio → Tools → AVD Manager → Create Virtual Device.
Choisissez un téléphone (ex. : Pixel 5 ou Pixel 6).
Dans System Image :
Sélectionnez Android (ou Google APIs) SANS « Google Play ».
Choisissez API 28 à 30 (recommandé : API 29 ou 30, x86_64).
Cliquez Download si besoin → Next → Finish.
Nommez l’AVD : MobSF_DIVA_API_30 (ou similaire).
Vérification : L’image ne contient aucun Play Store. C’est exactement ce qu’on veut.

![Import OVA](https://github.com/user-attachments/assets/4177d1e4-7027-4c66-bb30-97ac8c903cc1)

---
Étape 2 : Cloner MobSF pour utiliser les scripts AVD officiels 

Ouvrez un terminal / invite de commandes et tapez :

git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git
cd Mobile-Security-Framework-MobSF

![Import OVA](https://github.com/user-attachments/assets/2a82371d-0681-466d-9e2e-bd24aeb81fb2)

---

Étape 3 : Lancement de l’émulateur avec le script MobSF (rooté & prêt pour l’analyse)

![import OVA](https://github.com/user-attachments/assets/2f36ed0f-037d-40c2-a14d-39aeba755bbf)


Dans cette étape, nous avons lancé un émulateur Android configuré pour l’analyse dynamique à l’aide de MobSF.

Le script start_avd.ps1 permet de :

Lister les AVD (Android Virtual Devices) disponibles

Démarrer un émulateur compatible avec MobSF

Préparer l’environnement pour l’analyse dynamique (root, monitoring, etc...)



