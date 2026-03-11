C'est une opération courante mais qui demande de la prudence pour ne pas perdre vos données. Voici la procédure étape par étape pour réintégrer votre ancienne partition `/home` dans votre nouvelle installation de Linux Mint.

### ⚠️ Avertissement important
Avant de commencer, **sauvegardez vos données importantes** sur un disque externe si possible. Une erreur de manipulation (comme formater la mauvaise partition) peut être fatale.

---

### Étape 1 : Identifier la partition `/home`
Il faut d'abord trouver l'identifiant unique (UUID) de votre ancienne partition.

1.  Ouvrez un terminal.
2.  Tapez la commande suivante :
    ```bash
    lsblk -f
    ```
    ou
    ```bash
    sudo blkid
    ```
3.  Repérez la partition correspondant à votre ancien `/home` (regardez la taille pour la reconnaître).
4.  Notez son **UUID** (une suite de chiffres et lettres comme `a1b2c3d4-...`) et son type (généralement `ext4`).

### Étape 2 : Sauvegarder le `/home` actuel (nouvelle installation)
Votre nouvelle installation a créé un dossier `/home/votre_nom_utilisateur`. Comme nous allons monter l'ancienne partition sur `/home`, ce dossier actuel sera "masqué". Il est préférable de le déplacer au cas où.

1.  Créez un dossier de sauvegarde à la racine :
    ```bash
    sudo mkdir /root/home_nouveau_backup
    ```
2.  Déplacez le contenu actuel vers ce dossier :
    ```bash
    sudo mv /home/* /root/home_nouveau_backup/
    ```
    *(Assurez-vous que le dossier `/home` est maintenant vide ou ne contient que le point de montage futur).*

### Étape 3 : Modifier le fichier `/etc/fstab`
C'est ce fichier qui dit à Linux de monter la partition au démarrage.

1.  Ouvrez le fichier avec un éditeur de texte (ici `nano`) :
    ```bash
    sudo nano /etc/fstab
    ```
2.  Ajoutez une nouvelle ligne à la fin du fichier avec les informations notées à l'étape 1. La ligne doit ressembler à ceci :
    ```text
    UUID=votre-uuid-note    /home    ext4    defaults    0    2
    ```
    *Remplacez `votre-uuid-note` par le vrai UUID.*
    *Assurez-vous que le point de montage est bien `/home`.*
    *Le système de fichier est généralement `ext4`.*

3.  Sauvegardez et quittez (`Ctrl + O`, `Entrée`, puis `Ctrl + X` dans nano).

### Étape 4 : Monter la partition et tester
Avant de redémarrer, vérifions que tout fonctionne.

1.  Tentez de monter toutes les partitions définies dans fstab :
    ```bash
    sudo mount -a
    ```
    *Si aucune erreur ne s'affiche, c'est bon signe.*
2.  Vérifiez que vos anciens fichiers sont là :
    ```bash
    ls /home
    ```
    Vous devriez voir votre ancien dossier utilisateur.

### Étape 5 : Corriger les permissions (Très important)
Même si vous avez le même nom d'utilisateur, l'identifiant interne (UID) peut avoir changé entre les deux installations. Si c'est le cas, vous n'aurez pas accès à vos fichiers (ou il vous demandera constamment le mot de passe root).

1.  Vérifiez votre UID actuel :
    ```bash
    id -u
    ```
    (C'est souvent `1000`).
2.  Changez le propriétaire de votre dossier personnel pour qu'il corresponde à votre **nouvel** utilisateur. Remplacez `votre_nom_utilisateur` par votre nom de session actuel :
    ```bash
    sudo chown -R votre_nom_utilisateur:votre_nom_utilisateur /home/votre_nom_utilisateur
    ```
    *(La commande `chown -R` change le propriétaire récursivement pour tous les fichiers et sous-dossiers).*

### Étape 6 : Nettoyage et configuration
Les anciennes configurations logicielles (dossiers cachés commençant par `.`) peuvent parfois entrer en conflit avec la nouvelle version de Linux Mint ou des applications mises à jour.

1.  Il est recommandé de vider le cache et de vérifier les configs. Dans votre dossier home :
    ```bash
    rm -rf /home/votre_nom_utilisateur/.cache
    ```
2.  Si vous rencontrez des bugs avec des applications spécifiques (comme Firefox, Thunderbird, ou l'environnement de bureau), vous pouvez renommer leur dossier de configuration dans `.config` pour forcer la recréation (ex: `mv .config/mozilla .config/mozilla.old`).

### Étape 7 : Redémarrage
Redémarrez votre ordinateur :
```bash
sudo reboot
```
Au retour, connectez-vous. Votre session devrait charger avec tous vos anciens documents, images et fichiers, mais avec le nouveau système.

---

### Cas particulier : Chiffrement
Si votre ancienne partition `/home` était **chiffrée** (ecryptfs ou LUKS), cette procédure ne fonctionnera pas directement.
*   **LUKS :** Il faudra d'abord déchiffrer la partition ou configurer `/etc/crypttab`.
*   **Ecryptfs (Home chiffré) :** C'est très complexe à récupérer après réinstallation sans la phrase de passe de déchiffrement spécifique générée lors de la première installation.

Si ce n'était pas chiffré, la procédure ci-dessus est la méthode standard et sûre.