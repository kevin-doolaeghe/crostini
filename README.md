# Crostini - Containers and VM support on Chrome OS

## Auteur

### Kevin Doolaeghe

## Sources

* [Reddit - Instructions for Kali Linux on Crostini](https://www.reddit.com/r/Crostini/comments/fj8ddg/instructions_for_kali_linux_on_crostini/)
* [Installer un dual boot de Kali avec Chrome OS](https://chromeready.com/4714/install-kali-linux-chromebook/)
* [Kali Linux - Tutoriels](https://www.kali-linux.fr/tutoriels)
* [Chromium - Running Custom Containers Under Chrome OS](https://chromium.googlesource.com/chromiumos/docs/+/master/containers_and_vms.md)
* [Crouton - Chrooter un système Linux](https://github.com/dnschneid/crouton)
* [Crouton - Exemple d'installation d'une distribution Xenial](https://itsfoss.com/install-linux-chromebook/)

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
passwd <username>
usermod -aG sudo <username>
```

* Se connecter avec le compte de l'utilisateur non-root :
```
su - <username>
```
