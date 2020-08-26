# Sources serveur

### Préparation

Packages FreeBSD nécessaires pour la compilation :

* gmake
* makedepend
* mysql56-client ou supérieur
* python37 (ou autre version en changeant la commande dans le Makefile de game)

### Première compilation

```
$ cd Server
$ gmake all
```

Testé sous FreeBSD 12.0 64bits mysql 8.0

### Client

Sources client : https://github.com/antokg/clientsrc
