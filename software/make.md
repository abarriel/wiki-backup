<!-- TITLE: Make -->
<!-- SUBTITLE: A quick summary of Make -->

# Introduction 
Le lien suivant est un passage obligatoire: https://fr.wikipedia.org/wiki/Make
Si vous avez l'âme d'un warrior: https://www.gnu.org/software/make/manual/make.html
# Bonnes pratiques
## Accélerer la compilation
Make dispose d'une option `-j [jobs], --jobs[=jobs]`. Le paramètre de l'option correspond au nombre de jobs et est facultatif. 
### Méthode 1: Utilisation basique
Lancer la commande `make -j` dans le dossier courant à la place de `make`. (pour automatiser, il est possible de set la variable d'environnement `MAKEFLAGS`)
Pour que ça fonctionne, il faut que les dépendances soit correctement gérés, notament dans l'appel de sous-makefile.
### Méthode 2: Utilisation intégré au makefile
Cette méthode permet de conserver l'appel du makefile sur `make`, sans y ajouter `-j`.
Modifiez la règle all comme suit:
```
all :
	$(MAKE) -C $(LIBFT_DIR)
	$(MAKE) -j $(NAME)
```
(cf https://www.gnu.org/software/make/manual/html_node/MAKE-Variable.html pour plus d'info sur `$(MAKE)` )
## Gestion des dépendances
- Comment compiler effica
## Comment éviter d'appeler `make re`  à chaque compilation
Le fonctionnement suivant doit être possible sans erreur:

