
# ZAZA - Reverse Challenge
Pour mon premier CTF, j'ai pu réaliser un challenge de reverse engineering. Je vais vous expliquer ma démarche pour résoudre ce challenge dans cette write-up.

![chall](https://cdn.discordapp.com/attachments/1022274352550518857/1100424938428575784/Sans_titre.png)

## Analyse du binaire
Dans un premier temps, nous pouvons supposer au vu de l'instance remote que la fin du challenge se fera à distance. Nous allons à présent lancer le binaire dans IDA.

![main func](https://cdn.discordapp.com/attachments/1022274352550518857/1100424672828456990/Sans_titre.png)

Voici le code décompiléde la fonction main.
Le programme va d'abord demander une entrée utilisateur via la fonction scanf.
Cette entrée va être comparée a la valeur 0x1337 soit 4919. Puis il va nous proposer une seconde entrée qui va multiplier notre nombre par notre précédente entrée.
Si le résultat n'est pas égal à 1, alors nous pouvons avancer dans le programme.

## Crypto is Nice

Avant de faire ce challenge, je n'avais pas trop fait de cryptographie, j'ai pu apprendre a utiliser le XOR.
Pour rappel, voici la table de vérité XOR : 

![xor table](https://www.build-electronic-circuits.com/wp-content/uploads/2022/09/Truth-table-XOR-gate.png)
Cette partie est un peu plus corsée.
Encore une fois nous allons avoir une entrée utilisateur, celle-ci va être passée a la fonction XOR : 
![xor func](https://cdn.discordapp.com/attachments/1022274352550518857/1100424346046050374/Sans_titre.png)

Notre entrée xorée, va être comparé a une chaine de caractère semblant être elle aussi xorée.
Nous supposons à présent que la chaine qui va être comparée à notre entrée est notre "cipher text".
C'est à dire, que cette chaine a déjà été chiffrée, par la fonction XOR.
Nous allons donc devoir la déchiffrer pour passer cette étape.

## Dev&Flag

Nous allons utiliser le langage python qui est assez simple d'utilisation pour faire un script de résolution.
J'ai refais la fonction de XOR, cette fois-ci en prenant, notre "cipher text" en argument, afin de le déchiffrer.
```py
key = "anextremelycomplicatedkeythatisdefinitelyuselessss"
cipher = "2& =$!-( <*+*( ?!&$$6,. )' $19 , #9=!1 <*=6 <6;66#"
mdp = ""
longueur_clé = len(key)
print("[+] Longueur de la clé =", longueur_clé)
for i in range(1337):
	if (i >= longueur_clé):
		break
	mdp += chr(ord(cipher[i]) ^ ord(key[i]))
print("[+] Le mot de passe est ", mdp)
```


Nous avons plus qu'à respecter toutes les conditions et nous aurons le flag.

![flag](https://cdn.discordapp.com/attachments/1022274352550518857/1100424145398931527/Sans_titre.png)
