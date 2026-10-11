# 🐧 Treinamento Linux - Secday

Repositório onde documento, passo a passo, tudo o que estou aprendendo no **treinamento de Linux do Secday**: comandos, conceitos, scripts e pequenos projetos.

> **Objetivo:** fixar o conteúdo escrevendo com minhas próprias palavras, criar um material de consulta rápida e registrar minha evolução.

---

## 📖 Sobre o projeto

Este repositório é o meu **diário de estudos de Linux** durante o treinamento. Cada código, comando ou conceito que eu aprender será registrado aqui, organizado por tema e com exemplos práticos.

O que você vai encontrar:

- 📝 **Anotações** explicando comandos e conceitos
- 💻 **Scripts** em Shell (`.sh`) comentados
- 🧪 **Pequenos projetos** para praticar
- 📋 **Cheatsheet** com os comandos mais usados
- 🐛 **Problemas e soluções** que encontrei pelo caminho

---

## 🗂️ Estrutura

```
Treinameto_Liinux_Secday/
├── README.md
├── 01-basico/           # navegação, arquivos e pastas
├── 02-permissoes/       # chmod, chown, usuários e grupos
├── 03-processos/        # ps, top, kill
├── 04-redes/            # ping, ip, ssh
├── 05-shell-script/     # scripts em bash
├── projetos/            # práticas maiores
└── cheatsheet.md        # consulta rápida
```

Cada pasta possui seu próprio `README.md` com o resumo do tema.

---

## 🧠 O que é Linux?

**Linux** é um **sistema operacional** de código aberto, ou seja, o software que gerencia o hardware do computador (processador, memória, disco, rede) e permite que programas e usuários o utilizem. É um "concorrente" de sistemas como Windows e macOS.

### Linux é o kernel

Tecnicamente, **Linux é apenas o kernel** (o "núcleo" do sistema), criado por **Linus Torvalds** em **1991**. O kernel é a parte que conversa diretamente com o hardware.

Para formar um sistema completo, esse kernel é combinado com outras ferramentas (shell, utilitários, interface gráfica, gerenciador de pacotes). Esse conjunto é chamado de **distribuição**.

### Principais características

| Característica | O que significa |
|---|---|
| **Código aberto** | Qualquer pessoa pode ver, modificar e distribuir o código |
| **Gratuito** | A maioria das distribuições pode ser usada sem custo |
| **Seguro** | Sistema de permissões robusto e comunidade que corrige falhas rapidamente |
| **Estável** | Muito usado em servidores que ficam meses ligados sem reiniciar |
| **Personalizável** | Você escolhe e altera praticamente tudo no sistema |
| **Leve** | Funciona bem até em computadores antigos |

### Distribuições (distros) populares

| Distribuição | Perfil |
|---|---|
| **Ubuntu** | Muito popular, ótima para iniciantes |
| **Debian** | Estável, base de muitas outras distros |
| **Linux Mint** | Visual simples, parecido com Windows |
| **Fedora** | Tecnologias mais novas, apoiada pela Red Hat |
| **Arch Linux** | Minimalista e muito configurável, para usuários avançados |

### Onde o Linux é usado?

- 🌐 **Servidores web** e infraestrutura de internet
- ☁️ **Nuvem** (AWS, Google Cloud, Azure)
- 📱 **Android**, que é baseado no kernel Linux
- 🚀 **Supercomputadores** (praticamente todos usam Linux)
- 🔌 **Dispositivos embarcados** (roteadores, TVs, carros)
- 💻 **Desktops** de desenvolvedores e entusiastas

### Por que aprender Linux?

- Essencial para áreas como **DevOps, Cloud, Segurança e Desenvolvimento**
- Ajuda a entender como um computador realmente funciona
- Domínio do **terminal** aumenta muito a produtividade
- É uma habilidade muito valorizada no mercado

---

## 🚀 Como usar este repositório

Clone o projeto:

```bash
git clone https://github.com/DCapulot/Treinameto_Liinux_Secday.git
cd Treinameto_Liinux_Secday
```

Para rodar um dos scripts:

```bash
chmod +x 05-shell-script/ola-mundo.sh
./05-shell-script/ola-mundo.sh
```

---

## 🛣️ Roteiro de estudos

- [ ] Navegação e manipulação de arquivos
- [ ] Permissões e usuários
- [ ] Gerenciamento de processos
- [ ] Gerenciador de pacotes (`apt`, `dnf`, etc.)
- [ ] Redes e SSH
- [ ] Shell script
- [ ] Cron e automação
- [ ] Projetos práticos

*(Vou marcando conforme avanço.)*

---

## 📚 Recursos de estudo

- [Manual do Linux (`man`)](https://man7.org/linux/man-pages/): documentação oficial dos comandos
- [Linux Journey](https://linuxjourney.com/): curso gratuito para iniciantes
- [The Linux Command Line (livro gratuito)](https://linuxcommand.org/tlcl.php)

---

## 🤝 Contribuições

Este é um repositório pessoal de estudos, mas sugestões e correções são muito bem-vindas! Abra uma *issue* ou envie um *pull request*.

---

## 👤 Autor

**DCapulot**
[GitHub](https://github.com/DCapulot)

---

⭐ Se este material te ajudou, deixe uma estrela no repositório!
