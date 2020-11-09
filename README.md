# Sources serveur

Testé sous FreeBSD 12.0 64bits clang 6.0 mysql 8.0\
Pas de warning en `-Wall` - compilation en 64 bits - `c++17`

### Préparation

Packages FreeBSD nécessaires pour la compilation :

* cmake
* mysql56-client ou supérieur
* python37 (ou autre version en changeant la commande dans le CMakeLists.txt de game)

```
$ pkg install cmake mysql80-client python37
```

### Compilation game

Initialisation de cmake (uniquement à la première compilation ou en cas de modification des `CMakeLists.txt`)

```
$ cd Server/game
$ mkdir build
$ cd build
$ cmake ..
```

puis pour compiler : 

```
$ cd Server/game/build
$ make
```

Le fichier game est généré dans le dossier `build`. 

```
$ cp game /usr/metin2/share/game
```

### Compilation db

Pour compiler `db`, remplacez `game` par `db` dans les commandes ci-dessus.

### Installation game

```
$ cp game /usr/metin2/share/game
```

remplacez `/usr/metin2` par le chemin d'installation de vos fichiers serveur


### Installation db

```
$ cp db /usr/metin2/db/db
```

remplacez `/usr/metin2` par le chemin d'installation de vos fichiers serveur

### Client

Sources client : https://github.com/antokg/clientsrc

