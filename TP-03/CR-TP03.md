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

	|-------|----------------:|
	| BYTE	| 0b0000 	  |	
	| SET	| 0b000 	  |
	| TAG	| 0x004000 & 0b0  |  

__Question D2 :__

	|  TAG   | V | WORD3 | WORD2 | WORD1 | WORD0 |
	|--------|:-:|:-----:|:-----:|:-----:|------:|
	| 0x0040 | 1 | addi  | addi  | lw    | lw    |
	| 0x0040 | 1 |       | sw    | bne   | add   |

