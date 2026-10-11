# 📍 pwd

> **p**rint **w**orking **d**irectory: mostra em qual pasta você está.

## O que faz

O `pwd` mostra o **caminho completo (absoluto) da pasta atual**, a partir da raiz (`/`). É o comando para responder: *"onde eu estou agora?"*

## Sintaxe

```bash
pwd [OPÇÃO]
```

## Exemplo

![Terminal executando o comando pwd](img/pwd-terminal.png)

```bash
usuario@computador:~$ pwd
/home/usuario
```

O `~` no prompt é um atalho para a pasta *home* do usuário. Por isso o `pwd` mostra `/home/usuario`.

## 🧩 Opções (cada uma tem seu README)

| Opção | Nome | Resumo | Documentação |
|---|---|---|---|
| `-L` | *logical* | Caminho lógico, mantém os links simbólicos | [opcao-L](opcao-L/README.md) |
| `-P` | *physical* | Caminho físico, resolve os links simbólicos | [opcao-P](opcao-P/README.md) |
| `--help` | ajuda | Mostra um resumo de uso | [opcao-help](opcao-help/README.md) |
| `--version` | versão | Mostra a versão (só no `/bin/pwd`) | [opcao-version](opcao-version/README.md) |

## Duas versões do pwd

Existem **duas** versões do comando, e elas têm padrões diferentes:

| Versão | Onde está | Padrão |
|---|---|---|
| **Embutida no bash** (*builtin*) | dentro do próprio shell | `-L` |
| **GNU coreutils** | `/bin/pwd` | `-P` |

O comando `pwd` digitado normalmente usa a versão do **bash**. Para chamar a outra, use o caminho completo: `/bin/pwd`.

Para ver qual está sendo usada:

```bash
type -a pwd
```

> É por isso que a `man pwd` (que descreve o `/bin/pwd`) e o `pwd --help` (do bash) mostram padrões diferentes.

## Combinando opções

```bash
pwd -LP    # vale a última: -P
pwd -PL    # vale a última: -L
```

## Argumentos sem hífen são ignorados

```bash
usuario@computador:~$ pwd l
/home/usuario
usuario@computador:~$ pwd _p
/home/usuario
```

Como `l` e `_p` não começam com `-`, o `pwd` não os trata como opção e mostra o caminho normal.

## Variável relacionada: `$PWD`

A pasta atual também fica guardada na variável de ambiente `$PWD`:

```bash
echo $PWD
```

## Para consultar o manual

```bash
man pwd      # aperte q para sair
```

## 🐛 Erros comuns

| Comando | Erro | Causa | Solução |
|---|---|---|---|
| `pwd -p` | `bash: pwd: -p: invalid option` | Linux diferencia maiúscula de minúscula | Usar `-P` |
| `pwd --version` | `invalid option` | O `pwd` do bash não tem `--version` | Usar `/bin/pwd --version` |

## 📝 Observações

- O `pwd` **não altera nada**, só mostra informação. É seguro usar à vontade.
- Muito útil em **scripts** para saber de qual pasta eles estão rodando.

[⬅ Voltar para 01-basico](../README.md)
