# 03 · Arquivos

Criar, copiar, ler, escrever e remover arquivos e diretórios.

| Comando | Resumo |
|---------|--------|
| [`touch`](#touch) | Cria arquivo vazio ou atualiza datas |
| [`mkdir`](#mkdir) | Cria diretórios |
| [`rm`](#rm) | Remove arquivos e diretórios |
| [`cat`](#cat) | Mostra/concatena o conteúdo de arquivos |
| [`cp`](#cp) | Copia arquivos e diretórios |
| [`echo`](#echo) | Imprime texto |
| [`>` e `>>`](#e--redirecionamento) | Grava a saída de um comando em arquivo |
| [`head`](#head) | Mostra o início de um arquivo |
| [`tail`](#tail) | Mostra o fim de um arquivo |
| [`nano`](#nano) | Editor de texto no terminal |

---

## `touch`

**Para que serve:** cria um arquivo vazio. Se o arquivo já existe, só atualiza as datas de acesso/modificação.

**Como usar**

```bash
touch [opções] arquivo...
```

```bash
touch notas.txt              # cria notas.txt vazio
touch a.txt b.txt c.txt      # cria vários de uma vez
touch -c notas.txt           # só atualiza a data; não cria se não existir
```

**Como se comporta**

- Arquivo inexistente: cria vazio (0 bytes).
- Arquivo existente: **não** altera o conteúdo, apenas a data.
- Não cria diretórios intermediários: `touch pasta/a.txt` falha se `pasta` não existir.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-c` | Não cria o arquivo se ele não existir | `touch -c x.txt` |
| `-a` | Altera só a data de acesso | `touch -a x.txt` |
| `-m` | Altera só a data de modificação | `touch -m x.txt` |
| `-t [[CC]AAMMDDhhmm]` | Define data/hora específica | `touch -t 202610110930 x.txt` |
| `-d "texto"` | Define data a partir de texto | `touch -d "yesterday" x.txt` |
| `-r ref` | Copia as datas de outro arquivo | `touch -r a.txt b.txt` |

---

## `mkdir`

**Para que serve:** cria diretórios. Significa *make directory*.

**Como usar**

```bash
mkdir [opções] diretório...
```

```bash
mkdir projetos
mkdir -p projetos/linux/scripts     # cria toda a cadeia
mkdir -m 700 privado                # cria com permissão 700
```

**Como se comporta**

- Falha se o diretório já existe (a não ser que use `-p`).
- Falha se o diretório pai não existe (a não ser que use `-p`).
- Com `-p`, não dá erro se já existir e cria os pais que faltam.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-p` | Cria os diretórios pai se necessário; sem erro se já existir | `mkdir -p a/b/c` |
| `-v` | Mostra uma mensagem para cada diretório criado | `mkdir -v pasta` |
| `-m modo` | Define as permissões na criação | `mkdir -m 755 pasta` |

---

## `rm`

**Para que serve:** remove arquivos e diretórios. Significa *remove*.

> ⚠️ **Não há lixeira.** O que o `rm` apaga não volta. Confira o caminho antes de apertar Enter.

**Como usar**

```bash
rm [opções] arquivo...
```

```bash
rm antigo.txt              # remove um arquivo
rm -i *.log                # pergunta antes de cada remoção
rm -r pasta                # remove diretório e conteúdo
rm -d pasta_vazia          # remove só diretório vazio
```

**Como se comporta**

- Sem `-r`, recusa remover diretórios.
- Sem `-f`, pergunta quando o arquivo está protegido contra escrita.
- Remove a entrada no sistema de arquivos e não envia para lixeira.
- Curingas (`*`) são expandidos pelo shell **antes** do comando rodar; `rm *` apaga tudo no diretório atual.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-r`, `-R` | Recursivo: remove diretórios e tudo dentro | `rm -r pasta` |
| `-f` | Força: não pergunta e ignora arquivos inexistentes | `rm -f x.txt` |
| `-i` | Pergunta antes de cada remoção | `rm -i x.txt` |
| `-I` | Pergunta uma vez se for remover mais de 3 arquivos ou recursivo | `rm -I *` |
| `-d` | Remove diretórios vazios | `rm -d pasta` |
| `-v` | Mostra o que foi removido | `rm -v x.txt` |
| `--preserve-root` | Recusa operar recursivamente em `/` (já é padrão) | |

---

## `cat`

**Para que serve:** mostra o conteúdo de arquivos e concatena vários. Vem de *concatenate*.

**Como usar**

```bash
cat [opções] [arquivo...]
```

```bash
cat notas.txt                  # mostra o arquivo
cat a.txt b.txt                # mostra a seguido de b
cat a.txt b.txt > junto.txt    # junta em um novo arquivo
cat -n script.sh               # com número de linhas
```

**Como se comporta**

- Imprime o arquivo inteiro de uma vez; para arquivos grandes, prefira `less`, `head` ou `tail`.
- Sem argumentos, lê o teclado (entrada padrão) até `Ctrl+D`.
- Em arquivo binário, o terminal pode mostrar caracteres estranhos.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-n` | Numera todas as linhas | `cat -n x.txt` |
| `-b` | Numera só linhas não vazias | `cat -b x.txt` |
| `-s` | Comprime linhas vazias repetidas em uma | `cat -s x.txt` |
| `-E` | Mostra `$` no fim de cada linha | `cat -E x.txt` |
| `-T` | Mostra tabs como `^I` | `cat -T x.txt` |
| `-A` | Mostra tudo, incluindo caracteres invisíveis | `cat -A x.txt` |

---

## `cp`

**Para que serve:** copia arquivos e diretórios. Significa *copy*.

**Como usar**

```bash
cp [opções] origem destino
cp [opções] origem... diretório_destino/
```

```bash
cp a.txt b.txt                 # copia a.txt para b.txt
cp a.txt pasta/                # copia para dentro de pasta
cp -r pasta pasta_backup       # copia diretório inteiro
cp -a /etc/nginx /backup/      # cópia preservando tudo
```

**Como se comporta**

- Se o destino é um arquivo existente, ele é **sobrescrito sem aviso** (a não ser que use `-i`).
- Se o destino é um diretório, a cópia vai para dentro dele.
- Para copiar diretórios é obrigatório `-r` (ou `-a`).
- Por padrão a cópia recebe a data atual e o dono é quem executou; `-p`/`-a` preservam os originais.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-r`, `-R` | Copia diretórios recursivamente | `cp -r d1 d2` |
| `-i` | Pergunta antes de sobrescrever | `cp -i a b` |
| `-n` | Nunca sobrescreve | `cp -n a b` |
| `-f` | Força: remove o destino se não puder abri-lo | `cp -f a b` |
| `-u` | Copia só se a origem for mais nova ou o destino não existir | `cp -u a b` |
| `-v` | Mostra o que está sendo copiado | `cp -v a b` |
| `-p` | Preserva dono, permissões e datas | `cp -p a b` |
| `-a` | Modo arquivo: `-r` + preserva tudo + links simbólicos | `cp -a d1 d2` |
| `-b` | Faz backup do destino antes de sobrescrever | `cp -b a b` |

---

## `echo`

**Para que serve:** imprime um texto (ou o valor de variáveis) na saída.

**Como usar**

```bash
echo [opções] [texto...]
```

```bash
echo "Olá, mundo"
echo $HOME                    # valor de uma variável
echo -e "linha1\nlinha2"      # interpreta \n
echo -n "sem quebra de linha"
```

**Como se comporta**

- Adiciona uma quebra de linha no final, a menos que use `-n`.
- Aspas duplas permitem expandir variáveis (`"$USER"`); aspas simples imprimem o texto literal (`'$USER'`).
- Muito usado junto com redirecionamento para gravar em arquivos.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-n` | Não imprime a quebra de linha final | `echo -n "oi"` |
| `-e` | Interpreta sequências com `\` (`\n`, `\t`, `\\`) | `echo -e "a\tb"` |
| `-E` | Não interpreta sequências com `\` (padrão) | `echo -E "a\tb"` |

---

## `>` e `>>` (redirecionamento)

**Para que serve:** em vez de mostrar a saída de um comando no terminal, grava em um arquivo.

**Como usar**

```bash
echo "ola meu nome é" > arquivo.txt     # cria/sobrescreve
echo "ola meu nome é" >> arquivo.txt    # acrescenta ao final
```

**Como se comporta**

| Operador | Comportamento |
|----------|---------------|
| `>` | **Apaga** o conteúdo anterior do arquivo e grava o novo. Cria o arquivo se não existir |
| `>>` | **Acrescenta** ao final, preservando o conteúdo. Cria o arquivo se não existir |

- Quem faz o redirecionamento é o shell, antes de o comando rodar. Por isso `> arquivo.txt` sozinho já cria (ou zera) o arquivo.
- Um erro comum é usar `>` quando queria `>>` e perder o conteúdo.

**Operadores relacionados**

| Operador | O que faz | Exemplo |
|----------|-----------|---------|
| `<` | Usa um arquivo como entrada | `sort < lista.txt` |
| `2>` | Grava só as mensagens de erro | `ls /x 2> erros.txt` |
| `2>>` | Acrescenta mensagens de erro | `ls /x 2>> erros.txt` |
| `&>` | Grava saída e erros juntos (bash) | `cmd &> tudo.txt` |
| `\|` | Envia a saída para outro comando (pipe) | `cat x.txt \| head` |
| `/dev/null` | "Buraco negro": descarta o que for enviado | `cmd > /dev/null` |

---

## `head`

**Para que serve:** mostra o começo de um arquivo (padrão: 10 primeiras linhas).

**Como usar**

```bash
head [opções] [arquivo...]
```

```bash
head log.txt            # 10 primeiras linhas
head -n 3 log.txt       # 3 primeiras linhas
head -c 100 log.txt     # 100 primeiros bytes
```

**Como se comporta**

- Com mais de um arquivo, imprime um cabeçalho com o nome de cada um.
- Lê só o necessário, então é rápido mesmo em arquivos enormes.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-n N` | Mostra as N primeiras linhas | `head -n 5 x` |
| `-n -N` | Mostra tudo **exceto** as N últimas linhas | `head -n -2 x` |
| `-c N` | Mostra os N primeiros bytes | `head -c 50 x` |
| `-q` | Não imprime os cabeçalhos com nomes | `head -q a b` |
| `-v` | Sempre imprime o cabeçalho | `head -v x` |

---

## `tail`

**Para que serve:** mostra o final de um arquivo (padrão: 10 últimas linhas). Muito usado para acompanhar logs.

**Como usar**

```bash
tail [opções] [arquivo...]
```

```bash
tail log.txt                    # 10 últimas linhas
tail -n 20 log.txt              # 20 últimas
tail -f /var/log/syslog         # acompanha em tempo real (Ctrl+C sai)
```

**Como se comporta**

- Com `-f`, o comando **não termina**: continua mostrando novas linhas que chegam ao arquivo até você pressionar `Ctrl+C`.
- Se o arquivo for rotacionado (log novo), `-f` continua no arquivo antigo; use `-F` para seguir o nome.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-n N` | Mostra as N últimas linhas | `tail -n 5 x` |
| `-n +N` | Mostra a partir da linha N até o fim | `tail -n +3 x` |
| `-c N` | Mostra os N últimos bytes | `tail -c 100 x` |
| `-f` | Segue o arquivo, mostrando novas linhas | `tail -f x` |
| `-F` | Como `-f`, mas reabre o arquivo se for recriado | `tail -F x` |
| `-q` | Não imprime cabeçalhos | `tail -q a b` |
| `-v` | Sempre imprime cabeçalho | `tail -v x` |

---

## `nano`

**Para que serve:** editor de texto simples dentro do terminal, ideal para iniciantes. Os atalhos aparecem na parte de baixo da tela (`^` significa `Ctrl`, `M-` significa `Alt`).

**Como usar**

```bash
nano [opções] [arquivo]
```

```bash
nano notas.txt          # abre (ou cria) notas.txt
nano +25 script.sh      # abre já na linha 25
sudo nano /etc/hosts    # editar arquivo de sistema
```

**Como se comporta**

- Se o arquivo não existe, ele só é criado ao salvar.
- Funciona como editor comum: digite e use as setas. Os comandos usam `Ctrl`.
- Ao sair com alterações não salvas, pergunta se quer salvar.
- Para editar arquivos do sistema (`/etc/...`) é preciso `sudo`.

**Atalhos principais**

| Atalho | Ação |
|--------|------|
| `Ctrl+O` | Salva (*write Out*), confirme com Enter |
| `Ctrl+X` | Sai (pergunta se quer salvar) |
| `Ctrl+W` | Procura texto |
| `Ctrl+\` | Procura e substitui |
| `Ctrl+K` | Corta a linha atual |
| `Ctrl+U` | Cola o que foi cortado |
| `Ctrl+_` | Vai para uma linha/coluna |
| `Alt+U` / `Alt+E` | Desfaz / refaz |
| `Ctrl+C` | Mostra posição do cursor |
| `Ctrl+G` | Ajuda |

**Opções de linha de comando**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `+N` | Abre na linha N | `nano +10 x` |
| `-l` | Mostra números de linha | `nano -l x` |
| `-m` | Habilita uso do mouse | `nano -m x` |
| `-B` | Cria backup (`arquivo~`) ao salvar | `nano -B x` |
| `-i` | Indentação automática | `nano -i x` |
| `-w` | Não quebra linhas longas automaticamente | `nano -w x` |
| `-v` | Modo somente leitura | `nano -v x` |
