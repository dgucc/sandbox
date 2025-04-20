# Ollama

[cf. linuxtricks.fr : IA : Installer un Modèle de Langage (LLM) avec Ollama](https://www.linuxtricks.fr/wiki/print.php?id=1052)  


## Memo

`$ sudo nano /etc/systemd/system/ollama.service` 

```
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/local/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=$PATH"
Environment="OLLAMA_MODELS=/home/ollama/models"
Environment="OLLAMA_HOST=0.0.0.0:11434"

[Install]
WantedBy=default.target
```

openwebui  

`$ sudo apt-get install podman`  

`$ sudo nano /etc/systemd/system/openwebui.service`  

```
[Unit]
Description=Open WebUI
After=network.target
[Service]
Restart=always
RestartSec=10
ExecStart=/usr/bin/podman start openwebui
ExecStop=/usr/bin/podman stop openwebui
Environment=PODMAN_SYSTEMD_UNIT=%n

Type=forking
[Install]
WantedBy=multi-user.target
```

`$ sudo systemctl daemon-reload`  
`$ sudo systemctl enable openwebui.service`  

`$ podman run -d --rm -p 3000:8080 -v openwebui:/app/backend/data --name openwebui ghcr.io/open-webui/open-webui:main`  
`$ podman ps`  

Open WebUI in browser : http://localhost:3000/  

`$ podman stop openwebui`  

---

<details>
  <summary>Installer ollama (linuxtricks.fr)</summary>

IA : Installer un Modèle de Langage (LLM) avec Ollama
=====================================================

![ollama-logo](https://www.linuxtricks.fr/upload/ollama-logo.png)

  
  

Introduction
------------

  
  
Ollama est une plateforme qui facilite l'utilisation et le déploiement de modèles de langage (LLM) sur différentes infrastructures. Grâce à son interface simple et à ses fonctionnalités avancées, Ollama permet aux développeurs d'intégrer facilement des LLM dans leurs applications.  
Dans cet article, on va voir comment installer et utiliser Ollama.  
  
La base de cet article a été faite sur Red Hat Enterprise Linux 9.  
L'installation se fait de façon classique, sans Docker.  
  
Le site web officiel est disponible ici : [https://ollama.com/](https://ollama.com/)  
Les différents modèles de langage sont répertoriés ici : [https://ollama.com/library](https://ollama.com/library)  
  
Au moment de la résaction de cet article en Septembre 2024, les plateformes supportées sont x86\_64 et ARM64 (aarch64).  
  

Prérequis
---------

  
  
La génération de texte via des LLM est évidemment gourmande en ressources CPU, (GPU) et RAM. Il est nécessaire d'avoir une machine suffisemment dimensionnée pour le besoin.  
  
Pour de meilleures performances, il sera utile d'installer les pilotes propriétaires de notre carte graphique.  
  
Pour les cartes AMD - ROCm : [https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/quick-start.html](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/quick-start.html)  
Pour les cartes NVidia - CUDA : [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)  
  
A défaut, les calculs seront faits avec le CPU (plus lent)  
  

Installation d'Ollama
---------------------

  
  

### Installer ollama

  
  
Ollama n'est pas dans les dépôts des principales distributions.  
Bien qu'il existe un script automatisé d'installation, je vais ici faire les étapes manuellement.  
  
On récupère dans un premier temps ollama pour l'architecture souhaitée :  

Code BASH :

`wget https://ollama.com/download/ollama-linux-amd64.tgz`  

  
  
Pour ARM64 :  

Code BASH :

`wget https://ollama.com/download/ollama-linux-arm64.tgz`

  
  
Ensuite, on extrait l'archive :  

Code BASH :

`tar \-C /usr \-xvzf ollama-linux-\*.tgz`

  
  
Dans le cas d'une carte graphique AMD, on récupère des éléments additionnels :  

Code BASH :

`wget https://ollama.com/download/ollama-linux-amd64-rocm.tgz`

  
  
Et on les installe :  

Code BASH :

`tar \-C /usr \-xvzf ollama-linux-amd64-rocm.tgz`

  
  

### Création d'un utilisateur dédié

  
  
On va ensuite créer un utilisateur dédié et un service pour lancer automatiquement ollama avec un utilisateur spécifique.  

Code BASH :

`useradd \-r \-s /bin/false \-U \-m \-d /usr/share/ollama ollama`

  
  
On ajoute les utilisateurs d'ollama dans le groupe ollama :  

Code BASH :

`usermod \-a \-G ollama adrien`

  
  

### Création d'un service systemd

  
  
On va créer un service systemd pour lancer ollama au démarrage du système :  

Code BASH :

`vim /etc/systemd/system/ollama.service`

  
  
Voici le contenu du service systemd :  

Code :

```
[Unit]
Description=Ollama Service
After=network-online.target
[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=$PATH"
[Install]
WantedBy=default.target`
```
  
  
On recharge systemd :  

Code BASH :

`systemctl daemon-reload`

  
  
On active et démarre le service :  

Code BASH :

`systemctl enable \--now ollama`

  
  
On pourra voir les logs avec :  

Code BASH :

`journalctl \-u ollama`

  
  
Toutes les commandes ollama peuvent être utilisées en tant qu'utilisateur classique du système.  
  

Informations sur les modèles
----------------------------

  
  
Lorsque Oolama est installé, il n’inclut pas de modèles.  
  
Pour lister les modèles installés :  

Code BASH :

`ollama list`

  
  
Cette liste est en effet vide.  
  
Dans le contexte des modèles de langage (LLM), les chiffres tels que 8B, 70B, et 405B font référence au nombre de paramètres du modèle, exprimé en milliards (B pour "billion" en anglais). Les paramètres sont les éléments fondamentaux qui déterminent le comportement du modèle et sa capacité à apprendre des relations complexes dans les données.  
  
Voici 3 exemples pour comprendre la relation entre l'efficacité et les ressources nécessaires :  
\- **8B** (8 milliards) : Cela signifie que le modèle a 8 milliards de paramètres. Les modèles avec un nombre de paramètres plus faible peuvent être plus rapides à entraîner et à exécuter, mais ils peuvent également avoir des limitations en termes de compréhension et de génération de texte par rapport à des modèles plus grands.  
\- **70B** (70 milliards) : Ce modèle a 70 milliards de paramètres. En général, un modèle avec un plus grand nombre de paramètres peut capturer des nuances plus fines dans le langage et produire des résultats de meilleure qualité, mais il nécessite également plus de ressources pour l'entraînement et l'inférence.  
\- **405B** (405 milliards) : Ce modèle a 405 milliards de paramètres. Ces modèles peuvent générer un texte très cohérent et contextuellement pertinent, mais ils nécessitent des infrastructures matérielles très puissantes pour fonctionner efficacement.  
  
Les modèles sont stockés dans **/usr/share/ollama/.ollama/models**. Il est possible de changer leur emplacement. Se référer à la section adéquate en bas de cet article.  
  

Installer des modèles publics
-----------------------------

  
  
Sur le site d'Ollama, plusieurs modèles sont disponibles : [https://ollama.com/library](https://ollama.com/library)  
Dans ce tutoriel, on va utiliser celui qui a été le plus téléchargé : **llama3** ([https://ollama.com/library/llama3](https://ollama.com/library/llama3))  
  
On va récupérer ce modèle avec la commande **ollama pull** :  

Code BASH :

`ollama pull llama3`

  
  
La sortie produite pendant le téléchargement est similaire à la récupération d'images Docker :  

Code :

```
pulling manifest   pulling 6a0746a1ec1a... 100% ▕█████████████▏ 4.7 GB   pulling 4fa551d4f938... 100% ▕█████████████▏ 12 KB
pulling 8ab4849b038c... 100% ▕█████████████▏ 254 B
pulling 577073ffcc6c... 100% ▕█████████████▏ 110 B
pulling 3f8eb4da87fa... 100% ▕█████████████▏ 485 B   
verifying sha256 digest
writing manifest
success`
```
  
  
Par défaut il s'agit de la variante 8B.  
  
Pour récupérer la variante 70b :  

Code BASH :

`ollama pull llama3:70b`

  
  
On pourra vérifier que les modèles installés avec :  

Code BASH :

`ollama list`

  
  
Ce qui donne :  

Code :

```
NAME             ID              SIZE      MODIFIED
llama3:70b       786f3184aec0    39 GB     10 hours ago      llama3:latest    365c0bd3c000    4.7 GB    10 hours ago`  
```
  
  

Utiliser Ollama
---------------

  
  

### Exécuter le modèle en mode interractif

  
  
Une fois le modèle téléchargé, on pourra l'exécuter en utilisant la commande suivante :  

Code BASH :

`ollama run llama3`

  
  
Le prompt est prêt à écouter une requête :  

Code :

`>>> Send a message (/? for help)`

  
  
Si on lui pose une question :  

Code :

`>>> Qu'est ce que Fedora Linux ?`

  
  
Le LLM traite la demande et nous répond :  

Code :

```
Fedora est une distribution de système d'exploitation GNU/Linux créée en 2003 par le projet Fedora, un sous-projet de la Fondation Red Hat. Elle est considérée comme une version "bêta" ou "prérequis" pour l'éditeur de système d'exploitation Linux commercial, CentOS et Red Hat Enterprise Linux (RHEL).   Fedora est conçue pour être une distribution open-source, libre et communautaire, qui permet aux développeurs et aux utilisateurs de tester les dernières versions des logiciels et des fonctionnalités avant leur intégration dans les éditions commerciales. Elle est donc très populaire parmi les développeurs et les passionnés de Linux.   Les caractéristiques clés de Fedora sont :   1. Libre et open-source, ce qui signifie que tout le code est disponible sous licence libre.   2. Rapide à mettre à jour, car il s'agit d'une version "bêta" pour tester les nouveaux logiciels et les fonctionnalités avant leur intégration dans les éditions commerciales.   3. Conçue pour être compatible avec les systèmes d'exploitation Linux commerçiaux, tels que CentOS et RHEL.   4. Possède une communauté active de développeurs et d'utilisateurs qui contribuent au projet.   Fedora est disponible sous forme de CD/DVD ou de téléchargement direct, et il existe plusieurs versions pour différents ordinateurs et architectures (x86-64, ARM, etc.).
```

  
  

### Exécuter le modèle en one shot

  
  
On pourra exécuter une requête en utilisant la commande suivante :  

Code BASH :

`ollama run llama3 "Qu'est ce que Fedora Linux?"

  
  
Le résultat est produit sur la sortie standard.  
  

### Interroger le modèle via l'API

  
  
On pourra interroger le modèle via l'API disponible.  
Ollama écoute sur le port 11434.  
  
Voici un exemple de requête avec curl interrogeant l'API avec du JSON :  

Code BASH :
```bash
curl \-X POST http://localhost:11434/api/generate \-d '{
  "model": "llama3",
  "prompt":"En une phrase, qu est ce que Fedora Linux ?"
 }'
```
  
  
La réponse est produite en JSON :  

Code JAVASCRIPT :
```json
{"model":"llama3","created\_at":"2024-09-19T20:16:55.070219385Z","response":"Fed","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.184960991Z","response":"ora","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.248871673Z","response":" est","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.336215829Z","response":" un","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.435178812Z","response":" système","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.499418477Z","response":" d","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.561225601Z","response":"'","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.630712386Z","response":"explo","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.703879616Z","response":"itation","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.765078378Z","response":" Linux","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.840173868Z","response":" gratuit","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.90263307Z","response":" et","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:55.971443963Z","response":" open","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.034062226Z","response":"-source","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.09947807Z","response":",","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.172949487Z","response":" cré","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.256597318Z","response":"é","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.323157737Z","response":" par","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.390106591Z","response":" la","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.459911313Z","response":" commun","done":false}
{"model":"llama3","created\_at":"2024-09-19T20:16:56.526663982Z","response":"auté","done":false}
```
  
  
Par défaut, la réponse est fournie bout par bout et on voit le texte se générer.  
Pour une application, celle-ci peut traiter traiter la réponse :  
\- alors que la génération n'est pas terminée  
\- en recevant de petites quantité de données  
  
  
On pourra demander la réponse qu'une fois la génération terminée :  

Code BASH :
```bash
curl \-X POST http://localhost:11434/api/generate \-d '{
  "model": "llama3",
  "prompt":"En une phrase, qu est ce que Fedora Linux ?",
  "stream": false
 }'
```
  
  
La réponse met du temps à s'afficher mais elle est générée d'un bloc :  

Code JAVASCRIPT :
```json
{"model":"llama3","created\_at":"2024-09-19T20:27:46.739492809Z","response":"Fedora Linux est un système d'exploitation tournant sur le noyau Linux, conçu pour être une plateforme de développement et de test avant l'intégration dans la version commerciale Red Hat Enterprise Linux (RHEL), proposant une distribution stable et actuelle des dernières technologies open-source.","done":true,"done\_reason":"stop","context":\[128006,882,128007,271,1737,6316,17571,934,1826,3846,1744,80606,14677,30,128009,128006,78191,128007,271,92887,6347,14677,1826,653,72601,294,6,69331,7709,259,3514,519,1765,514,912,88,2933,14677,11,390,79884,5019,23761,6316,12235,76701,409,82620,1880,409,1296,33670,326,55624,978,911,367,7010,1208,2373,95194,20487,3816,22050,26551,14677,320,49,51812,705,10045,519,6316,8141,15528,1880,1180,31037,951,36852,59307,14645,1825,31874,13\],"total\_duration":5010253585,"load\_duration":26615990,"prompt\_eval\_count":20,"prompt\_eval\_duration":330708000,"eval\_count":67,"eval\_duration":4609753000}
```
  
  
Plus d'informations sur l'utilisation de l'API : [https://github.com/ollama/ollama/blob/main/docs/api.md](https://github.com/ollama/ollama/blob/main/docs/api.md)  
  

### Activité du système

  
  
Lors de la réponse, qui peut être générée plus ou moins vite suivant les ressources de notre serveur. Les calculs sont générés sur notre serveur lui même et le LLM est totalement autonome.  
  
Lorsque le LLM traite la réponse à notre prompt, voici un aperçu sur le serveur avec htop de l'activité :  

![ollama-htop-serveur](https://www.linuxtricks.fr/upload/ollama-htop-serveur.png))

  
  

Faire écouter Ollama sur le réseau
----------------------------------

  
  
Par défaut, Ollama écoute sur le port 11434 et uniquement en local :  

Code BASH :

`ss \-unplat | grep 11434`

  

Code :

`tcp   LISTEN 0      4096       127.0.0.1:11434       0.0.0.0:*`

  
  
Pour permettre à Ollama d'accepter des requêtes depuis d'autres hôtes, on édite le service systemd créé précédemment.  
Dans la section **\[Service\]** on ajoute une ligne **Environment** :  

Code BASH :

Environment\="OLLAMA\_HOST=0.0.0.0"

  
  
On recharge systemd :  

Code BASH :

`systemctl daemon-reload`

  
  
On redémarre Ollama via le service :  

Code BASH :

`systemctl restart ollama.service`

  
  
On vérifie que Ollama écoute sur toutes les adresses :  

Code BASH :

`ss \-unplat | grep 11434`

  

Code :

`tcp   LISTEN 0      4096               *:11434             *:*`

  
  
Mettre en oeuvre les mesures de protection (Pare-Feu) pour que n'importe qui ne puisse pas requêter votre LLM !  
  

Personnaliser le comportement d'un modèle
-----------------------------------------

  
  
Avec Ollama, il est possible de modifier le comportement d'un modèle.  
  
Cela passe par la création d'un fichier **Modelfile**. C'est dans ce fichier qu'on va définir les instructions nécessaires pour personnaliser votre modèle.  
  
Ici, je vais partir sur le LLM d'IBM nommé granite (Merci le Red Hat Summit pour la découverte), que je vais personnaliser pour qu'il me produise directement du code PERL.  
  
On va ranger le modèle dans un dossier, par exemple :  

Code BASH :

`mkdir \-p ~/ollama-modeles/granite-code-perl`

  
  
Dans ce dossier, on va créer notre fichier Modelfile :  

Code BASH :

`cd ~/ollama-modeles/granite-code-perl/`

  

Code BASH :

`vim Modelfile`

  
  
Voici un exemple :  

Code BASH :
```
FROM granite-code:20b
PARAMETER temperature 0.5
PARAMETER num\_ctx 4096
SYSTEM Le code à produire doit être en langage PERL et être sécurisé. Donne juste le code sans explications.
```
  
  
Bien que les fichiers Modelfiles soient hautement personnalisables (cf. la doc : [https://github.com/ollama/ollama/blob/main/docs/modelfile.md)](https://github.com/ollama/ollama/blob/main/docs/modelfile.md)) voici quelques notions rapides que j'utilise dans le modèle présenté :  
  
**FROM** : Spécifie le modèle de base à utiliser (obligatoire)  
**PARAMETER** : Définit les paramètres d'exécution du modèle  
**SYSTEM** : Définit le message système qui guidera le comportement du modèle.  
  
Je vous ai montré 2 paramètres ici :  
**temperature** : Ce paramètre contrôle la créativité et l'aléatoire des réponses du modèle. Une valeur élevée (comme 1) rend le modèle plus créatif et imprévisible. A contrario, une valeur plus basse (proche de 0) rend les réponses plus cohérentes et déterministes.  
**num\_ctx** : Ce paramètre définit la taille de la fenêtre de contexte du modèle. Une valeur plus élevée permet au modèle de "se souvenir" d'une plus grande partie de la conversation ou du texte précédent. La valeur par défaut est souvent 2048.  
  
Ensuite, on va déployer le modèle via :  

Code BASH :

`ollama create granite-perl \-f ~/ollama-modeles/granite-code-perl/Modelfile`

  
  
Si le modèle de base est déjà installé, la création est très rapide.  
  
On pourra vérifier que le modele personnalisé est bien intégré avec :  

Code BASH :

`ollama list`

  
  
Ce qui donne :  

Code :

```
NAME                   ID              SIZE      MODIFIED
granite-perl:latest    0e85cc76ba66    2.0 GB    6 seconds ago        llama3:latest          365c0bd3c000    4.7 GB    18 minutes ago
```
  
  
On peut tester une requête en utilisant la commande suivante :  

Code BASH :

`ollama run granite-perl "Lire le CSV export.csv ligne par ligne et afficher les champs 1 et 3"`

  
  
On n'a rien précisé et le contexte a bien été pris en compte (de produire du code PERL) et on n'a pas le blabla habituel :  

Code :

` ```perl   
#!/usr/bin/perl -w   
use strict;   
use warnings;   
open my $fh, '<', 'export.csv' or die "Impossible d'ouvrir l'YYYY export.csv : $!";   
while (my $line = <$fh>) {       
  chomp $line;       
  my ($ champ1, undef, $champ3 ) = split ",", $line;       
  print "$champ1,$champ3\n";   
}   
close $fh;   
``` 

  
  

Personnalisation fantaisiste

On pourra demander des choses fantaisistes, en faisant se comporter le LLM comme Pikachu par exemple :  

Code BASH :

`mkdir \-p ~/ollama-modeles/llama3-pokemon`

  
  
On va créer un Modelfile plus fantaisiste :  

Code BASH :

`vim ~/ollama-modeles/llama3-pokemon/Modelfile`

  
  
Qui contient ceci :  

Code TEXT :
```
FROM llama3
PARAMETER temperature 2
PARAMETER num\_ctx 4096
SYSTEM Répond à toutes les questions comme si tu étais Pikachu
```
  
  
On créé le modèle :  

Code BASH :

`ollama create pika \-f ~/ollama-modeles/llama3-pokemon/Modelfile`

  
  
On utilise le modèle :  

Code BASH :

`ollama run pika "Comment ça va ?"`

  
  
Le résultat est sans appel ![:)](/images/smileys/1.gif ":)")  

Code TEXT :

`Pika, pika! Chuuuuuuu! (J'vrai bien, merci!)`

  
  
  

Mettre à jour Ollama
--------------------

  
  
Pour mettre à jour Ollama, il suffit simplement de retélécharger l'archive et la réextraire sur notre système comme vu dans la section installation.  
  
On n'aura pas besoin de refaire les étapes de création d'utilisateur et de service systemd.  
  
Il suffira juste, après la mise à jour, de redémarrer le service pour charger la nouvelle version.  
  

Supprimer Ollama
----------------

  
  
Si on a juste voulu jouer avec Ollama et qu'on en a plus besoin, voici comment le désinstaller.  
  
On supprime le service ollama (après l'avoir stoppé et désactivé :  

Code BASH :
```bash
systemctl stop ollama
systemctl disable ollama
rm /etc/systemd/system/ollama.service
```
  
  
On supprime le binaire :  

Code BASH :

`rm /usr/bin/ollama`

  
  
On supprime les modèles téléchargés :  

Code BASH :

`rm \-r /usr/share/ollama`

  
  
On supprime le dossier des bibliothèques additionnelles :  

Code BASH :

`rm \-r /usr/lib/ollama`

  
  
On supprime l'utilisateur et le groupe ollama:  

Code BASH :
```bash
userdel ollama
groupdel ollama
```
  
  

Informations complémentaires
----------------------------

  
  

### Emplacement des modèles

  
  
Les modèles sont stockés dans **/usr/share/ollama/.ollama/models**.  
  
Il est possible de changer l'emplacement par défaut en ajoutant une nouvelle variable d'environnement.  
  
On éditera le service précédemment créé :  

Code BASH :

`vim /etc/systemd/system/ollama.service`

  
  
On ajoutera une variable d'environnement **OLLAMA\_MODELS** :  
  

Code :

```
[Unit]
Description=Ollama Service
After=network-online.target
[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=$PATH"
Environment="OLLAMA_MODELS=/home/ollama/models"
[Install]
WantedBy=default.target`
```
  
  
On recharge systemd :  

Code BASH :

`systemctl daemon-reload`

  
  
Et on redémarre le service :  

Code BASH :

`systemctl restart ollama.service`

  
  
Si on avait déjà des modèles installés, on pourra les déplacer :  

Code BASH :

`mv /usr/share/ollama/.ollama/models /home/ollama/models`

  
  
On pensera bien à réaffecter les droits à l'utilisateur et au groupe ollama :  

Code BASH :

`chown \-R ollama:ollama /home/ollama`

</details>
