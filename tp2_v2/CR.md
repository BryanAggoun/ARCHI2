# Notes pour le CR du TP2 de ARCHI2 :
===================================

Question C2 :
---------------

Le segment seg_reset est assigné au composant ROM contrairement aux autres segments qui sont quand à eux assigné au composant RAM (sauf le segment TTY). En effet, le segment_reset contient le code de boot et doit donc être assigné à la ROM qui est la mémoire devant contenir le premier code exécuté au démarrage de la machine.

Question C3 :
---------------

Il ne doit pas être cachable pour des problèmes de cohérence avec la mémoire.

Question C4 : 
---------------

Les segments de cette architecture étant accessible seulement en mode superviseur :
  * Le segment Reset car il contient le code du boot, qui est le premier code à s'exécuter au démarage de la machine.
  * Le segment Kcode car il contient le code du système d'exploitation.
  * Le segment Kunc et Kdata ?????

Question D1 : 
---------------

Le programme utilisateur doit fournir au système la cause de l'appel système,
la fonction du noyau demandée,...

Question D2 :
---------------

Le tableau `_cause_vector[16]` contient les 16 causes qui justifie pourquoi
on veut entrer dans le système (GIET ici).
Ce tableau est initialisé dans le fichier "exc_handler.c".

Le tableau `_syscall_vector[32]` contient les 32 points d'entrées des syscall 
(appels systèmes), soit les adresses de ces fonctions.
Ce tableau est initialisé dans le fichier "sys_handler.c".

Question D3 :
---------------

L'appel système `proctime()` va entrainer l'appel des fonctions suivantes :
  * `sys_call()` : fonction c utilisée pour implémenter tous les appels systèmes.
  * `_giet` : fonction asm permettant de jumper à une addresse dans le karnel dépendante de la cause d'entré dans celui-ci (syscall dans ce cas).
  * `_sys_handler` : fonction asm qui sauvegarde le contexte et l'addresse de retour du syscall, saute à l'addresse correcte du syscall.
  * `proctime()` : fonction réelle `proctime`.

Question D4 :
---------------

## E) Génération du code binaire
------------------------------

Question E1 :
---------------

Le code du boot doit s'exécuter en mode superviseur car il accède aux regitres protégés du processeur.

Question E2 :
---------------

La convention permettant de récupérer l'adresse du point d'entrée dans le code de l'applicatif est que le fichier binaire de l'application soit au format "elf".
Ce format binaire contient en son entête, contient des meta-informations importante sur l'applicatif dont l'adresse où doivent être rangé chacun des segments.

Avant de compiler le fichier "drivers.c", il faut bien s'assurer de définir "NO_HARD_CC" dans "config.h".

Question E3 :
---------------

Cela peut entrainer des incohérences.

Question E4 : 
---------------

Les objets logiciels placés dans les 2 segments contenant du code système exécutable sont le code du reset ".reset" et le code du karnel utilisé ".giet".

Question E5 :
---------------

D'après le fichier "sys.bin.txt" :
  * Longueur effective du segment seg_reset : 40 octets (0xBFC00000 à OxBFC00024 =>36 octets).
  * Longueur effective du segment seg_kcode : 8741 octets "?????" (0x80000000 à 0x80002224).

