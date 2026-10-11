# 04 · Gerenciamento de usuários

Quem sou eu, como virar administrador e como criar/remover usuários.

> Os exemplos assumem **Debian/Ubuntu** (`adduser` e `deluser` são scripts dessas distribuições). Criar e remover usuários exige privilégios de administrador (`sudo`).

| Comando | Resumo |
|---------|--------|
| [`whoami`](#whoami) | Mostra o usuário atual |
| [`sudo -s`](#sudo--s) | Abre um shell como root usando `sudo` |
| [`su root`](#su-root) | Troca para o usuário root |
| [`passwd root`](#passwd-root) | Altera a senha de um usuário |
| [`adduser`](#adduser) | Cria usuário |
| [`deluser`](#deluser) | Remove usuário (Debian) |
| [`userdel`](#userdel) | Remove usuário (baixo nível) |
| [`rm -rf`](#rm--rf) | Remove diretórios à força (ex.: sobras de usuário) |
| [`adduser usuario grupo`](#adduser-usuario-grupo) | Adiciona usuário a um grupo |

---

## `whoami`

**Para que serve:** imprime o nome do usuário com que você está logado.

**Como usar**

```bash
$ whoami
maria
$ sudo whoami
root
```

**Como se comporta**

- Mostra o usuário **efetivo** do processo. Dentro de `sudo` ou `su`, mostra `root`.
- Útil em scripts e para conferir se você realmente virou root.

**Opções**

| Opção | O que faz |
|-------|-----------|
| `--help` | Mostra a ajuda |
| `--version` | Mostra a versão |

> Para ver mais detalhes (UID, GID, grupos), use `id`.

---

## `sudo -s`

**Para que serve:** abrir um shell com privilégios de root, autenticando com a **sua própria senha**.

**Como usar**

```bash
sudo -s            # shell root mantendo parte do seu ambiente
sudo -i            # shell de login do root (ambiente do root)
sudo comando       # executa um único comando como root
sudo -u maria cmd  # executa como outro usuário
```

**Como se comporta**

- Pede a **sua** senha (não a do root) e só funciona se você estiver autorizado no `/etc/sudoers` ou no grupo `sudo`.
- O prompt muda de `$` para `#`.
- Para sair, digite `exit` ou `Ctrl+D`.
- A senha fica em cache por alguns minutos (padrão: 15).
- As ações ficam registradas em log.
- `-s` inicia o shell mas mantém boa parte do ambiente atual (home etc.); `-i` simula um login completo como root.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-s` | Abre um shell (o definido em `$SHELL`) como root | `sudo -s` |
| `-i` | Shell de login (ambiente do usuário alvo) | `sudo -i` |
| `-u usuário` | Executa como o usuário indicado | `sudo -u www-data ls` |
| `-l` | Lista o que você pode executar com sudo | `sudo -l` |
| `-k` | Invalida o cache da senha | `sudo -k` |
| `-v` | Renova o cache da senha | `sudo -v` |
| `-E` | Preserva as variáveis de ambiente | `sudo -E cmd` |
| `-b` | Executa em segundo plano | `sudo -b cmd` |

---

## `su root`

**Para que serve:** trocar de usuário. Significa *substitute user* (ou *switch user*). Sem nome, assume `root`.

**Como usar**

```bash
su root            # vira root (mantém seu ambiente)
su - root          # vira root com ambiente completo (recomendado)
su - maria         # vira outro usuário
su -c "cmd" root   # executa um comando como root e volta
```

**Como se comporta**

- Pede a senha do **usuário de destino** (a do root, em `su root`).
- No Ubuntu a conta root vem sem senha por padrão, então `su` falha até que você defina uma senha (veja [`passwd root`](#passwd-root)). Por isso lá se usa `sudo -i`.
- `su` sem o `-` mantém variáveis e diretório atual; `su -` carrega o ambiente do destino e vai para a home dele.
- Para voltar ao usuário anterior: `exit`.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-`, `-l`, `--login` | Shell de login: ambiente e diretório do usuário alvo | `su - root` |
| `-c "cmd"` | Executa um único comando e volta | `su -c "whoami" root` |
| `-s shell` | Usa o shell indicado | `su -s /bin/sh maria` |
| `-p`, `-m` | Preserva o ambiente atual | `su -p root` |

---

## `passwd root`

**Para que serve:** altera a senha de um usuário. Sem argumento, altera a **sua**.

**Como usar**

```bash
passwd                   # altera a sua senha
sudo passwd root         # define/altera a senha do root
sudo passwd maria        # altera a senha da maria
sudo passwd -l maria     # bloqueia a conta da maria
```

**Como se comporta**

- Usuário comum só altera a própria senha e precisa informar a senha atual.
- Como root (ou com `sudo`), altera a senha de qualquer usuário sem precisar da atual.
- Pede a nova senha duas vezes; a digitação **não aparece** na tela.
- Aplica regras de qualidade (tamanho mínimo, simplicidade). Root pode ignorá-las com aviso.
- A senha é guardada de forma criptografada em `/etc/shadow`.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-l` | Bloqueia a conta (senha deixa de funcionar) | `sudo passwd -l maria` |
| `-u` | Desbloqueia a conta | `sudo passwd -u maria` |
| `-d` | Remove a senha (conta sem senha, **inseguro**) | `sudo passwd -d maria` |
| `-e` | Expira a senha: exige nova no próximo login | `sudo passwd -e maria` |
| `-S` | Mostra o status da senha da conta | `passwd -S maria` |
| `-n dias` | Mínimo de dias entre trocas | `sudo passwd -n 1 maria` |
| `-x dias` | Máximo de dias de validade | `sudo passwd -x 90 maria` |
| `-w dias` | Dias de aviso antes de expirar | `sudo passwd -w 7 maria` |

---

## `adduser`

**Para que serve:** cria um novo usuário. No Debian/Ubuntu é um script amigável que usa o `useradd` por baixo.

**Como usar**

```bash
sudo adduser [opções] usuário
```

```bash
sudo adduser maria
sudo adduser --shell /bin/zsh --home /srv/joao joao
sudo adduser --system --no-create-home servico
sudo adduser --disabled-password ana
```

**Como se comporta**

- De forma interativa, pergunta: senha, nome completo, telefones e confirmação.
- Cria o diretório `/home/usuário`, copia os arquivos de `/etc/skel` (`.bashrc`, `.profile`...) e cria um grupo com o mesmo nome do usuário.
- Define o shell padrão (normalmente `/bin/bash`).
- Adiciona uma linha em `/etc/passwd`, `/etc/shadow` e `/etc/group`.
- Se o usuário já existe, avisa e não altera nada.

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `--home DIR` | Define o diretório home | `--home /srv/joao` |
| `--shell SHELL` | Define o shell de login | `--shell /bin/zsh` |
| `--uid N` | Define o UID | `--uid 1500` |
| `--gid N` | Define o grupo principal (por número) | `--gid 1500` |
| `--ingroup GRUPO` | Define o grupo principal (existente) | `--ingroup devs` |
| `--no-create-home` | Não cria a home | `--no-create-home` |
| `--disabled-password` | Cria sem senha (login por senha desativado; chave SSH ainda funciona) | |
| `--disabled-login` | Cria sem senha e sem login possível | |
| `--system` | Cria usuário de sistema (serviços) | `--system` |
| `--comment "texto"` (`--gecos` em versões antigas) | Preenche nome completo/comentário | `--comment "Maria Silva"` |
| `--group` | Com `--system`, cria um grupo com o mesmo nome | |

---

## `adduser usuario grupo`

**Para que serve:** quando os dois argumentos já existem, o `adduser` **adiciona o usuário a um grupo existente**.

**Como usar**

```bash
sudo adduser maria sudo        # maria passa a ser administradora
sudo adduser maria docker      # maria pode usar o docker
```

```
Adicionando o usuário `maria' ao grupo `sudo' ...
```

**Como se comporta**

- Só altera os grupos **secundários** do usuário; o grupo principal não muda.
- O usuário precisa **sair e entrar de novo** (logout/login) para a mudança valer na sessão.
- Para conferir: `groups maria` ou `id maria`.
- Equivalente em qualquer distribuição: `sudo usermod -aG grupo usuário`. O `-a` (append) é essencial; sem ele, o usuário **perde** os outros grupos.

**Opções / formas relacionadas**

| Comando | O que faz |
|---------|-----------|
| `adduser usuário grupo` | Adiciona usuário ao grupo |
| `deluser usuário grupo` | Remove usuário do grupo |
| `usermod -aG grupo usuário` | Adiciona ao grupo (qualquer distro) |
| `groups usuário` | Lista os grupos do usuário |
| `adduser --group grupo` | Cria um novo grupo |

---

## `deluser`

**Para que serve:** remove um usuário (script do Debian/Ubuntu, equivalente amigável ao `userdel`).

**Como usar**

```bash
sudo deluser [opções] usuário
```

```bash
sudo deluser maria                    # remove a conta, mantém a home
sudo deluser --remove-home maria      # remove a conta e a home
sudo deluser --backup --remove-home maria
```

**Como se comporta**

- Por padrão **só remove a conta** (linhas de `/etc/passwd`, `/etc/shadow` e dos grupos). Os arquivos continuam no disco.
- O grupo com o mesmo nome do usuário é removido se estiver vazio.
- Não remove usuário com processos em execução.
- `deluser usuário grupo` remove o usuário do grupo, sem apagar a conta.

**Opções**

| Opção | O que faz |
|-------|-----------|
| `--remove-home` | Remove também a home e a caixa de e-mail |
| `--remove-all-files` | Remove **todos** os arquivos do usuário no sistema |
| `--backup` | Faz backup dos arquivos removidos (`.tar.gz`) |
| `--backup-to DIR` | Define onde salvar o backup |
| `--system` | Só remove se for usuário de sistema |
| `--group` | Remove um grupo em vez de usuário |

---

## `userdel`

**Para que serve:** remove um usuário. Comando padrão de baixo nível, presente em qualquer distribuição.

**Como usar**

```bash
sudo userdel [opções] usuário
```

```bash
sudo userdel maria        # remove a conta, mantém a home
sudo userdel -r maria     # remove a conta, a home e o spool de e-mail
```

**Como se comporta**

- Não pergunta confirmação.
- Recusa remover se o usuário estiver logado ou tiver processos (a não ser que use `-f`).
- Sem `-r`, a home e os arquivos ficam como "órfãos" (o dono passa a aparecer só como número de UID).

**Opções**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-r` | Remove também a home e o spool de e-mail | `userdel -r maria` |
| `-f` | Força, mesmo se logado (perigoso) | `userdel -f maria` |

**`deluser` × `userdel`**

| | `deluser` | `userdel` |
|--|-----------|-----------|
| Disponibilidade | Debian/Ubuntu | Todas as distros |
| Estilo | Script amigável | Comando de baixo nível |
| Backup dos arquivos | Sim (`--backup`) | Não |
| Remover home | `--remove-home` | `-r` |

---

## `rm -rf`

**Para que serve:** remove um diretório **e tudo que há dentro**, sem perguntar nada. Aparece aqui porque, depois de `userdel` sem `-r`, é com ele que se limpa a antiga home.

> ⚠️ **O comando mais perigoso desta lista.** Sem lixeira, sem confirmação, sem desfazer. Um espaço no lugar errado ou uma variável vazia pode apagar o que você não queria. Veja a tabela de `rm` em [`03-arquivos.md`](03-arquivos.md#rm).

**Como usar**

```bash
sudo rm -rf /home/maria_antiga
```

**Boas práticas**

- Rode antes `ls /home/maria_antiga` para conferir o alvo.
- Use `rm -ri` (pergunta a cada item) ou `rm -rv` (mostra o que apaga) enquanto estiver aprendendo.
- Nunca rode `rm -rf` com um caminho que você não conferiu, especialmente com `sudo`.
- Evite combinar com variáveis que podem estar vazias (`rm -rf $DIR/`).

**Opções**

| Opção | O que faz |
|-------|-----------|
| `-r` | Recursivo: entra nos diretórios |
| `-f` | Força: não pergunta e ignora erros |
| `-i` | Pergunta antes de apagar cada item |
| `-v` | Mostra o que foi removido |
