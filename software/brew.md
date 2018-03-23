<!-- TITLE: Brew -->
<!-- SUBTITLE: A quick summary of Brew -->

## Installation
	rm -rf $HOME/.brew && git clone --depth=1 https://github.com/Homebrew/brew $HOME/.brew && export PATH=$HOME/.brew/bin:$PATH && brew update && echo "export PATH=$HOME/.brew/bin:$PATH" >> ~/.zshrc
## Utilisation
`brew install <name>`
## trouver un binaire

vous pouvez utiliser le site http://formulae.brew.sh pour rechercher une formule plutôt que d'essayer `brew install xxx` aléatoirement