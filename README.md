# 🐧 Linux Terminal

Guia em português dos comandos básicos do terminal Linux. Para cada comando você encontra:

- **Para que serve**: o que ele faz, em uma frase;
- **Como usar**: sintaxe e exemplos prontos para copiar;
- **Como se comporta**: o que acontece por baixo, pegadinhas e cuidados;
- **Opções**: uma tabela com as principais opções e exemplos.

> Os exemplos foram pensados para **Debian/Ubuntu** (por causa de `apt`, `dpkg`, `adduser`, `deluser`), mas a maior parte vale para qualquer distribuição.

## 📚 Índice

| # | Tópico | Comandos |
|---|--------|----------|
| 01 | [Navegação](docs/01-navegacao.md) | `pwd` `ls` `.` `..` `cd` |
| 02 | [Ajuda](docs/02-ajuda.md) | `--help` `man` |
| 03 | [Arquivos](docs/03-arquivos.md) | `touch` `mkdir` `rm` `cat` `cp` `echo` `>` `>>` `head` `tail` `nano` |
| 04 | [Gerenciamento de usuários](docs/04-gerenciamento-de-usuarios.md) | `whoami` `sudo -s` `su root` `passwd` `adduser` `deluser` `userdel` `rm -rf` |
| 05 | [Usuário: dados](docs/05-usuario-dados.md) | `/etc/passwd` `/etc/sudoers` `/bin/bash` `/etc/group` |
| 06 | [Gerenciamento de pacotes e busca](docs/06-gerenciamento-de-pacotes.md) | `apt` `dpkg` `find` `whereis` `which` `locate` `updatedb` |
| 07 | [Permissões](docs/07-permissoes.md) | `chmod` `chown` |

## 🗂️ Estrutura

```
linux-terminal/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── 01-navegacao.md
│   ├── 02-ajuda.md
│   ├── 03-arquivos.md
│   ├── 04-gerenciamento-de-usuarios.md
│   ├── 05-usuario-dados.md
│   ├── 06-gerenciamento-de-pacotes.md
│   └── 07-permissoes.md
└── referencia/
    └── lista-original.txt     # lista que deu origem ao repositório
```

## 🚀 Como usar

1. Escolha o tópico no índice acima.
2. Dentro do arquivo, use a tabela do topo para ir direto ao comando.
3. Teste os exemplos em um terminal. Prefira uma máquina virtual ou um diretório de testes (`mkdir ~/teste && cd ~/teste`) para praticar com segurança.

## ⚠️ Comandos que exigem cuidado

| Comando | Risco |
|---------|-------|
| `rm -rf` | Apaga sem confirmação e sem lixeira |
| `chmod 777` / `o+w` | Deixa arquivos abertos a qualquer usuário |
| `nano /etc/sudoers` | Erro de sintaxe pode bloquear o `sudo` (use `visudo`) |
| `userdel -f` | Remove usuário mesmo logado |
| `passwd -d` | Deixa a conta sem senha |

## 🧭 Dicas rápidas

- `Tab` completa nomes de comandos e arquivos; `Tab Tab` mostra as opções.
- `↑` repete comandos anteriores; `Ctrl+R` busca no histórico.
- `Ctrl+C` interrompe o comando atual; `Ctrl+D` encerra o shell.
- Em dúvida, `comando --help` ou `man comando`.

## 🤝 Contribuindo

Sugestões e correções são bem-vindas: abra uma *issue* ou um *pull request*. Ao adicionar um comando, siga o mesmo formato: **Para que serve → Como usar → Como se comporta → Opções**.

## 📄 Licença

Distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE).
