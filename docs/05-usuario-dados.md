# 05 · Usuário: dados

Arquivos do sistema onde ficam as informações de usuários, grupos e permissões de administrador. São **arquivos de configuração**, não comandos.

| Arquivo | Resumo |
|---------|--------|
| [`/etc/passwd`](#etcpasswd) | Lista de usuários |
| [`nano /etc/sudoers`](#nano-etcsudoers) | Quem pode usar `sudo` |
| [`/bin/bash`](#binbash) | Shell de login padrão |
| [`cat /etc/group`](#cat-etcgroup) | Lista de grupos |

---

## `/etc/passwd`

**Para que serve:** guarda uma linha por usuário do sistema (contas de pessoas e de serviços).

**Como usar**

```bash
cat /etc/passwd              # lista tudo
grep maria /etc/passwd       # só a linha da maria
cut -d: -f1 /etc/passwd      # só os nomes de usuário
```

**Como se comporta**

- Pode ser lido por qualquer usuário.
- Cada linha tem 7 campos separados por `:`.
- Apesar do nome, **não guarda mais senhas**: elas ficam em `/etc/shadow`, que só o root lê.
- Editá-lo à mão é arriscado; prefira `adduser`, `usermod`, `userdel`. Se precisar, use `sudo vipw`.

**Formato**

```
maria:x:1001:1001:Maria Silva,,,:/home/maria:/bin/bash
  1   2  3    4         5            6           7
```

| # | Campo | Significado |
|---|-------|-------------|
| 1 | Nome | Login do usuário |
| 2 | Senha | `x` = senha em `/etc/shadow` |
| 3 | UID | ID numérico (0 = root; 1-999 = sistema; 1000+ = pessoas) |
| 4 | GID | ID do grupo principal |
| 5 | GECOS | Nome completo e comentários |
| 6 | Home | Diretório pessoal |
| 7 | Shell | Programa executado no login (`/usr/sbin/nologin` impede login) |

---

## `nano /etc/sudoers`

**Para que serve:** define **quem pode usar `sudo`** e o que pode executar.

> ⚠️ Um erro de sintaxe neste arquivo pode **bloquear o `sudo` para todos**. A forma segura de editar é `sudo visudo`, que valida a sintaxe antes de salvar. Se você prefere o `nano`, peça ao `visudo` para usá-lo:
>
> ```bash
> sudo EDITOR=nano visudo
> ```

**Como usar**

```bash
sudo visudo                    # edita com validação
sudo nano /etc/sudoers         # funciona, mas sem validação (não recomendado)
```

**Como se comporta**

- Somente root (ou `sudo`) pode abrir para edição.
- É lido a cada uso do `sudo`; a mudança vale imediatamente.
- Em Debian/Ubuntu o grupo `sudo` já está autorizado; por isso basta `adduser usuário sudo`.
- Arquivos extras podem ficar em `/etc/sudoers.d/` (também edite com `sudo visudo -f`).

**Formato de uma regra**

```
usuário  host=(executar_como)  comandos
```

| Exemplo | Significado |
|---------|-------------|
| `root ALL=(ALL:ALL) ALL` | root pode tudo |
| `%sudo ALL=(ALL:ALL) ALL` | membros do grupo `sudo` podem tudo (`%` = grupo) |
| `maria ALL=(ALL) ALL` | maria pode executar qualquer comando como qualquer usuário |
| `maria ALL=(ALL) NOPASSWD: ALL` | maria usa `sudo` sem digitar senha (**use com cuidado**) |
| `joao ALL=(ALL) /usr/bin/apt` | joao só pode rodar `apt` com sudo |

**Opções do `visudo`**

| Opção | O que faz |
|-------|-----------|
| `-c` | Só verifica a sintaxe |
| `-f arquivo` | Edita outro arquivo (ex.: em `/etc/sudoers.d/`) |

---

## `/bin/bash`

**Para que serve:** é o programa **shell** (interpretador de comandos) mais usado no Linux. Aparece no último campo do `/etc/passwd` como o shell de login do usuário.

**Como usar**

```bash
echo $SHELL                  # seu shell de login
cat /etc/shells              # shells válidos no sistema
chsh -s /bin/zsh             # troca o seu shell de login
sudo usermod -s /bin/bash maria   # troca o shell de outro usuário
/bin/bash                    # abre um novo bash dentro do atual
```

**Como se comporta**

- Ao fazer login, o sistema executa o programa indicado no campo 7 do `/etc/passwd`.
- Se o shell for `/usr/sbin/nologin` ou `/bin/false`, o usuário **não consegue** abrir sessão interativa (comum em contas de serviço).
- Só podem ser usados como login os shells listados em `/etc/shells`.
- `exit` encerra o shell.

**Shells comuns**

| Shell | Caminho |
|-------|---------|
| Bash | `/bin/bash` |
| Sh (Dash no Debian/Ubuntu) | `/bin/sh` |
| Zsh | `/usr/bin/zsh` |
| Fish | `/usr/bin/fish` |
| Sem login | `/usr/sbin/nologin` |

**Opções comuns do `bash`**

| Opção | O que faz | Exemplo |
|-------|-----------|---------|
| `-c "cmd"` | Executa um comando e sai | `bash -c "echo oi"` |
| `-x` | Mostra cada comando antes de executar (debug) | `bash -x script.sh` |
| `-n` | Só verifica a sintaxe, sem executar | `bash -n script.sh` |
| `--version` | Mostra a versão | `bash --version` |

---

## `cat /etc/group`

**Para que serve:** lista os grupos do sistema e seus membros.

**Como usar**

```bash
cat /etc/group
grep sudo /etc/group      # quem está no grupo sudo
groups maria              # grupos de um usuário
id maria                  # UID, GID e grupos
```

**Como se comporta**

- Somente leitura para usuários comuns.
- Cada linha tem 4 campos separados por `:`.
- Todo usuário tem um **grupo principal** (campo 4 do `/etc/passwd`) que **não** aparece na lista de membros; só os grupos secundários aparecem aqui.
- Altere por `adduser`, `deluser`, `usermod -aG` ou `gpasswd`, não à mão.

**Formato**

```
sudo:x:27:maria,joao
  1  2 3     4
```

| # | Campo | Significado |
|---|-------|-------------|
| 1 | Nome | Nome do grupo |
| 2 | Senha | Normalmente `x` (senha de grupo em `/etc/gshadow`) |
| 3 | GID | ID numérico do grupo |
| 4 | Membros | Lista de usuários separada por vírgula |

**Comandos relacionados**

| Comando | O que faz |
|---------|-----------|
| `groups [usuário]` | Lista os grupos |
| `id [usuário]` | Mostra UID, GID e grupos |
| `getent group nome` | Consulta um grupo (inclusive de LDAP etc.) |
| `sudo groupadd nome` | Cria um grupo |
| `sudo groupdel nome` | Remove um grupo |
