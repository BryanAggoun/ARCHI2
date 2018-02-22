# CR du TP2 de ARCHI2 :

## Modélisation de l'architecture matérielle

__Question C1__ :

Informations sur les caractéristiques du Icache, Dcache et tampon d'écritures postées : 
  - Ligne de cache => 16 octets
  - Capacité totale du cache => 1024 octets
  - Profondeur pour le tampon => 32 octets

```c
    size_t  icache_ways       = 1;  // instruction cache number of ways
    size_t  icache_sets       = 64; // instruction cache number of sets
    size_t  icache_words      = 4;  // instruction cache number of words per line
    size_t  dcache_ways       = 1;  // data cache number of ways
    size_t  dcache_sets       = 64; // data cache number of sets
    size_t  dcache_words      = 4;  // data cache number of words per line
    size_t  wbuf_depth        = 8;  // write buffer depth
```

__Question C2__ :

Le segment seg_reset est assigné au composant ROM contrairement aux autres segments qui sont quand à eux assigné au composant RAM (sauf le segment TTY). En effet, le segment_reset contient le code de boot et doit donc être assigné à la ROM qui est la mémoire devant contenir le premier code exécuté au démarrage de la machine.

__Question C3 :__

Il ne doit pas être cachable pour des problèmes de cohérence avec la mémoire.
En effet, des composants autres que la mémoire peuvent modifier les regitres référés au segment TTY.

__Question C4 :__ 

Les segments de cette architecture étant accessible seulement en mode superviseur :
  * Le segment Reset car il contient le code du boot, qui est le premier code à s'exécuter au démarage de la machine.
  * Le segment Kcode car il contient le code du système d'exploitation.
  * Le segment Kunc et Kdata car ils contiennent d'autres données du NOYAU.

```c
#define SEG_RESET_BASE  0xBFC00000
#define SEG_RESET_SIZE  4096

#define SEG_KCODE_BASE  0x80000000
#define SEG_KCODE_SIZE  16384

#define SEG_KDATA_BASE  0x82000000
#define SEG_KDATA_SIZE  65536

#define SEG_KUNC_BASE   0x81000000
#define SEG_KUNC_SIZE   4096

#define SEG_DATA_BASE   0X01000000
#define SEG_DATA_SIZE   16384

#define SEG_CODE_BASE   0x00400000
#define SEG_CODE_SIZE   16384

#define SEG_STACK_BASE  0x02000000
#define SEG_STACK_SIZE  16384

#define SEG_TTY_BASE    0x90000000
#define SEG_TTY_SIZE    16
```
## Système d'exploitation : GIET

__Question D1 :__ 

Pour exécuter un appel système, le programme utilisateur doit fournir au système la la fonction système demandé ainsi que les arguments nécéssaires de la fonction.
Un appel système s'exécute à l'aide de la fonction user `syscall()`.

__Question D2 :__

Le tableau `_cause_vector[16]` contient les 16 causes qui justifie pourquoi
on veut entrer dans le système (GIET ici).
Ce tableau est initialisé dans le fichier "exc_handler.c".

Le tableau `_syscall_vector[32]` contient les 32 points d'entrées des syscall 
(appels systèmes), soit les adresses de ces fonctions.
Ce tableau est initialisé dans le fichier "sys_handler.c".

__Question D3 :__

L'appel système `proctime()` va entrainer l'appel des fonctions suivantes :
  * `sys_call()` : fonction c utilisée pour implémenter tous les appels systèmes.
  * `_giet` : fonction asm permettant de jumper à une addresse dans le karnel dépendante de la cause d'entré dans celui-ci (syscall dans ce cas).
  * `_sys_handler` : fonction asm qui sauvegarde le contexte et l'addresse de retour du syscall, saute à l'addresse correcte du syscall.
  * `_proctime()` : fonction réelle de `proctime`.


## Génération du code binaire

__Question E1 :__

Le code du boot doit s'exécuter en mode superviseur car il accède aux regitres protégés du processeur.

__Question E2 :__

La convention permettant de récupérer l'adresse du point d'entrée dans le code de l'applicatif est que le fichier binaire de l'application soit au format "elf".
Ce format binaire contient en son entête, contient des meta-informations importante sur l'applicatif dont l'adresse où doivent être rangé chacun des segments.

Avant de compiler le fichier "drivers.c", il faut bien s'assurer de définir "NO_HARD_CC" dans "config.h".

__Question E3 :__

Cela peut entrainer des incohérences mais aussi des bus error et erreur de segmentation car le processeur peut alors écrire dans un composant qui n'est pas la cible voulu.

__Question E4 :__

Les objets logiciels placés dans les 2 segments contenant du code système exécutable sont le code du reset ".reset" et le code du karnel utilisé ".giet".

__Question E5 :__

D'après le fichier "sys.bin.txt" :
  * Longueur effective du segment seg_reset : 40 octets (0xBFC00000 à OxBFC00024 + 4).
  * Longueur effective du segment seg_kcode : 8744 octets (0x80000000 à 0x80002224 + 4).

__Question E6 :__

```c
while(1)
        {
            tty_puts(s);
            tty_gets(&c);
        }
```

__Question E8 :__

Le segment utilisateur possède une taille de **4944** octets.

##  Exécution du code binaire sur le prototype virtuel

__Question F1 :__

la première transaction sur le bus correspond à la lecture du bloc contenantla première instruction du code de boot dans la ROM. Cette instruction est exécuté au 10e cycle.

__Question F2 :__

La première instruction du `main()` est exécuté au cycle 57.

__Question F3 :__

La première transaction se fait au cycle 94. Cette transaction traduit seulement le début de `Hello wordl`.



