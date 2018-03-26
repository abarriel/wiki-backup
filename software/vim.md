<!-- TITLE: Vim -->
<!-- SUBTITLE: A quick summary of Vim -->

# syntaxe 42
la norme impose de préfixer `t_` aux typedefs, pour incorper cette règle à vim:

```
autocmd BufEnter * syntax match cType "\<[t]_\w\+\>"
```