# 07 · Permissões

Quem pode ler, escrever e executar cada arquivo.

| Comando | Resumo |
|---------|--------|
| [`chmod o+w`](#chmod-ow) | Altera permissões usando letras (modo simbólico) |
| [`chmod 740`](#chmod-740) | Altera permissões usando números (modo octal) |
| [`chown`](#chown) | Altera dono e grupo |

---

## Entendendo as permissões

O `ls -l` mostra as permissões de cada arquivo:

```
-rwxr-xr-- 1 maria devs 4096 out 10 09:30 script.sh
```

Os 10 primeiros caracteres se dividem assim:

```
-   rwx   r-x   r--
│    │     │     │
│    │     │     └─ outros (o): leitura
│    │     └─ grupo (g): leitura e execução
│    └─ dono (u): leitura, escrita e execução
└─ tipo: - arquivo, d diretório, l link
```

| Permissão | Letra | Valor | Em arquivo | Em diretório |
|-----------|-------|-------|------------|--------------|
| Leitura | `r` | 4 | Ler o conteúdo | Listar o conteúdo (`ls`) |
| Escrita | `w` | 2 | Alterar o conteúdo | Criar/apagar/renomear itens dentro |
| Execução | `x` | 1 | Executar como programa | Entrar (`cd`) e acessar itens |
| Nenhuma | `-` | 0 | | |

A soma dos valores forma um dígito: `rwx` = 4+2+1 = **7**, `r-x` = 4+0+1 = **5**, `r--` = **4**, `---` = **0**.

| Octal | Permissão | Octal | Permissão |
|-------|-----------|-------|-----------|
| 0 | `---` | 4 | `r--` |
| 1 | `--x` | 5 | `r-x` |
| 2 | `-w-` | 6 | `rw-` |
| 3 | `-wx` | 7 | `rwx` |

---

## `chmod o+w`

**Para que serve:** altera permissões no **modo simbólico**: você diz *para quem*, *se adiciona ou remove* e *qual permissão*. `chmod` vem de *change mode*.

`o+w` = **o**utros + dar permissão de **w**rite (escrita).

**Como usar**

```bash
chmod [opções] modo arquivo...
```

```bash
chmod o+w arquivo.txt        # outros ganham escrita
chmod u+x script.sh          # dono ganha execução
chmod g-w arquivo.txt        # grupo perde escrita
chmod a+r arquivo.txt        # todos ganham leitura
chmod u=rwx,g=rx,o=r f.sh    # define tudo de uma vez
chmod -R g+w pasta           # recursivo
```

**Como se comporta**

- Só o **dono** do arquivo (ou o root) pode mudar suas permissões.
- `+` e `-` alteram **apenas** o que foi indicado e mantêm o resto; `=` define exatamente aquelas permissões e remove as demais.
- Dar `o+w` deixa **qualquer usuário do sistema** alterar o arquivo. Isso raramente é uma boa ideia (principalmente em scripts e arquivos de configuração).
- Em diretórios, `x` é necessário para entrar neles.

**Sintaxe: `[quem][operador][permissões]`**

| Parte | Valores | Significado |
|-------|---------|-------------|
| Quem | `u` | Dono (*user*) |
| | `g` | Grupo |
| | `o` | Outros (*others*) |
| | `a` | Todos (*all*) = `ugo` (padrão se omitido) |
| Operador | `+` | Adiciona |
| | `-` | Remove |
| | `=` | Define exatamente |
| Permissão | `r` `w` `x` | Leitura, escrita, execução |
| | `X` | Execução só se for diretório ou já tiver `x` em alguém |
| | `s` | setuid/setgid |
| | `t` | *sticky bit* |

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-R` | Recursivo: aplica em diretórios e conteúdo | `chmod -R u+rwX pasta` |
| `-v` | Mostra todos os arquivos processados | `chmod -v o-w x` |
| `-c` | Mostra só os que mudaram | `chmod -c o-w x` |
| `-f` | Silencia mensagens de erro | `chmod -f o-w x` |
| `--reference=ref` | Copia as permissões de outro arquivo | `chmod --reference=a b` |

---

## `chmod 740`

**Para que serve:** altera permissões no **modo octal (numérico)**: três dígitos, um para dono, um para grupo e um para outros.

**Como usar**

```bash
chmod 740 arquivo.txt
```

**Como se comporta**

`740` é lido dígito a dígito:

| Dígito | Quem | Cálculo | Permissão |
|--------|------|---------|-----------|
| 7 | Dono (u) | 4+2+1 | `rwx`: ler, escrever, executar |
| 4 | Grupo (g) | 4 | `r--`: só ler |
| 0 | Outros (o) | 0 | `---`: nada |

Resultado no `ls -l`: `-rwxr-----`

- Ao contrário do modo simbólico, o octal **substitui todas** as permissões de uma vez.
- Dá para escrever com 4 dígitos para incluir bits especiais (`chmod 1777 /tmp`).

**Valores comuns**

| Modo | Símbolos | Uso típico |
|------|----------|------------|
| `600` | `rw-------` | Arquivos privados (chaves SSH) |
| `644` | `rw-r--r--` | Arquivos comuns |
| `640` | `rw-r-----` | Dono edita, grupo lê, outros nada |
| `700` | `rwx------` | Diretório/script só do dono |
| `740` | `rwx r-- ---` | Dono total, grupo lê |
| `750` | `rwxr-x---` | Dono total, grupo lê e executa |
| `755` | `rwxr-xr-x` | Programas e diretórios públicos |
| `777` | `rwxrwxrwx` | Todos podem tudo (**evite**) |

**Bits especiais (1º dígito, opcional)**

| Valor | Nome | Efeito |
|-------|------|--------|
| 4 | setuid | Executa com os privilégios do **dono** do arquivo |
| 2 | setgid | Executa com os do grupo; em diretório, novos arquivos herdam o grupo |
| 1 | sticky | Em diretório, só o dono do arquivo pode apagá-lo (ex.: `/tmp`) |

**Opções:** as mesmas do `chmod` (veja a tabela em [`chmod o+w`](#chmod-ow)).

---

## `chown`

**Para que serve:** altera o **dono** e/ou o **grupo** de arquivos e diretórios. Significa *change owner*.

**Como usar**

```bash
sudo chown [opções] dono[:grupo] arquivo...
```

```bash
sudo chown maria arquivo.txt             # novo dono
sudo chown maria:devs arquivo.txt        # novo dono e novo grupo
sudo chown :devs arquivo.txt             # só o grupo
sudo chown -R maria:maria /home/maria    # recursivo
```

**Como se comporta**

- Normalmente só o **root** pode mudar o dono (use `sudo`). O dono comum pode, no máximo, trocar o grupo para um grupo do qual faça parte (com `chgrp`).
- Aceita nome ou número (UID/GID).
- Alterar dono não muda as permissões, só a quem as regras de "dono" se aplicam.
- Com `-R` em diretório, afeta tudo dentro. Cuidado com o caminho para não alterar o sistema todo.
- Por padrão não segue links simbólicos recursivamente; use `-h` para alterar o próprio link.

**Formas de usar o `dono:grupo`**

| Forma | Efeito |
|-------|--------|
| `chown maria arq` | Só o dono |
| `chown maria:devs arq` | Dono e grupo |
| `chown maria: arq` | Dono e grupo = grupo de login da maria |
| `chown :devs arq` | Só o grupo |

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-R` | Recursivo | `chown -R maria pasta` |
| `-v` | Mostra todos os arquivos processados | `chown -v maria x` |
| `-c` | Mostra só os que mudaram | `chown -c maria x` |
| `-h` | Altera o link simbólico em vez do alvo | `chown -h maria link` |
| `--from=dono:grupo` | Só altera se o dono/grupo atual forem esses | `chown --from=joao maria x` |
| `--reference=ref` | Copia dono e grupo de outro arquivo | `chown --reference=a b` |

**Comandos relacionados**

| Comando | O que faz |
|---------|-----------|
| `chgrp grupo arq` | Altera só o grupo |
| `umask` | Define as permissões padrão dos novos arquivos |
| `ls -l` | Mostra dono, grupo e permissões |
| `stat arq` | Mostra permissões em octal e simbólico |
