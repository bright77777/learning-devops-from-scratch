
## Linux → DevOps — Parcours pratique

Parcours pratique basé sur des situations réelles d’administration système et de DevOps.

Environnement :

* MacOS comme machine hôte
* VirtualBox
* VM Debian vierge
* Accès réseau entre le Mac et la VM

Règle principale :

Ne cherche pas uniquement à mémoriser les commandes.

Pour chaque problème :

Problème → Observation → Hypothèse → Commande → Résultat → Diagnostic → Correction → Vérification

Tu peux utiliser man, --help, la documentation Debian et les documentations officielles des logiciels.

Ne cherche pas directement un tutoriel qui fait exactement l’exercice. L’objectif est de développer le réflexe de recherche et de diagnostic.

⸻

Projet 1 — Premier serveur Web

Mission

Tu viens de recevoir un serveur Debian vierge.

Ta première mission est de le transformer en serveur Web accessible depuis ton Mac.

1. Découverte du système

Identifie :

* ton utilisateur actuel ;
* le hostname ;
* la distribution et sa version ;
* l’architecture du système ;
* le nombre de CPU disponibles ;
* la mémoire RAM disponible ;
* l’espace disque disponible ;
* l’adresse IP de la VM.

Tu dois être capable d’expliquer ce que représente chacune de ces informations.

⸻

2. Préparation du serveur

* Mettre à jour les informations des paquets.
* Mettre à jour le système.
* Vérifier que le système est correctement configuré.

⸻

3. Installer Nginx

Installer Nginx.

Vérifier :

* que le paquet est installé ;
* que le service existe ;
* que le service fonctionne ;
* que Nginx écoute bien sur le port HTTP attendu.

⸻

4. Créer une page Web

Créer une page HTML personnalisée.

Elle doit afficher par exemple :

Linux DevOps Lab
Server: Debian
Web Server: Nginx
Author: <ton nom>

Trouver :

* où sont stockés les fichiers Web ;
* où se trouve la configuration Nginx ;
* où se trouvent les logs Nginx.

⸻

5. Tester localement

Depuis la VM, vérifier que le serveur Web répond.

Tu dois pouvoir expliquer la différence entre :

localhost
127.0.0.1
IP de la VM

⸻

6. Tester depuis le Mac

Depuis macOS, accéder à :

http://IP_DE_LA_VM

La page créée précédemment doit apparaître.

⸻

7. Comprendre le service

Arrêter Nginx.

Constater ce qui se passe.

Puis :

* vérifier qu’il est réellement arrêté ;
* redémarrer Nginx ;
* vérifier à nouveau que le site fonctionne.

⸻

8. Mini-incident

Incident :

Le navigateur du Mac n’arrive plus à accéder au site.

Sans réinstaller Nginx, déterminer pourquoi.

Vérifier successivement :

* réseau ;
* adresse IP ;
* processus ;
* service ;
* port ;
* configuration ;
* logs.

⸻

Compétences

À la fin du projet, tu dois comprendre :

* fichiers et répertoires Linux ;
* utilisateurs ;
* paquets ;
* services ;
* processus ;
* ports ;
* IP ;
* HTTP ;
* logs ;
* premières commandes de diagnostic ;
* pipe | ;
* différence entre une commande et un service.

⸻

Projet 2 — Administrer le serveur à distance avec SSH

Mission

Tu n’as plus le droit d’utiliser directement le terminal de la VM.

À partir de maintenant, tu dois administrer Debian depuis ton Mac avec SSH.

⸻

1. Installer OpenSSH Server

Installer et configurer le serveur SSH.

Vérifier :

* que le service fonctionne ;
* que le port SSH écoute ;
* que le serveur est accessible sur le réseau.

⸻

2. Connexion depuis macOS

Depuis le Mac, établir une connexion :

Mac
 ↓
SSH
 ↓
Debian VM

Fermer puis rouvrir plusieurs fois la connexion pour vérifier que tout fonctionne correctement.

⸻

3. Gestion des utilisateurs

Créer un utilisateur dédié :

devops

Tester la connexion avec cet utilisateur.

Comprendre :

* /home
* /etc
* /var
* /tmp
* /opt

⸻

4. Permissions

Créer plusieurs fichiers et répertoires.

Expérimenter avec :

* propriétaire ;
* groupe ;
* permissions de lecture ;
* permissions d’écriture ;
* permissions d’exécution.

Comprendre précisément :

r
w
x

et les permissions :

u
g
o

⸻

5. Authentification par clé SSH

Depuis le Mac :

