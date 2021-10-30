# Crostini - Containers and VM support on Chrome OS

## Auteur

### Kevin Doolaeghe

## Sources

* [Reddit - Instructions for Kali Linux on Crostini](https://www.reddit.com/r/Crostini/comments/fj8ddg/instructions_for_kali_linux_on_crostini/)
* [Chromium - Running Custom Containers Under Chrome OS](https://chromium.googlesource.com/chromiumos/docs/+/master/containers_and_vms.md)

## Accès au terminal `crosh`

Sur Google Chrome, le raccourci `Ctrl`+`Alt`+`T` permet d'ouvrir un nouvel onglet donnant accès à un shell `crosh`.

## Commande `vmc`

* Afficher les VM :
```
vmc list
```
La VM existante se nomme `termina`.

* Démarrer `termina` :
```
vmc start termina
```
La commande `exit` permet de revenir sur le shell `crosh`.

* Arrêter `termina` :
```
vmc stop termina
```

* Détruire `termina` :
```
vmc destroy termina
```

* Créer un conteneur `kali` dans `termina` :
```
vmc container termina kali https://us.images.linuxcontainers.org kali
```
Les images Linux utilisables pour les conteneurs de `termina` sont disponibles [ici](https://us.images.linuxcontainers.org).

## Commande `lxc`

* Ouvrir un Virtual SHell `vsh` sur la VM `termina` :
```
vsh termina
```

* Afficher les conteneurs :
```
lxc list
```

* Afficher les conteneurs :
```
lxc info <container>
```

* Créer une snapshot de conteneur :
```
lxc snapshot <container> <snapshot>
```

* Restaurer une snapshot de conteneur :
```
lxc restore <container> <snapshot>
```

* Supprimer un conteneur :
```
lxc delete <container>
```

* Copier un conteneur :
```
lxc copy <old> <new>
```

* Exécuter un shell `bash` sur le conteneur `kali` :
```
lxc exec <container> -- bash
```

## Configuration initiale du conteneur

* Mettre à jour le conteneur :
```
sudo apt update
sudo apt upgrade
```

* Trouver l'utilisateur non-root créé pour le conteneur :
```
grep 1000:1000 /etc/passwd|cut -d':' -f1
```

* Donner à l'utilisateur un mot de passe et les permissions super-utilisateur :
```
passwd doolaeghekevin
usermod -aG sudo doolaeghekevin
```

* Se connecter avec le compte de l'utilisateur non-root :
```
su - doolaeghekevin
```

* Autoriser les applications à tourner en tâche de fond :
```
loginctl enable-linger doolaeghekevin
```

* Installer le paquet `gnome-terminal` :
```
sudo apt install gnome-terminal
```

## Configuration de base du conteneur `penguin`

* Mettre à jour le conteneur :
```
sudo apt update
sudo apt upgrade
```

* Installer les paquets de base :
```
sudo apt install nano build-essential net-tools git make
```

* Créer un lien symbolique vers `/` dans le dossier utilisateur :
```
sudo ln -s / ~/root
```

* Modifier le mot de passe `root` :
```
sudo passwd root
```

* Modifier les paramètres du shell :
```
nano ~/.bashrc
```

* Installer `vscode` :
```
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
```

```
sudo apt install apt-transport-https
sudo apt update
sudo apt install code
```
