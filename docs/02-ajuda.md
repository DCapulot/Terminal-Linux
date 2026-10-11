# 02 · Ajuda

Como descobrir o que um comando faz sem sair do terminal.

| Comando | Resumo |
|---------|--------|
| [`--help`](#--help) | Resumo rápido de uso de um comando |
| [`man`](#man) | Manual completo |

---

## `--help`

**Para que serve:** mostra um resumo de uso e das opções de um comando.

**Como usar**

```bash
comando --help
```

```bash
ls --help
cp --help | less     # paginado, para textos longos
```

**Como se comporta**

- Imprime direto no terminal e volta ao prompt (sem paginação).
- É rápido, mas resumido. Para detalhes, use `man`.
- Funciona na maioria dos programas GNU. Comandos internos do bash (como `cd`) usam `help cd`.
- Alguns programas aceitam `-h` ou `-?` no lugar.

**Variações**

| Forma | O que faz |
|-------|-----------|
| `comando --help` | Ajuda do comando |
| `comando -h` | Ajuda curta (em alguns programas) |
| `help comando` | Ajuda de comandos internos do bash (`cd`, `echo`, `type`...) |
| `comando --version` | Mostra a versão |

---

## `man`

**Para que serve:** abre o manual completo de um comando. Significa *manual*.

**Como usar**

```bash
man [opções] [seção] comando
```

```bash
man ls          # manual do ls
man 5 passwd    # manual do arquivo /etc/passwd (seção 5)
man -k copiar   # busca páginas cujo nome/descrição contém "copiar"
```

**Como se comporta**

- Abre o manual em um paginador (normalmente `less`). Para sair, pressione `q`.
- Cada página tem seções: NAME, SYNOPSIS, DESCRIPTION, OPTIONS, EXAMPLES, SEE ALSO etc.
- Se existe mais de uma página com o mesmo nome (como `passwd`), mostra a de menor seção, a não ser que você indique a seção.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-k palavra` | Busca a palavra nos nomes e descrições (igual a `apropos`) | `man -k network` |
| `-f comando` | Descrição de uma linha (igual a `whatis`) | `man -f ls` |
| `-a` | Mostra todas as páginas com esse nome, uma após a outra | `man -a passwd` |
| `-w` | Mostra o caminho do arquivo da página | `man -w ls` |
| `-L idioma` | Escolhe o idioma, se instalado | `man -L pt_BR ls` |

**Seções do manual**

| Seção | Conteúdo |
|-------|----------|
| 1 | Comandos de usuário |
| 2 | Chamadas de sistema |
| 3 | Funções de biblioteca (C) |
| 4 | Arquivos de dispositivo |
| 5 | Formatos de arquivo e configuração (ex.: `passwd`, `sudoers`) |
| 6 | Jogos |
| 7 | Diversos |
| 8 | Comandos de administração (ex.: `useradd`) |

**Navegando dentro do `man`**

| Tecla | Ação |
|-------|------|
| `Espaço` / `b` | Página abaixo / acima |
| `↑` `↓` | Linha por linha |
| `/texto` | Busca para frente |
| `n` / `N` | Próxima / anterior ocorrência |
| `g` / `G` | Início / fim |
| `h` | Ajuda do paginador |
| `q` | Sai |