* créer une paire de clés SSH ;
* configurer l’accès à la VM ;
* vérifier que la connexion fonctionne avec la clé ;
* comprendre la différence entre clé privée et clé publique.

Ne jamais transmettre ou publier la clé privée.

⸻

6. Diagnostic SSH

Créer volontairement une situation dans laquelle SSH ne fonctionne plus.

Exemples :

* service arrêté ;
* mauvais port ;
* mauvaise configuration ;
* problème de permissions ;
* problème réseau.

Diagnostiquer le problème uniquement avec les informations disponibles sur le système.

⸻

7. Logs

Trouver les logs liés à SSH.

Répondre :

* Qui s’est connecté ?
* Quand ?
* Quelle connexion a échoué ?
* Pourquoi ?

⸻

Compétences

À la fin du projet :

Tu dois être capable d’administrer une machine Linux distante sans utiliser son interface graphique.

⸻

Projet 3 — Déployer une application derrière Nginx

Mission

Tu dois maintenant déployer une véritable application sur le serveur.

Architecture cible

Mac
 │
 │ HTTP :80
 ▼
Nginx
 │
 │ reverse proxy
 ▼
Application :3000

⸻

1. Git

Installer Git.

Créer ou utiliser le dépôt :

GitHub
└── linux-devops-lab

Cloner le dépôt sur la VM.

⸻

2. Application

Installer les dépendances nécessaires à l’application.

Lancer l’application.

Vérifier qu’elle fonctionne directement depuis la VM.

Elle doit répondre sur un port différent de 80, par exemple :

localhost:3000

⸻

3. Diagnostic réseau

Déterminer :

* quel processus utilise le port ;
* quel port est ouvert ;
* sur quelle adresse l’application écoute ;
* comment tester l’application sans navigateur.

⸻

4. Nginx Reverse Proxy

Configurer Nginx pour obtenir :

Mac
 ↓
http://IP_VM
 ↓
Nginx :80
 ↓
Application :3000

Le navigateur ne doit plus avoir besoin d’accéder directement au port 3000.

⸻

5. Service systemd

Créer un service :

myapp.service

L’application doit :

* démarrer avec systemd ;
* tourner avec un utilisateur dédié ;
* redémarrer correctement ;
* être vérifiable avec systemctl.

⸻

6. Reboot

Redémarrer complètement la VM.

Après le redémarrage :

* SSH doit fonctionner ;
* Nginx doit fonctionner ;
* l’application doit fonctionner ;
* le site doit être accessible depuis le Mac.

⸻

7. Incidents

Résoudre successivement :

Incident A

Nginx retourne :

502 Bad Gateway

Incident B

L’application fonctionne manuellement mais pas avec systemd.

Incident C

L’application démarre mais ne peut pas lire un fichier de configuration.

Incident D

Le processus existe mais le site n’est plus accessible.

Pour chaque incident :

Diagnostiquer avant de modifier.

⸻

Projet 4 — Préparer un serveur comme un environnement de production

Mission

Tu es responsable d’un serveur Debian qui héberge une application.

Tu dois améliorer son organisation, sa sécurité et sa maintenabilité.

⸻

Architecture

                  Debian
                    │
          ┌─────────┴─────────┐
          │                   │
        Nginx             Application
         :80                  :3000
          │                   │
          └─────────┬─────────┘
                    │
                   Logs

⸻

1. Utilisateurs

Créer une organisation similaire à :

root
 │
 ├── devops
 │
 └── app

L’application ne doit pas fonctionner avec root.

⸻

2. Organisation du système

Organiser correctement les fichiers de l’application :

/opt/myapp
/etc/myapp
/var/log/myapp

Déterminer les permissions nécessaires.

⸻

3. Configuration

Séparer :

code
configuration
logs

Comprendre pourquoi cette séparation est utile.

⸻

4. systemd

Créer et configurer :

myapp.service

Le service doit :

* utiliser l’utilisateur app ;
* démarrer automatiquement ;
* redémarrer si l’application tombe ;
* avoir une configuration claire ;
* produire des logs exploitables.

⸻

5. Logs

Être capable de retrouver :

* les erreurs ;
* les redémarrages ;
* les dernières requêtes ;
* les problèmes de démarrage ;
* les erreurs Nginx ;
* les erreurs de l’application.

Utiliser notamment les commandes Linux permettant de filtrer et analyser du texte.

⸻

6. Incidents

Résoudre :

Incident A

502 Bad Gateway

Incident B

L’application ne démarre plus après un reboot.

Incident C

