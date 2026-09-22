## Et surtout, les projets ne sont pas  indépendants : le projet 2 reprend le serveur du projet 1, le 3 reprend le 2, etc. À la fin, il a construit progressivement un vrai petit serveur Linux exploitable, plutôt que cinq exercices jetables.


Projet 1 — Mettre en place son premier serveur Web

Objectif : comprendre la machine, les fichiers, les paquets, les processus et le réseau.

Mission

Tu viens de recevoir un serveur Debian vierge.
Tu dois le transformer en serveur Web accessible depuis ton Mac.

Il doit :

1. Identifier :
    * utilisateur courant
    * hostname
    * IP de la VM
    * distribution/version Debian
    * architecture CPU
    * mémoire/disque disponibles
2. Mettre le système à jour.
3. Installer Nginx.
4. Vérifier que Nginx :
    * est installé
    * tourne
    * écoute sur le port 80
5. Créer une page HTML personnalisée.
6. Trouver où Nginx stocke :
    * ses fichiers de configuration
    * les fichiers Web
    * les logs
7. Depuis macOS, ouvrir :

http://IP_DE_LA_VM

et afficher la page.

8. Arrêter Nginx → constater que le site ne répond plus.
9. Le redémarrer.

Commandes qu’il devrait naturellement rencontrer

apt, systemctl, service, ps, ss, ip, curl, cat, less, grep, nano, cp, mv, rm, mkdir, chmod, sudo.

Compétence acquise :

« Je sais prendre une machine Linux vierge et y déployer un service accessible sur le réseau. »

⸻

Projet 2 — SSH : administrer la VM comme un vrai serveur

On arrête progressivement de travailler directement dans VirtualBox.

Mission

Le serveur est dans une salle distante. Tu n’as plus accès à son écran. Tu dois l’administrer uniquement avec SSH depuis ton Mac.

Installer et configurer OpenSSH Server.

Depuis macOS :

ssh user@IP_VM

Il doit ensuite :

* créer un nouvel utilisateur devops
* lui donner les permissions nécessaires
* comprendre /home
* comprendre /etc
* comprendre /var
* comprendre /tmp
* modifier le hostname
* vérifier les connexions SSH
* consulter les logs SSH
* identifier les connexions actives
* se déconnecter/reconnecter

Puis deuxième étape :

Authentification par clé SSH

Créer une paire :

Mac
 ↓
SSH private key
 ↓
Debian VM

Configurer l’authentification par clé et vérifier qu’il peut se connecter sans mot de passe.

Puis éventuellement désactiver l’authentification SSH par mot de passe après avoir vérifié que la clé fonctionne.

Use case diagnostic

Tu lui donnes volontairement une situation :

« SSH ne fonctionne plus. Trouve pourquoi. »

Il doit utiliser :

systemctl
ss
ip
grep
journalctl
tail

pour déterminer le problème.

Compétence acquise :

Administrer une machine Linux distante sans interface graphique.

⸻

Projet 3 — Déployer une vraie application derrière Nginx

Ici on commence à entrer dans le vrai DevOps.

On prend un dépôt GitHub réel, par exemple une petite application Node.js/Python que vous avez choisie pour l’exercice.

Architecture :

Mac
 │
 │ HTTP
 ▼
Debian VM
 │
 ▼
Nginx :80
 │
 ▼
Application :3000

Mission

Tu dois déployer cette application sur le serveur.

Il doit :

1. Installer Git.
2. Cloner le dépôt GitHub.
3. Installer les dépendances.
4. Lancer l’application.
5. Vérifier qu’elle répond directement sur localhost:3000.
6. Comprendre pourquoi elle fonctionne sur 3000 mais que l’utilisateur doit accéder au site sur 80.
7. Configurer Nginx comme reverse proxy.

Donc :

Mac
 ↓
:80
 ↓
Nginx
 ↓
:3000
 ↓
Application

Puis on casse volontairement quelque chose.

Par exemple :

* mauvaise configuration Nginx
* mauvais port
* application arrêtée
* mauvaise permission
* mauvais chemin
* mauvais processus

Et il doit diagnostiquer avec :

curl
grep
tail
journalctl
systemctl
ps
ss

Bonus

Créer un service systemd pour que l’application démarre automatiquement :

VM reboot
    ↓
systemd
    ↓
Application
    ↓
Nginx

Là, il commence réellement à comprendre service + processus + réseau + reverse proxy + logs.

