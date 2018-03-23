<!-- TITLE: Qemu -->
<!-- SUBTITLE: A quick summary of Qemu -->

[Page Officielle](https://www.qemu.org/)
# Installation
`brew install qemu`, ceci dit je n'ai jamais essayé `qemu` sur les macs de l'école, a la maison j'ai du activer une option de virtualisation dans le bios avec linux donc je doute que ca marche facilement à l'école.

# astuces KFS
certaines options s'avèrent très utiles pour débugguer la série de projets [KFS](/projects/KFS)

- `-gdb dev`
	 - e.g. `gdb tcp::4242` pour connecter via tcp, marche aussi en UDP
	 - e.g. `(gdb) target remote | exec qemu-system-i386 -gdb stdio ...` pour une connection en pseudo-tty
- `--enable-kvm` accéleration hardware de la virtualisation sur linux
- `-monitor dev` redigire le monitor graphique vers dev
	- e.g. `-monitor telnet:127.0.0.1:4343`
- `-curses` sortie graphique dnas le terminal remplace la fenêtre GUI par défault