L’application reçoit :

Permission denied

Incident D

Le serveur écoute sur 3000, mais Nginx ne parvient pas à joindre l’application.

Incident E

Le disque commence à être rempli.

Identifier ce qui consomme l’espace.

⸻

7. Diagnostic obligatoire

Pour chaque incident, écrire dans un fichier :

incident-report.md

avec :

# Incident
## Symptôme
## Hypothèses
## Vérifications
## Cause
## Correction
## Vérification finale

⸻

Projet 5 — Mini environnement DevOps

Mission

Un développeur vient de publier une nouvelle version de l’application sur GitHub.

Tu dois être capable de récupérer cette version et de la déployer sur le serveur.

⸻

Architecture finale

                    GitHub
                       │
                       │ Git
                       ▼
                Debian Server
                       │
              ┌────────┴────────┐
              │                 │
            Nginx           systemd
              │                 │
              └────────┬────────┘
                       │
                    MyApp
                       │
                     Logs

⸻

1. Git

Le serveur doit pouvoir :

* cloner le dépôt ;
* vérifier son état ;
* récupérer une nouvelle version ;
* consulter l’historique ;
* comparer deux versions.

⸻

2. Déploiement

Créer :

deploy.sh

Le script doit automatiser le déploiement.

Objectif :

Nouvelle version
       ↓
git pull
       ↓
installation / mise à jour
       ↓
vérification
       ↓
redémarrage
       ↓
test

⸻

3. Vérification automatique

Après le déploiement, le script doit vérifier :

* que le service fonctionne ;
* que le port attendu écoute ;
* que l’application répond ;
* que Nginx répond ;
* que le HTTP retourne une réponse correcte.

Si une étape échoue, le déploiement doit être considéré comme échoué.

⸻

4. Analyse des logs

À partir des logs, répondre à des questions comme :

Combien d’erreurs ont été enregistrées ?

Quelles erreurs apparaissent le plus souvent ?

Quand le service a-t-il redémarré ?

Combien de requêtes ont retourné une erreur HTTP 500 ?

Quelles sont les dernières erreurs ?

Utiliser les outils Linux appropriés pour filtrer, transformer, compter et trier les données.

⸻

5. Automatisation

Créer des scripts Bash pour les tâches répétitives.

Exemples :

healthcheck.sh
backup.sh
deploy.sh
logs.sh

Chaque script doit avoir un objectif précis.

⸻

6. Incident final

Une série de problèmes est introduite sur le serveur.

Exemples :

* mauvais port ;
* service arrêté ;
* mauvaise permission ;
* mauvaise configuration Nginx ;
* application qui ne démarre plus ;
* fichier de configuration incorrect ;
* disque presque plein ;
* processus qui consomme trop de ressources.

Tu dois diagnostiquer chaque problème sans recevoir directement la commande à utiliser.

⸻

Objectif final

À la fin de ces 5 projets, tu dois être capable de prendre une VM Debian vierge et de :

Installer
   ↓
Configurer
   ↓
Administrer
   ↓
Sécuriser
   ↓
Déployer
   ↓
Diagnostiquer
   ↓
Automatiser
   ↓
Maintenir

Tu ne dois pas seulement connaître les commandes Linux.

Tu dois savoir quand les utiliser, pourquoi les utiliser et comment les combiner pour résoudre un problème réel.

⸻

Règles du parcours

1. Ne mémorise pas les commandes sans comprendre leur utilité.
2. Utilise man et --help.
3. Utilise la documentation officielle.
4. Avant de modifier le système, observe son état.
5. Lors d’un incident, diagnostique avant de corriger.
6. Après chaque correction, vérifie que le problème est réellement résolu.
7. Documente les problèmes rencontrés.
8. Si une commande est utilisée avec |, >, >>, &&, || ou xargs, comprends le rôle de chaque élément.
9. Une solution trouvée sur Internet doit être comprise avant d’être exécutée.
10. À la fin de chaque projet, être capable d’expliquer ce qui a été fait sans lire les commandes.

Progression

Projet 1
Linux + Nginx + réseau
        ↓
Projet 2
SSH + utilisateurs + permissions
        ↓
Projet 3
Git + application + Nginx + systemd
        ↓
Projet 4
Production + logs + sécurité + incidents
        ↓
Projet 5
Git + Bash + déploiement + diagnostic
        ↓
        
Docker
        ↓
CI/CD
        ↓
Jenkins / GitHub Actions
        ↓
Monitoring
        ↓
Infrastructure as Code
        ↓
Kubernetes