⸻

Projet 4 — Petit serveur de production

Ici, on lui donne une mission beaucoup plus réaliste.

« Tu es responsable d’un serveur Debian hébergeant une application. Mets-le dans un état exploitable. »

Architecture

             Debian VM
                 │
        ┌────────┴────────┐
        │                 │
      Nginx           Application
      :80                :3000
        │                 │
        └────────┬────────┘
                 │
               logs

Il doit mettre en place :

Utilisateurs

root
 ├── devops
 └── app

L’application ne doit pas tourner en root.

Filesystem

Il doit organiser quelque chose comme :

/opt/myapp
/var/log/myapp
/etc/myapp

et déterminer lui-même les permissions adaptées.

Service

Créer :

myapp.service

avec systemd.

Le service doit :

* démarrer automatiquement
* redémarrer si l’application tombe
* fonctionner avec l’utilisateur app
* écrire correctement ses logs

Nginx

Configurer :

:80 → myapp:3000

Diagnostic

On lui donne plusieurs incidents successifs :

Incident A

Le site retourne 502 Bad Gateway.

Incident B

L’application fonctionne manuellement mais ne démarre pas après reboot.

Incident C

L’application ne peut plus écrire dans son dossier.

Incident D

Le serveur écoute sur 3000, mais le site n’est plus accessible.

À chaque fois, il doit diagnostiquer avant de modifier.

C’est important.

⸻

Projet 5 — Mini environnement DevOps complet

Celui-ci peut devenir un véritable petit projet portfolio.

On part d’un dépôt GitHub.

Par exemple :

github.com/<organisation>/linux-devops-lab

avec :

app/
├── src/
├── package.json
└── README.md

Mission

Un développeur vient de pousser une nouvelle version de l’application. Tu dois être capable de récupérer, déployer et maintenir cette application sur le serveur.

Il doit mettre en place :

             GitHub
                │
                │ git clone / pull
                ▼
          Debian Server
                │
        ┌───────┴────────┐
        │                │
      Nginx          systemd
        │                │
        └───────┬────────┘
                ▼
             App

Étape 1 — Git

Il apprend réellement :

git clone
git pull
git status
git log
git diff
git checkout

Étape 2 — Déploiement

Créer un script :

deploy.sh

qui fait par exemple :

récupérer nouvelle version
        ↓
installer dépendances
        ↓
vérifier application
        ↓
redémarrer service
        ↓
vérifier service
        ↓
tester HTTP

Étape 3 — Logs

Créer des commandes/scripts permettant de répondre à :

Combien d’erreurs aujourd’hui ?

Quelles sont les erreurs les plus fréquentes ?

Quand le service a-t-il redémarré ?

Combien de requêtes ont retourné 500 ?

Et là, il va naturellement utiliser :

grep
awk
sed
sort
uniq
wc
head
tail
cut
xargs
|
>
>>

C’est exactement le genre de situation où grep + pipe cesse d’être abstrait.

⸻

Et je rajouterais des “incidents” à chaque projet

C’est probablement le point le plus important.

Au lieu de faire uniquement :

« Installe Nginx. »

on peut dire :

Incident : le serveur Web ne répond plus.

Et lui doit déterminer :

Est-ce que la machine est accessible ?
        ↓
Quelle est son IP ?
        ↓
Le réseau fonctionne ?
        ↓
Le port 80 écoute ?
        ↓
Nginx tourne ?
        ↓
Nginx est correctement configuré ?
        ↓
Le firewall bloque ?
        ↓
Le backend répond ?
        ↓
Les logs disent quoi ?

Il apprend ainsi à raisonner :

PROBLÈME
   ↓
OBSERVATION
   ↓
HYPOTHÈSE
   ↓
COMMANDE
   ↓
RÉSULTAT
   ↓
DIAGNOSTIC
   ↓
CORRECTION
   ↓
VÉRIFICATION

C’est beaucoup plus proche du travail réel d’un administrateur Linux/DevOps que de mémoriser une liste de commandes.

Progression globale

Projet	Thème principal	Niveau
1. Debian Web Server	Linux + Nginx + réseau	🟢
2. Remote Server	SSH + utilisateurs + permissions	🟢
3. Application Server	Git + app + Nginx + systemd	🟡
4. Production Server	services + sécurité + logs + incidents	🟠
5. DevOps Deployment Lab	Git + déploiement + scripts + monitoring	🔴

