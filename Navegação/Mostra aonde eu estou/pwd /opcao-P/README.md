# 📂 pwd -P

> **P** de *physical* (físico)

## O que faz

Mostra o caminho **físico real** da pasta atual, **resolvendo todos os links simbólicos**.

Forma longa: `pwd --physical`

## Sintaxe

```bash
pwd -P
```

## Exemplo simples

```bash
usuario@computador:~$ pwd -P
/home/usuario
```

Sem links simbólicos no caminho, o resultado é igual ao `pwd` normal.

## Exemplo com link simbólico

```bash
mkdir real          # pasta de verdade
ln -s real link     # atalho (link simbólico) para ela
cd link             # entra pelo atalho

pwd -P
```

Saída:

```
/home/usuario/real
```

O `-P` mostra **onde a pasta realmente está** (`real`), ignorando o atalho (`link`) usado para entrar.

## Quando usar

- Quando você precisa saber a **localização real** da pasta
- Em **scripts**, para evitar confusão com atalhos
- É o padrão do `/bin/pwd` (versão GNU coreutils)

## ⚠️ Atenção: maiúsculo!

O Linux diferencia maiúsculas de minúsculas:

```bash
usuario@computador:~$ pwd -p
bash: pwd: -p: invalid option
pwd: usage: pwd [-LP]
```

`-P` (maiúsculo) funciona, `-p` (minúsculo) **não existe**.

## Veja também

- [`pwd -L`](../opcao-L/README.md): o oposto, mostra o caminho lógico
- [Voltar para o pwd](../README.md)
