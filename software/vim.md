<!-- TITLE: Vim -->
<!-- SUBTITLE: A quick summary of Vim -->

# syntaxe 42
la norme impose de préfixer `t_` aux typedefs.

```
autocmd BufEnter * syntax match cType "\<[t]_\w\+\>"
```