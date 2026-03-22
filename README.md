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

![Import OVA](https://github.com/user-attachments/assets/cd3efd23-163b-4b52-8b53-938b6fa7d0ee")

---

Étape 3 : Lancement de l’émulateur avec le script MobSF (rooté & prêt pour l’analyse)

![Import OVA](https://github.com/user-attachments/assets/22f80d08-1f85-46c9-8a44-810f157bd312)

Optimisation de l'interface et Validation ADB
Après le premier démarrage réussi de l'instance MobSF_Geny_API_30, nous avons ajusté la densité d'affichage (DPI) dans les paramètres Android pour une meilleure visibilité des éléments lors de l'analyse dynamique.

Validation finale de l'environnement :
L'émulateur est désormais détecté par l'hôte via ADB. Cette étape confirme que le tunnel réseau entre Genymotion (VirtualBox) et le système hôte est opérationnel.

Identifiant de l'appareil : > * Commande : adb devices

Identifiant : 192.168.56.101:5555




