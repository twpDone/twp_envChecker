twp_envChecker
==============

Environment checker

http://twpdone.github.io/twp_envChecker
FR

Tapez checkEnv -h pour afficher l'aide.

Vous pouvez lancer les verification une par une:<br>
`checkEnv -NomVerif`

Ou toutes à la fois <br>
`checkEnv -all`


Options facultatives<br>
`-eo` : Error only :Seules les erreurs s'affichent<br>
`-nc` : No color : enleve la coloration syntaxique dans la sortie console, utile si vous souhaitez sauvegarder la sortie dans un fichier<br>
`-html` : Produit une sortie html <br>
		Rediriger la sortie vers un fichier la sortie pour sauvegarder vos resultats sous format html<br>
`-file fileName` : Execute les verifications que vos avez definies dans le fichier fileName<br>

Pour avoir de l'aide pour creer votre fichier de configuration : <br>
`checkEnv -howToConfig`

Cette option liste les verifications configurables depuis le fichier de config.

	"Le format du fichier de configuration est simple
	 Le format de chaque ligne doit etre :
		command:argument[:argument2][:argument3][:...]
	
	..."
	
Exemple de fichier de config :<br>
```
	sshd:/etc/ssh/sshd_config:PermitRootLogin:yes
	userExist:root:system
	fileOwnership:/bin:bin:bin
```

/!\ attention pas d'espaces en debut de ligne, sinon la commande ne sera pas executee

## DEV CORNER

Pour developper la suite:

Affichage standard avec la fonction "normal", du message de succes de la verification avec "success", et message d'echec de la verification avec "fail".<br>
Ces fonctions sont a utiliser comme echo

Pensez a mettre a jour les fonctions usage, cherckArg, howTo .

Faire une fonction qui fait un check sur un point precis (un user, un fichier, un fs, ...)<br>
Cette fonction prend des Arguments.

Faire la fonction de check par defaut.<br>
Cette fonction appellera autant de fois que necessaire la fonction precedente (fonction de check unitaire)<br>
Cette fonction pourra eventuellement lire dans le fichier $CONFIGFILE les regles qui matchent a son nom : <br>

FunctionName etant le nom de la fonction de test unitaire<br>

```
	if test -f $CONFIGFILE
	then
		success $Cyan"*************************"$Off
		success $Cyan"Reading from "$CONFIGFILE$Off
		success $Cyan"*************************"$Off
		functionNameLines=`cat $CONFIGFILE | egrep "^functionName"`
		for functionNameLine in $functionNameLines
		do
			# les arguments sont separes par ":"
			functionNameArg1=`echo $functionNameLine | cut -d ":" -f 2`
			functionNameArg2=`echo $functionNameLine | cut -d ":" -f 3`
			
			functionName $functionNameArg1 $functionNameArg2
		done
	else
		# si $CONFIGFILE n'existe pas
		# traitement par default
	fi
```

