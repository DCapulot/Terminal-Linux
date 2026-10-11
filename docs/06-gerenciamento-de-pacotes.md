# 06 · Gerenciamento de pacotes e busca

Instalar/remover programas (Debian/Ubuntu) e encontrar arquivos e comandos no sistema.

> `apt` e `dpkg` são do **Debian, Ubuntu e derivados**. Em outras distros os equivalentes são `dnf`/`rpm` (Fedora), `pacman` (Arch), `zypper` (openSUSE).

| Comando | Resumo |
|---------|--------|
| [`apt update`](#apt-update) | Atualiza a lista de pacotes disponíveis |
| [`apt install`](#apt-install) | Instala software |
| [`apt remove`](#apt-remove) | Remove software |
| [`apt search`](#apt-search) | Procura pacotes |
| [`dpkg -i`](#dpkg--i) | Instala um arquivo `.deb` local |
| [`find`](#find) | Procura arquivos em tempo real |
| [`whereis`](#whereis) | Localiza binário, fonte e manual |
| [`which`](#which) | Mostra o caminho de um comando |
| [`locate`](#locate) | Procura arquivos via banco de dados |
| [`updatedb`](#updatedb) | Atualiza o banco do `locate` |

---

## `apt update`

**Para que serve:** baixa a lista atualizada de pacotes e versões dos repositórios. **Não instala nem atualiza nada**, só atualiza o "catálogo".

**Como usar**

```bash
sudo apt update
sudo apt upgrade        # depois, para de fato atualizar os programas
```

**Como se comporta**

- Lê os repositórios configurados em `/etc/apt/sources.list` e `/etc/apt/sources.list.d/`.
- No final informa quantos pacotes podem ser atualizados.
- Rode sempre antes de `apt install`, para evitar versões antigas ou pacotes "não encontrados".
- Precisa de internet e de `sudo`.

**Opções e comandos relacionados**

| Comando / opção | O que faz |
|-----------------|-----------|
| `apt update` | Atualiza a lista de pacotes |
| `apt upgrade` | Atualiza pacotes instalados (sem remover nenhum) |
| `apt full-upgrade` | Atualiza e pode remover pacotes se necessário |
| `apt list --upgradable` | Lista os pacotes com atualização disponível |
| `-q` | Saída mais silenciosa |

---

## `apt install`

**Para que serve:** instala um ou mais pacotes, resolvendo e baixando as dependências automaticamente.

**Como usar**

```bash
sudo apt install software
sudo apt install git curl vim           # vários de uma vez
sudo apt install -y htop                # sem pedir confirmação
sudo apt install nginx=1.24.0-1         # versão específica
sudo apt install ./programa.deb         # arquivo local (com dependências)
```

**Como se comporta**

- Mostra o que será instalado e pede confirmação `[S/n]`.
- Instala também as dependências necessárias.
- Se o pacote já está instalado, atualiza-o se houver versão nova; senão avisa que já está na versão mais recente.
- Se o nome estiver errado, retorna `E: Impossível encontrar o pacote`.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-y` | Responde "sim" a todas as perguntas | `apt install -y git` |
| `-s`, `--simulate` | Simula, sem instalar | `apt install -s git` |
| `--no-install-recommends` | Não instala pacotes recomendados | |
| `--reinstall` | Reinstala o pacote | `apt install --reinstall git` |
| `--only-upgrade` | Só atualiza se já estiver instalado | |
| `-f`, `--fix-broken` | Tenta corrigir dependências quebradas | `apt install -f` |
| `-d`, `--download-only` | Só baixa, sem instalar | |

---

## `apt remove`

**Para que serve:** desinstala um pacote, mantendo seus arquivos de configuração.

**Como usar**

```bash
sudo apt remove software
sudo apt purge software          # remove também as configurações
sudo apt autoremove              # remove dependências que ninguém mais usa
```

**Como se comporta**

- Remove os programas, mas **mantém** os arquivos de configuração em `/etc` (útil se for reinstalar).
- Dependências instaladas junto ficam no sistema; use `autoremove` para limpá-las.
- Mostra o que será removido e pede confirmação.

**Opções e variações**

| Comando / opção | O que faz |
|-----------------|-----------|
| `apt remove pacote` | Remove, mantém a configuração |
| `apt purge pacote` | Remove, **apaga** a configuração |
| `apt autoremove` | Remove dependências órfãs |
| `apt autoremove --purge` | Idem, apagando configurações |
| `-y` | Não pede confirmação |
| `-s` | Simula sem remover |

---

## `apt search`

**Para que serve:** procura pacotes pelo nome ou pela descrição.

**Como usar**

```bash
apt search software
apt search "editor de texto"
apt search --names-only vim       # só nos nomes
apt show vim                      # detalhes de um pacote
apt list --installed              # pacotes instalados
```

**Como se comporta**

- Pesquisa nos nomes e nas descrições; aceita expressões regulares.
- Não precisa de `sudo`.
- Lê o catálogo local; se estiver desatualizado, rode `apt update` antes.
- Pacotes já instalados aparecem marcados com `[instalado]`.

**Opções e comandos relacionados**

| Comando / opção | O que faz |
|-----------------|-----------|
| `apt search termo` | Busca por termo |
| `--names-only` | Busca só nos nomes |
| `apt show pacote` | Mostra versão, dependências, tamanho e descrição |
| `apt list --installed` | Lista os instalados |
| `apt list --upgradable` | Lista os atualizáveis |
| `apt policy pacote` | Mostra versão instalada e candidata |

---

## `dpkg -i`

**Para que serve:** instala diretamente um arquivo `.deb` baixado. `dpkg` é a ferramenta de baixo nível; o `apt` é construído em cima dela.

**Como usar**

```bash
sudo dpkg -i software.deb
sudo apt install -f              # corrige dependências faltantes
```

**Como se comporta**

- Instala o pacote **sem baixar dependências**. Se faltarem, o `dpkg` dá erro e deixa o pacote "meio configurado"; rode `sudo apt install -f` para resolver.
- Alternativa mais prática: `sudo apt install ./software.deb`, que já resolve as dependências.
- Pode ser usado para atualizar o pacote para outra versão.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-i arquivo.deb` | Instala | `dpkg -i x.deb` |
| `-r pacote` | Remove (mantém configurações) | `dpkg -r x` |
| `-P pacote` | Remove com configurações (*purge*) | `dpkg -P x` |
| `-l [padrão]` | Lista pacotes instalados | `dpkg -l \| grep nginx` |
| `-L pacote` | Lista os arquivos instalados pelo pacote | `dpkg -L nginx` |
| `-S arquivo` | Diz a qual pacote um arquivo pertence | `dpkg -S /bin/ls` |
| `-s pacote` | Mostra o status e as informações | `dpkg -s nginx` |
| `-I arquivo.deb` | Mostra informações de um `.deb` | `dpkg -I x.deb` |
| `-c arquivo.deb` | Lista o conteúdo de um `.deb` | `dpkg -c x.deb` |
| `--configure -a` | Termina configurações pendentes | |

---

## `find`

**Para que serve:** procura arquivos e diretórios **em tempo real**, percorrendo o disco, por nome, tipo, tamanho, data, dono etc.

**Como usar**

```bash
find [onde procurar] [critérios] [ação]
```

```bash
find / -name arquivo                  # procura em todo o sistema
find ~/projetos -name "arqu*"         # nomes que começam com "arqu"
find . -iname "*.TXT"                 # ignora maiúsculas/minúsculas
find /var/log -type f -size +10M      # arquivos com mais de 10 MB
find / -name arquivo 2>/dev/null      # oculta erros de "Permissão negada"
```

**Como se comporta**

- Percorre recursivamente o diretório indicado e seus subdiretórios.
- Sem critério, lista tudo.
- `-name` é **sensível a maiúsculas** e compara o nome inteiro; use curingas (`*`, `?`) para nomes parciais.
- **Sempre coloque o padrão entre aspas** (`"arqu*"`). Sem aspas, o shell expande o `*` com arquivos do diretório atual e pode quebrar a busca.
- Buscar em `/` pode demorar e gerar muitos erros "Permissão negada" se não usar `sudo`.
- Os critérios são combinados com "E" por padrão.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-name padrão` | Nome (sensível a maiúsculas) | `-name "*.log"` |
| `-iname padrão` | Nome (ignora maiúsculas) | `-iname "*.jpg"` |
| `-type f` / `d` / `l` | Arquivo / diretório / link | `-type d` |
| `-size +10M` | Tamanho (`+` maior, `-` menor; `k`, `M`, `G`) | `-size -1k` |
| `-mtime -N` | Modificado nos últimos N dias (`+N` = há mais de N dias) | `-mtime -7` |
| `-user nome` | Pertence ao usuário | `-user maria` |
| `-perm modo` | Tem as permissões indicadas | `-perm 644` |
| `-empty` | Arquivos ou diretórios vazios | `-empty` |
| `-maxdepth N` | Limita a profundidade | `-maxdepth 2` |
| `-exec cmd {} \;` | Executa um comando para cada resultado | `-exec ls -l {} \;` |
| `-delete` | Apaga os resultados (**cuidado**) | `-name "*.tmp" -delete` |
| `-not`, `!` | Nega um critério | `! -name "*.txt"` |
| `-o` | "OU" entre critérios | `-name "*.c" -o -name "*.h"` |

---

## `whereis`

**Para que serve:** localiza o **binário**, o **código-fonte** e o **manual** de um comando.

**Como usar**

```bash
whereis ls
```

```
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```

**Como se comporta**

- Procura só em locais padrão do sistema (não percorre o disco inteiro), por isso é rápido.
- Não encontra arquivos comuns do usuário; serve para comandos e programas.
- Se nada for encontrado, mostra apenas `nome:`.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-b` | Só binários | `whereis -b ls` |
| `-m` | Só manuais | `whereis -m ls` |
| `-s` | Só fontes | `whereis -s ls` |
| `-u` | Mostra os comandos que não têm uma das entradas | `whereis -m -u *` |

---

## `which`

**Para que serve:** mostra o **caminho do executável** que será rodado quando você digitar um comando.

**Como usar**

```bash
which python3
# /usr/bin/python3
which -a python3     # todas as ocorrências no PATH
```

**Como se comporta**

- Procura nos diretórios listados em `$PATH`, na ordem, e mostra o primeiro encontrado.
- Não mostra nada se o comando não existir (retorna código de saída 1).
- Não enxerga comandos internos do shell (`cd`, `echo`) nem aliases; para isso use `type comando`.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-a` | Mostra todas as ocorrências no `PATH` | `which -a python3` |
| `--version` | Mostra a versão | `which --version` |
| *(alternativa)* `type -a cmd` | Mostra aliases, builtins e arquivos | `type -a ls` |
| *(dica)* `echo $PATH` | Mostra os diretórios pesquisados | |

---

## `locate`

**Para que serve:** procura arquivos pelo nome de forma **muito rápida**, consultando um banco de dados pré-indexado.

**Como usar**

```bash
locate nome
locate -i "relatorio"          # ignora maiúsculas
locate -c "*.conf"             # só conta os resultados
locate -n 10 ".bashrc"         # limita a 10 resultados
```

**Como se comporta**

- Não percorre o disco: consulta o banco gerado pelo `updatedb`. Por isso é muito mais rápido que o `find`.
- O banco pode estar **desatualizado**: arquivos recém-criados não aparecem e arquivos apagados podem aparecer. Rode `sudo updatedb` para atualizar.
- Procura o texto em qualquer parte do **caminho completo**.
- Pode não vir instalado: `sudo apt install plocate` (ou `mlocate`).
- Mostra só o que você teria permissão para ver.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-i` | Ignora maiúsculas/minúsculas | `locate -i foto` |
| `-c` | Só mostra a quantidade | `locate -c .log` |
| `-n N` | Limita a N resultados | `locate -n 5 foto` |
| `-b` | Compara só com o nome do arquivo (não o caminho todo) | `locate -b nome` |
| `-r regex` | Usa expressão regular | `locate -r '\.conf$'` |
| `-e` | Só mostra arquivos que ainda existem | `locate -e nome` |
| `-S` | Estatísticas do banco | `locate -S` |

---

## `updatedb`

**Para que serve:** atualiza o banco de dados usado pelo `locate`.

**Como usar**

```bash
sudo updatedb
```

**Como se comporta**

- Percorre o sistema de arquivos e reconstrói o índice. Pode levar de segundos a minutos.
- Normalmente roda sozinho uma vez por dia (cron/timer), então só precisa chamar manualmente após criar/apagar arquivos e querer achá-los logo com `locate`.
- Configuração em `/etc/updatedb.conf`, onde se define o que **não** indexar (ex.: `/tmp`, `/proc`, sistemas de rede).
- Rodar sem `sudo` só indexa o que o seu usuário consegue ler.

**Opções** (variam um pouco entre `mlocate` e `plocate`; confira com `man updatedb`)

| Opção | O que faz |
|-------|-----------|
| `-v` | Mostra os arquivos conforme são indexados |
| `-U DIR` | Indexa apenas a partir de DIR |
| `-o ARQUIVO` | Grava o banco em outro arquivo |
| `-e DIRS` | Diretórios a excluir da indexação |
| `-l 0/1` | Controla a verificação de permissões na consulta |
