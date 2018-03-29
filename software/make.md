<!-- TITLE: Make -->
<!-- SUBTITLE: A quick summary of Make -->

# Introduction 
Le lien suivant est un passage obligatoire: https://fr.wikipedia.org/wiki/Make
Une documentation assez classique: http://gl.developpez.com/tutoriel/outil/makefile/
Un super poste sur le forum: https://forum.intra.42.fr/topics/85/messages?page=1#3640
Si vous avez l'âme d'un warrior: https://www.gnu.org/software/make/manual/make.html

Cette page ne reviens pas sur ces notions de bases. Vous devriez déjà être capable de faire un Makefile basique, qui ne relink pas (y compris sur l'affichage), et qui inclut les règles de bases (all, $(NAME), clean, fclean, re)
Si vous avez un doute sur `$@`, `$^`, `$<`... https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html
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
#### Pourquoi ?
La gestion des dépendances est utiles pour recompiler uniquement les binaires qui le nécessitent, sans faire appel a un `make re` long et fastidieux.
> Ex:
> Je make une mini libft, puis je modifie str.h qui est inclus par strlen.c, strcat.c mais pas atoi.c. Je relance make.
> - Si les dépendances ne sont pas gérées, il ne se passe rien. :warning:
> - Si les dépendances sont mal gérées, les trois .c et libft.a vont etre recompilés y compris atoi.c qui n'en a pas besoin. :snail:
> - Si les dépendances sont correctement gérées, strlen.c et stcat.c sont recompilés, mais pas atoi.c. libft.a est aussi recompilée. :thumbsup:

#### Tout savoir: 
La doc qui va bien: http://make.mad-scientist.net/papers/advanced-auto-dependency-generation/
#### TL;DR:
```
SRCS = foo.c bar.c ...
[...]
%.o: %.c
	$(CC) -MMD -c $< -o $@
[...]
-include $(SRCS:.c=.d)
```
## Arborescence des sources
Il est appréciable d'avoir ses **.o** (et **.d** si vous avez suivi la gestion de dépendances) séparés des **.c**. Voci comment avoir la même arborescence de fichiers dans le objs que dans srcs.

```
SRCS    = foo.c bar.c foo/bar.c foo/foo2/foo.c ...
SRC_DIR = srcs/
OBJ_DIR = objs/

OBJS    = $(addprefix $(OBJ_DIR), $(SRCS:.c=.o))

DIR     = $(sort $(dir $(OBJS)))

$(NAME): $(OBJS)
	$(CC) -c $^ -o $@

$(DIR):
	mkdir -p $@

$(OBJ_DIR)%.o: $(SRC_DIR)%.c | $(DIR)
	$(CC) -c $< -o $@
```

:warning: Si vous avez une gestion des dépendances, il faut modifier la ligne d'include pour qu'elle soit la suivante : `-include $(OBJS:.o=.d)` :warning:

> - Mais dis moi Jamy c'est quoi ce `|` dans ton Makefile ?
> - Bonne question Fred ! Il s'agit d'un pré-requis **order-only**. La règle est appelé seulement si le fichier n'existe pas. Dans le cas où le fichier existe (ici tous les dossiers et sous-dossiers objs), on ne regarde pas le timestamp.
> - Merci Jamy !

> FYI:
> - Pré-requis: https://www.gnu.org/software/make/manual/html_node/Prerequisite-Types.html
> - Utilisation de sort: https://www.gnu.org/software/make/manual/html_node/Text-Functions.html#index-sorting-words
## Divers
il peut être judicieux de mettre le Makefile en dépendances des règles de compilation. Ainsi, on assure qu'un changement manuel dans le Makefile sera immédiatement répercuté. Cela se fait de la façon suivante :
```
$(NAME): $(OBJS) Makefile
	[...]
%.o: %.c Makefile
	[...]
```