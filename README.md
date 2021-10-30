# Crostini - Containers and VM support on Chrome OS

## Auteur

### Kevin Doolaeghe

## Sources

* [Reddit - Instructions for Kali Linux on Crostini](https://www.reddit.com/r/Crostini/comments/fj8ddg/instructions_for_kali_linux_on_crostini/)

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

* Exécuter un shell `bash` sur le conteneur `kali` :
```
lxc exec kali -- bash
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
passwd <username>
usermod -aG sudo <username>
```

* Se connecter avec le compte de l'utilisateur non-root :
```
su - <username>
```

## Intégration avec Chrome OS

* Installer les paquets nécessaires :
```
sudo apt install -y gnupg2 psmisc
```

* Installer `cros-guest-tools` pour intégrer le conteneur à Chrome OS :
```
sudo apt install ca-certificates
sudo echo "deb https://storage.googleapis.com/cros-packages stretch main" > /etc/apt/sources.list.d/cros.list
if [ -f /dev/.cros_milestone ]; then sudo sed -i "s?packages?packages/$(cat /dev/.cros_milestone)?" /etc/apt/sources.list.d/cros.list; fi
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1397BC53640DB551
```

```
sudo apt update
```

```
sudo apt install cros-guest-tools
```

* Activer l'intégration du conteneur à Chrome OS :
```
systemctl --user enable --now sommelier@0 sommelier-x@0 sommelier@1 sommelier-x@1
```

* Autoriser les applications à tourner en tâche de fond :
```
loginctl enable-linger <username>
```

* Installer le paquet `gnome-terminal` :
```
sudo apt-get install gnome-terminal
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
