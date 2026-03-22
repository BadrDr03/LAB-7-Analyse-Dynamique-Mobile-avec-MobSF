# LAB-7-Analyse-Dynamique-Mobile-avec-MobSF

Objectifs du lab
Comprendre en profondeur l’analyse dynamique (runtime) d’une application Android avec MobSF (Mobile Security Framework).
Configurer un émulateur propre sans Play Store.
Installer/lancer MobSF via Docker.
Tester l’APK vulnérable DIVA (Damn Insecure and Vulnerable Android App) en dynamique : logs runtime, trafic réseau, instrumentation Frida, proxy HTTPS, etc.
Apprendre à détecter des vulnérabilités en temps réel (stockage insecure, intents, hard-coded secrets, etc.).

---

Au lieu de l'AVD standard d'Android Studio (souvent sujet à des erreurs de corruption d'images), nous utilisons Genymotion (basé sur VirtualBox) pour sa légèreté et sa stabilité.

Configuration de l'appareil :

Modèle : Google Pixel 5.

Version Android : 11.0 (API 30).

Pourquoi API 30 ? C’est la version maximale supportée par MobSF pour l’analyse dynamique (car les versions supérieures restreignent l’accès en écriture à la partition /system).

Root natif : L’appareil est rooté par défaut, ce qui est indispensable pour l’instrumentation Frida.

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

![Import OVA](https://github.com/user-attachments/assets/5172b2f8-30ec-4524-92f6-ae36ea2fcd7a)

---

Étape 3 : Lancement de l’émulateur avec le script MobSF (rooté & prêt pour l’analyse)

![Import OVA](https://github.com/user-attachments/assets/22f80d08-1f85-46c9-8a44-810f157bd312)

Optimisation de l'interface et Validation ADB
Après le premier démarrage réussi de l'instance MobSF_Geny_API_30, nous avons ajusté la densité d'affichage (DPI) dans les paramètres Android pour une meilleure visibilité des éléments lors de l'analyse dynamique.

Validation finale de l'environnement :
L'émulateur est désormais détecté par l'hôte via ADB. Cette étape confirme que le tunnel réseau entre Genymotion (VirtualBox) et le système hôte est opérationnel.

Identifiant de l'appareil : > * Commande : adb devices

Identifiant : 192.168.56.101:5555





