# 🏷️ pwd --version

## O que faz

Mostra a **versão** do programa e informações de licença.

> ⚠️ Só funciona no **`/bin/pwd`** (GNU coreutils). O `pwd` embutido no bash **não tem** essa opção.

## Sintaxe

```bash
/bin/pwd --version
```

## Exemplo

```bash
$ /bin/pwd --version
pwd (GNU coreutils) 9.4
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
```

*(O número da versão muda conforme o seu sistema. Na `man` da aula aparece a 8.28.)*

## Com o `pwd` do bash

```bash
$ pwd --version
bash: pwd: --: invalid option
pwd: usage: pwd [-LP]
```

O `pwd` do bash só conhece `-L` e `-P`, então dá erro.

## Por que isso acontece?

Existem duas versões do `pwd`:

| Versão | Comando | Aceita `--version`? |
|---|---|---|
| Embutida no bash | `pwd` | ❌ |
| GNU coreutils | `/bin/pwd` | ✅ |

Veja a diferença completa no [README do pwd](../README.md).

## Veja também

- [`pwd --help`](../opcao-help/README.md)
- [Voltar para o pwd](../README.md)
