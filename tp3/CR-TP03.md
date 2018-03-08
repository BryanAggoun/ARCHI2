Notes pour le TP3 de ARCHI2 :
==============================

## Application logicielle :

__Question C1 :__

L'adresse de la première instruction de la fonction `main` est "0x00400000".
En effet, elle correspond à la première adresse du segment code.

L'adresse de la première instruction de la boucle d'étiquette *loop* est "0x00400010".

__Question C2 :__

L'adresse de base du segment data est "0x01000000" donc :
	* Adresse de base du tableau A : "0x01000000".
	* Adresse de base du tableau B : "0x01000080".
	* Adresse de base du tableau C : "0x01000100".
__Question C3 :__

L'instruction **sw** est placée après l'instruction **bne** à cause de delay slot du branchement. L'instruction qui suit un branchement est automatiquement exécuté que le branchement échoue ou pas.

__Question C4 :__

Il faut 7 cycle pour exécuter une itération de la boucle.

## Fonctionnement du cache instruction :

__Question D1 :__

* Le champ BYTE désigne un octet dans une ligne et est défini sur **4 bit**.
* Le champ SET définit un numéro de famille et est défini sur **3 bit**.
* Le champ TAG identifie une ligne particulière dans une famille et est défini sur **25 bit**.

Valeur de ces champs pour la première adresse du programme  :

	* BYTE	0b0000
	* SET	0b000
	* TAG	0x004000 & 0b0

__Question D2 :__

|  TAG   | V | WORD3 | WORD2 | WORD1 | WORD0 |
|--------|:-:|:-----:|:-----:|:-----:|------:|
| 0x0040 | 1 | addi  | addi  | lw    | lw    |
| 0x0040 | 1 |       | sw    | bne   | add   |

__Question D3 :__

Le contenu du cache à la fin de 20e itération est le même qu'a la première itération.
Le taux de miss est 5%.

__Question D4 :__

L'état *MISS_SELECT* est indispensable les caches de type set assiociatif (et full associatif) de niveau asssociatif 2 ou plus. En effet, dans ce type de cache, une ligne de cache n'a pas de case/emplacement prédéfini.

__Question D5 :__

	* A => IMISS and IUNC		   	
	* B => IMISS and not(IUNC) 		
	* C => not(IMISS) 			
	* I => Inconditionnelle			
	* G => IREQ and VALID and ERROR		
	* F => IREQ and VALID			
	* H => not(IREQ) or not(VALID)		
	* N => Inconditionnelle			
	* K => IREQ and VALID and ERROR		
	* L => IREQ and VALID and not(ERROR)	
	* J => not(IREQ) or not(VALID)		
	* M => Inconditionnelle			
	* O => Inconditionnelle 			|

__Question D6 :__

A l'activation du signal RESETN l'automate doit être forcé à l'état UNC_WAIT car au démarage de la machine, celle-ci ne fait confiance à personne et donc toutes les données sont non cachables.

## Fonctionnement du cache de données

__Question E1 :__

|  TAG   | V | WORD3 | WORD2 | WORD1 | WORD0 |
|--------|:-:|:-----:|:-----:|:-----:|------:|
|0x20002 | 1 | 104   |  103  |  102  |  101  |


__Question E2 :__

|  TAG   | V | WORD3 | WORD2 | WORD1 | WORD0 |
|--------|:-:|:-----:|:-----:|:-----:|------:|
|0x20002 | 1 | 104   |  103  |  102  |  101  |
|0x20002 | 1 | 108   |  107  |  106  |  105  |
|0x20002 | 1 | 112   |  111  |  110  |  109  |
|0x20002 | 1 | 116   |  115  |  114  |  113  |
|0x20002 | 1 | 120   |  119  |  118  |  117  |

__Question E3 :__

	* A => not(WRITE) and DMISS and DUNC
	* B => not(WRITE) and DMISS and not(DUNC)
	* C => not(WRITE) and not(DMISS)
	* I => Inconditionnelle
	* G => DREQ and VALID and ERROR
	* F => DREQ and VALID
	* H => not(DREQ) or not(VALID)
	* N => Inconditionnelle
	* K => DREQ and VALID and ERROR
	* L => DREQ and VALID and not(ERROR)
	* J => not(DREQ) or not(VALID)
	* M => Inconditionnelle
	* O => Inconditionnelle
	* D => not(DMISS) and WRITE
	* E => DMISS and WRITE 
	* P => Inconditionnelle

__Question E4 :__

La différence vient du fait qu'il faut vérifier si WOK est à '1' ou non, c'est à dire si le Write Buffer est plein ou non.

## Accès au Pibus

__Question F1 :__

Les écritures ont la priorité la plus élevée pour des raisons de consistance mémoire. En effet, les requêtes vers la mémoire doivent être faites dans l'ordre de requête. 
Ce qui induit que la résolution d'un Miss ne peut être réalisée que si le Buffer d'écriture est vide.

__Question F2 :__

Pour transmettre une requête de lecture vers le PIBUS_FSM, les deux automates ICACHE_FSM et DCACHE_FSM utilisent le mécanisme de tampon d'écriture protégé par une bascule RS (SET-RESET).

Pour prévenir les clients que la requête à bien été prise en compte, le serveur vide le tampon d'écriture (RESET la bascule).

