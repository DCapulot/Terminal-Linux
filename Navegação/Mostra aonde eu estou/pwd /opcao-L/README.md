# 🔗 pwd -L

> **L** de *logical* (lógico)

## O que faz

Mostra o caminho **lógico** da pasta atual: usa o valor da variável `$PWD`, **mantendo os links simbólicos** do jeito que você navegou até lá.

Forma longa: `pwd --logical`

## Sintaxe

```bash
pwd -L
```

## Exemplo simples

```bash
usuario@computador:~$ pwd -L
/home/usuario
```

Sem links simbólicos no caminho, o resultado é igual ao `pwd` normal.

## Exemplo com link simbólico

```bash
mkdir real          # pasta de verdade
ln -s real link     # atalho (link simbólico) para ela
cd link             # entra pelo atalho

pwd -L
```

Saída:

```
/home/usuario/link
```

O `-L` mostra o caminho **como você entrou** (`link`), e não onde a pasta realmente está (`real`).

## Quando usar

- Quando você quer ver o caminho **do jeito que navegou**
- É o padrão do `pwd` no **bash**, então `pwd` e `pwd -L` dão o mesmo resultado

## Veja também

- [`pwd -P`](../opcao-P/README.md): o oposto, mostra o caminho físico real
- [Voltar para o pwd](../README.md)
