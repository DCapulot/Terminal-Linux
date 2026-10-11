# ❓ pwd --help

## O que faz

Mostra na tela um **resumo rápido** de como usar o `pwd`, com as opções disponíveis. Não executa o comando em si.

## Sintaxe

```bash
pwd --help
```

## Saída (versão do bash)

```bash
usuario@computador:~$ pwd --help
pwd: pwd [-LP]
    Print the name of the current working directory.

    Options:
      -L	print the value of $PWD if it names the current working
    		directory
      -P	print the physical directory, without any symbolic links

    By default, `pwd' behaves as if `-L' were specified.

    Exit Status:
    Returns 0 unless an invalid option is given or the current
    directory cannot be read.
```

## Como ler

| Trecho | Significado |
|---|---|
| `pwd [-LP]` | Aceita as opções `-L` e `-P` (os colchetes indicam que são opcionais) |
| `-L` | Mostra o valor de `$PWD` |
| `-P` | Mostra o caminho físico, sem links simbólicos |
| `By default ... -L` | Se você não passar opção, usa `-L` |
| `Exit Status` | Retorna `0` se deu certo; erro se a opção for inválida ou a pasta não puder ser lida |

## `--help` vs `man`

| | `pwd --help` | `man pwd` |
|---|---|---|
| Tamanho | Curto | Completo |
| Descreve | O `pwd` do **bash** | O `/bin/pwd` (coreutils) |
| Padrão informado | `-L` | `-P` |
| Como sair | Já volta ao prompt | Aperte `q` |

## Veja também

- [`pwd --version`](../opcao-version/README.md)
- [Voltar para o pwd](../README.md)
