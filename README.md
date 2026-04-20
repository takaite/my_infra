# 🚀 My Infra - Ambiente de Desenvolvimento

Este repositório contém a automação em **Ansible** para provisionar um ambiente de desenvolvimento padronizado em instâncias **Ubuntu 24.04**. O setup é projetado para ser agnóstico e modular, permitindo que cada desenvolvedor escolha exatamente quais componentes deseja instalar.

## 🛠️ Componentes Incluídos

* **Base:** Git, Tmux, Mosh, Curl, Build-essential, Ripgrep.
* **Editor:** Neovim (via PPA estável) configurado com Lazy.nvim, LSP (C++/Python) e Copilot.
* **Shell:** ZSH + Oh My Zsh (Opcional).
* **Infra:** Docker Engine (Pronto para builds Yocto).
* **Personalização:** Gerenciamento de links simbólicos via **GNU Stow**.

---

## 📋 Pré-requisitos

Certifique-se de ter o Ansible instalado na sua máquina local:

```bash
sudo apt update && \
sudo apt install -y software-properties-common && \
sudo add-apt-repository --yes --update ppa:ansible/ansible && \
sudo apt install -y ansible
```

## 🚀 Como Usar
### 1. Configurar o Inventário
Edite o arquivo `inventory.yml` para incluir os hosts desejados. Você pode configurar o `localhost` para rodar na própria máquina usando `ansible_connection: local`.

### 2. Rodar o Playbook
Utilize as **Tags** para controlar o que será instalado.

**Setup Completo (Padrão):**
```bash
ansible-playbook setup_dev_env.yml --ask-become-pass
```

**Instalar apenas o básico para Build (Docker + Base):**
```bash
ansible-playbook setup_dev_env.yml --tags "infra,base"
```

**Instalar tudo, EXCETO o ZSH:**
```bash
ansible-playbook setup_dev_env.yml --skip-tags "zsh"
```

**Apenas atualizar Dotfiles (via Stow):**
```bash
ansible-playbook setup_dev_env.yml --tags "stow"
```
---

## 👤 Personalização (Dotfiles)

Este projeto utiliza o **GNU Stow** para gerenciar suas configurações pessoais de forma modular.

1.  Clone seu repositório pessoal de dotfiles em `~/my_dotfiles`.
2.  Organize o repositório em pastas por módulo:
    * `zsh/` -> Contendo seu `.zshrc`
    * `nvim/` -> Contendo `.config/nvim/`
    * `tmux/` -> Contendo `.tmux.conf`
3.  O Ansible detectará a pasta automaticamente e criará os links simbólicos na sua `$HOME`.

---

## ⌨️ Atalhos Úteis (Neovim + Solarized)

A configuração inclusa foi otimizada para desenvolvedores que alternam ambientes:
* `<Leader>sl`: Ativa o **Solarized Light** (ideal para ambientes muito iluminados).
* `<Leader>sd`: Ativa o **Solarized Dark** (ideal para baixa luminosidade).
* `:Copilot setup`: Comando para vincular sua conta do GitHub Copilot.

---

## 🐳 Notas sobre Yocto / Embarcados

O ambiente está preparado para lidar com fluxos de trabalho pesados:
* Variáveis de compilação: `PARALLEL_MAKE="-j 4"` e `BB_NUMBER_THREADS=4`.
* Paths configurados para toolchains em: `~/embedded/mw_tools` e `/opt/labs/ex/...`.
* Uso recomendado do **Mosh** para acesso via tablets/redes móveis, garantindo persistência de sessão e baixa latência.

---

