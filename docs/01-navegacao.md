# 01 · Navegação

Comandos para saber onde você está e se mover pelo sistema de arquivos.

| Comando | Resumo |
|---------|--------|
| [`pwd`](#pwd) | Mostra o diretório atual |
| [`ls`](#ls) | Lista o conteúdo de um diretório |
| [`.`](#--diretório-atual) | Atalho para o diretório atual |
| [`..`](#---diretório-pai) | Atalho para o diretório pai |
| [`cd`](#cd) | Muda de diretório |

---

## `pwd`

**Para que serve:** mostra o caminho completo (absoluto) do diretório em que você está. Significa *print working directory*.

**Como usar**

```bash
pwd [opções]
```

```bash
$ pwd
/home/maria/projetos
```

**Como se comporta**

- Não recebe argumentos; só imprime o caminho atual.
- Por padrão (`-L`) mostra o caminho lógico, ou seja, mantém os links simbólicos que você usou para chegar lá.
- Com `-P` mostra o caminho físico, resolvendo os links simbólicos.

**Opções**

| Opção | O que faz |
|-------|-----------|
| `-L` | Caminho lógico (padrão). Mantém links simbólicos no caminho |
| `-P` | Caminho físico. Resolve todos os links simbólicos |
| `--help` | Mostra a ajuda |
| `--version` | Mostra a versão |

---

## `ls`

**Para que serve:** lista arquivos e diretórios. Significa *list*.

**Como usar**

```bash
ls [opções] [caminho...]
```

```bash
ls                 # conteúdo do diretório atual
ls /etc            # conteúdo de /etc
ls -lah ~          # lista detalhada, com ocultos, da sua home
ls *.txt           # só arquivos .txt
```

**Como se comporta**

- Sem argumentos lista o diretório atual, em ordem alfabética.
- Arquivos que começam com `.` são **ocultos** e só aparecem com `-a` ou `-A`.
- Se o argumento é um arquivo, mostra só ele. Se é um diretório, mostra o conteúdo.
- Com `-l` cada linha mostra: tipo e permissões, nº de links, dono, grupo, tamanho, data e nome.

```
-rwxr-xr-- 1 maria devs 4096 out 10 09:30 script.sh
│└┬┘└┬┘└┬┘   │     │     │       │         └─ nome
│ │  │  │    │     │     │       └─ data de modificação
│ │  │  │    │     │     └─ tamanho em bytes
│ │  │  │    │     └─ grupo
│ │  │  │    └─ dono
│ │  │  └─ permissões de outros
│ │  └─ permissões do grupo
│ └─ permissões do dono
└─ tipo (- arquivo, d diretório, l link)
```

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-l` | Formato longo (permissões, dono, tamanho, data) | `ls -l` |
| `-a` | Mostra todos, incluindo ocultos, `.` e `..` | `ls -a` |
| `-A` | Mostra ocultos, mas sem `.` e `..` | `ls -A` |
| `-h` | Tamanhos legíveis (K, M, G). Use com `-l` | `ls -lh` |
| `-R` | Recursivo: entra nos subdiretórios | `ls -R /etc` |
| `-t` | Ordena por data de modificação (mais novo primeiro) | `ls -lt` |
| `-r` | Inverte a ordem | `ls -ltr` |
| `-S` | Ordena por tamanho (maior primeiro) | `ls -lS` |
| `-d` | Mostra o próprio diretório, não o conteúdo | `ls -ld /etc` |
| `-1` | Um item por linha | `ls -1` |
| `-i` | Mostra o número do inode | `ls -i` |
| `-F` | Marca tipos: `/` diretório, `*` executável, `@` link | `ls -F` |
| `--color=auto` | Colore por tipo de arquivo | `ls --color=auto` |

---

## `.` (diretório atual)

**Para que serve:** representa o diretório em que você está.

**Como usar**

```bash
./script.sh        # executa um script que está no diretório atual
cp /tmp/foto.png . # copia para o diretório atual
ls .               # lista o diretório atual (igual a ls)
```

**Como se comporta**

- Existe em todo diretório como uma entrada especial (aparece em `ls -a`).
- O shell **não** procura executáveis no diretório atual por segurança, por isso é preciso escrever `./programa` para rodar algo que está aqui.
- `./arquivo` e `arquivo` apontam para o mesmo lugar em comandos como `cat` e `cp`, mas só `./arquivo` funciona para executar.

> `. arquivo` (ponto, espaço, arquivo) é outra coisa: é o comando `source`, que executa o arquivo no shell atual.

---

## `..` (diretório pai)

**Para que serve:** representa o diretório acima do atual.

**Como usar**

```bash
cd ..              # sobe um nível
cd ../..           # sobe dois níveis
ls ..              # lista o diretório pai
cp arquivo ../     # copia para o diretório pai
```

**Como se comporta**

- Pode ser encadeado: `../../pasta` sobe dois níveis e entra em `pasta`.
- Na raiz (`/`), o pai de `/` é ela mesma.

---

## `cd`

**Para que serve:** muda o diretório atual. Significa *change directory*.

**Como usar**

```bash
cd [diretório]
```

```bash
cd /var/log        # caminho absoluto
cd projetos        # caminho relativo
cd ..              # sobe um nível
cd                 # vai para a sua home (~)
cd ~               # idem
cd -               # volta ao diretório anterior
cd "Meus Documentos"   # nomes com espaço precisam de aspas
```

**Como se comporta**

- É um comando interno (*builtin*) do shell, não um programa separado, porque precisa alterar o estado do próprio shell.
- Sem argumentos, vai para a home (`$HOME`).
- Caminho que começa com `/` é absoluto; os demais são relativos ao diretório atual.
- Se o diretório não existe ou você não tem permissão de execução nele, mostra erro e permanece onde está.
- `cd -` alterna entre os dois últimos diretórios e imprime o destino.

**Atalhos e opções**

| Forma | O que faz |
|-------|-----------|
| `cd` ou `cd ~` | Vai para a home |
| `cd -` | Volta ao diretório anterior |
| `cd ..` | Sobe um nível |
| `cd /` | Vai para a raiz |
| `cd ~usuario` | Vai para a home de outro usuário |
| `-L` | Segue links simbólicos (padrão) |
| `-P` | Usa a estrutura física, resolvendo links simbólicos |
