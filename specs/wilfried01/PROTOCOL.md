# 1. Introduction

## 1.1 Objectifs du protocole
Le but de se protocole est de spécifier les règles de communication entre une application serveur et des applications clients qui s'y connectent afin d'effectuer des opérations arithmétiques. Lors de la connexion d'un client, le serveur le renseigne sur la liste des opérations qui peuvent être réalisées au moyen de ce protocole. Toutes les opérations spécifier ici doivent être réaliser par les serveurs. Il peut cependant offrir des opérations en plus. Le protocole est ligne par ligne.

## 1.2 Fonctionnement général
Le procole **TCP** est le protocole de transport utilisé pour ce protocole. Le port de communication par défaut est le port 3705.

Pour exécuter une opération, le client envoie un message de a liste des opérations renseignées lors de sa connexion précéder du mot clé **EXECUTE**. 

Si le message reçu du client est correct, le serveur exécute l'opération et retourne le résultat avec un message **RESULT** suivi du résultat de l'opération. Si par contre le message est incorrect, le serveur renvoie un message **ERROR** suivi de l'erreur observée.

Les erreurs admissibles sont les suivantes :

- **INCORRECT_SYNTAX** : Si la syntaxe de l'opération à réaliser n'est pas correcte
- **UNKNOW_OPERATION** : Si l'opération à réaliser ne fait pas partie des opération admissible
- **INCORRECT_OPERANDES** : Si un des opérandes n'est pas un nombre ou un chiffre.

Le nom des opérations à réaliser est sensible à la case.

Pour mettre un terme à une connexion, le client envoie une requête **QUIT**.

Le protocole est **sans état**. De plus les serveurs et les clients utilisent l'encodage **ASCII** et la séquence **CRLF** pour indiquer la fin d'une ligne.

# Syntaxe des messages
Les messages suivants sont susceptibles d'être échangés entre un client et un serveur :

- **WELCOME** : Envoyer par le serveur au client lorsque celui-ci se connecte à l'application. Pour chaque opération, il est nécessaire de spécifier le nombre d'opérandes nécessaires ainsi que la description de l'opération.
Exemple : 
> WELCOME CRLF
> 
> List available operations CRLF
> - ADD 2 Additionne 2 nombres CRLF
> - MULT 2 Multiplie 2 nombres CRLF
> - SQRT 1 Retourne la racine carré d'une nombre CRLF
> END CRLF

- **COMPUTE** : Envoyer par le client au serveur pour demander à celui-ci d'effectuer une opération. Pour chaque opération, il faut spécifier le nombre d'opérandes nécessaires. Si ce critère n'est pas respecter, le serveur retourne une erreur.
Exemple : 
> - COMPUTE ADD 3 5 CRLF
> - COMPUTE MULT 5 7 CRLF
> - COMPUTE SQRT 25 CRLF

- **RESULT** : Réponse du serveur au client si la requête **COMPUTE** précédente est correcte. La valeur du résultat est indiquée.
Exemple :
> - Le client envoie COMPUTE ADD 5 4 CRLF
> - Le serveur répond RESULT 9 CRLF
> - Le client envoie SQRT 36 CRLF
> - Le serveur répond RESULT 6 CRLF

- **ERROR** : Réponse du serveur au client si la requête **COMPUTE** précédente est incorrecte. Le serveur renvoie toujours une seule erreur (même si la requête pourrait en contenir plusieurs). Il n'y a pas de priorité dans les erreurs.
Exemple :
> - Le client envoie COMPUTE ADD 4 + 5 CRLF
> - Le serveur répond ERROR INCORRECT_SYNTAX CRLF
> - Le client envoie COMPUTE SQRT Trente-six CRLF
> - Le serveur répond ERROR INCORRECT_OPERANDES CRLF
> - Le client envoie COMPUTE MUL 4 * 5 CRLF
> - Le serveur répond ERROR UNKNOW_OPERATION CRLF
> - ou Le serveur répond ERROR INCORRECT_SYNTAX CRLF

- **QUIT** : Envoyer par le client pour terminer sa connexion. La syntaxe est toujours la même.
> - QUIT CRLF


