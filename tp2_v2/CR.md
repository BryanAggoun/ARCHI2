Notes pour le CR du TP2 de ARCHI2 :
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
  * `_giet` : fonction asm permettant de jumper au bon point d'entré suivant 

