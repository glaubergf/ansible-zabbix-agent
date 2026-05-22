<!---
# ================================================================
Projeto: ansible-zabbix-agent
---
Descrição: Projeto Ansible modular para instalar, configurar e remover
o Zabbix Agent em distribuições Linux baseadas em Debian, RedHat e Suse,
incluindo gerenciamento automático de firewall (UFW, Firewalld e IPTables).
---
Autor: Glauber GF (mcnd2)
Criado: 21-04-2024
Atualizado: 21-05-2026
# ================================================================
--->

# Zabbix Agent com Ansible (Debian, RedHat e Suse)

![Image](https://github.com/glaubergf/ansible-zabbix-agent/blob/main/images/hosts_zabbix.png)

![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)

## 📜 Sobre o Projeto

Este projeto automatiza a instalação, configuração e remoção do **Zabbix Agent** utilizando **Ansible**, com suporte para distribuições Linux baseadas em:

- Debian
- RedHat
- Suse

O projeto foi totalmente refatorado para uma arquitetura **modular**, facilitando manutenção, reutilização e execução seletiva através de **tags**.

Além da instalação do agente, o projeto também realiza:

- Configuração do serviço
- Configuração automática de firewall
- Gerenciamento de regras UFW, Firewalld e IPTables
- Limpeza completa na remoção
- Tasks específicas por distribuição

---

# 📊 Sobre o Zabbix Agent

O **[Zabbix Agent](https://www.zabbix.com/documentation/current/pt/manual/concepts/agent)** é responsável pela coleta de métricas e informações do sistema operacional monitorado.

Ele pode operar em dois modos:

## 🔹 Modo Passivo

O servidor Zabbix consulta o agente periodicamente.

## 🔹 Modo Ativo

O agente envia informações automaticamente para o servidor Zabbix.

---

# 🤖 Sobre o Ansible

O **[Ansible](https://docs.ansible.com/ansible/latest/index.html)** é uma ferramenta open source de automação utilizada para provisionamento, gerenciamento de configuração e orquestração.

Com o Ansible é possível:

- Automatizar instalação de pacotes
- Configurar serviços
- Gerenciar firewall
- Padronizar ambientes
- Reduzir erros operacionais

O projeto utiliza uma estrutura baseada em:

- Roles
- Tasks modulares
- Variáveis por distribuição
- Tags específicas

---

# 🧩 Tecnologias Utilizadas

![Ansible](https://img.shields.io/badge/Ansible-EE0000?logo=ansible&logoColor=white&style=for-the-badge)

- [Ansible](https://www.ansible.com/) — Automação e gerenciamento de configuração.

---

![Zabbix](https://img.shields.io/badge/Zabbix-D40000?logo=zabbix&logoColor=white&style=for-the-badge)

- [Zabbix](https://www.zabbix.com/) — Plataforma de monitoramento.

---

![Debian](https://img.shields.io/badge/Debian-A81D33?logo=debian&logoColor=white&style=for-the-badge)

- [Debian](https://www.debian.org/) — Sistema Operacional.

---

![RedHat](https://img.shields.io/badge/RedHat-EE0000?logo=redhat&logoColor=white&style=for-the-badge)

- [Red Hat](https://www.redhat.com/) — Sistema Operacional.

---

![SUSE](https://img.shields.io/badge/SUSE-0C322C?logo=suse&logoColor=white&style=for-the-badge)

- [SUSE](https://www.suse.com/) — Sistema Operacional.

---

## 🔹 Distribuições testadas

| Distribuição     | Versão | Status |
| ---------------- | ------ | ------ |
| ![Debian](https://img.shields.io/badge/Debian-A81D33?logo=debian&logoColor=white&style=for-the-badge) | 13 | ✅ |
| ![Rocky Linux](https://img.shields.io/badge/Rocky%20Linux-10B981?logo=rockylinux&logoColor=white&style=for-the-badge) | 9 | ✅ |
| ![openSUSE](https://img.shields.io/badge/openSUSE-73BA25?logo=opensuse&logoColor=white&style=for-the-badge) | 16 | ✅ |
| ![Zorin OS](https://img.shields.io/badge/Zorin%20OS-15A6F0?logo=zorin&logoColor=white&style=for-the-badge) | 18 | ✅ |

---

# 🛠️ Pré-requisitos

- ✅ Ansible instalado
- ✅ Python instalado nos hosts alvo
- ✅ Acesso SSH aos hosts
- ✅ Usuário com permissão sudo
- ✅ Conectividade entre controlador e hosts

---

# ⚠️ Importante Sobre SSH

O Ansible depende de acesso SSH para executar tarefas remotamente.

Portanto:

- O serviço SSH deve estar instalado no host alvo
- A porta SSH deve estar liberada previamente
- O firewall não pode bloquear a conexão inicial

Caso o SSH esteja bloqueado, o Ansible não conseguirá acessar o host para aplicar as regras automaticamente.

---

# 📂 Estrutura do Projeto

```bash
ansible-zabbix-agent
├── ansible.cfg
├── CHANGEDLOG.md
├── group_vars
│   └── all.yml
├── hosts
├── images
│   ├── dashboard_zabbix.png
│   └── hosts_zabbix.png
├── LICENSE
├── main.yml
├── README.md
└── roles
    ├── zabbix-agent-linux-in
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── readme.md
    │   ├── tasks
    │   │   ├── config.yml
    │   │   ├── docker.yml
    │   │   ├── firewall
    │   │   │   ├── firewalld.yml
    │   │   │   ├── iptables.yml
    │   │   │   └── ufw.yml
    │   │   ├── firewall.yml
    │   │   ├── install.yml
    │   │   ├── main.yml
    │   │   ├── prereq.yml
    │   │   ├── repo.yml
    │   │   └── service.yml
    │   ├── templates
    │   │   └── zabbix_agent.conf.j2
    │   └── vars
    │       ├── debian.yml
    │       ├── redhat.yml
    │       └── suse.yml
    └── zabbix-agent-linux-rm
        ├── readme.md
        ├── tasks
        │   ├── cleanup.yml
        │   ├── firewall
        │   │   ├── firewalld.yml
        │   │   ├── iptables.yml
        │   │   └── ufw.yml
        │   ├── firewall.yml
        │   ├── main.yml
        │   ├── os
        │   │   ├── debian.yml
        │   │   ├── redhat.yml
        │   │   └── suse.yml
        │   ├── prereq.yml
        │   ├── remove.yml
        │   └── service.yml
        └── vars
            └── main.yml
```

---

# 📑 Histórico de Alterações

Todas as mudanças importantes do projeto são documentadas no arquivo:

- [CHANGELOG.md](CHANGELOG.md)

Lá você encontra:
- novas funcionalidades
- correções
- refatorações
- melhorias
- mudanças de compatibilidade
- evolução entre versões

---

# 🚀 Fluxo de Funcionamento

## 🔹 Role de Instalação (`zabbix-agent-linux-in`)

A role executa:

1. Pré-requisitos
2. Detecção automática da distribuição
3. Detecção automática do firewall
4. Configuração do repositório oficial do Zabbix e Docker (se necessário)
5. Instalação do pacote zabbix-agent
6. Instalação do pacote docker (se necessário)
7. Configuração do arquivo `zabbix_agentd.conf`, `zabbix_agent2.conf` e `docker.conf` (se necessário)
8. Configuração do serviço
9. Configuração do firewall

---

## 🔹 Role de Remoção (`zabbix-agent-linux-rm`)

A role executa:

1. Pré-requisitos
2. Parada do serviço
3. Remoção do pacote
4. Remoção dos arquivos de configuração
5. Remoção das regras de firewall
6. Tasks específicas por distribuição
7. Limpeza final

---

# 🔥 Firewalls Suportados

O projeto detecta automaticamente qual backend está ativo:

| Firewall | Distribuições |
|---|---|
| UFW | Debian / Ubuntu / Zorin |
| Firewalld | RedHat / Rocky / openSUSE |
| IPTables | Debian minimal |

---

# 🏷️ Tags Disponíveis

## 📥 Instalação

| Tag | Descrição |
|---|---|
| `zbx-agt-in` | Executa toda instalação |
| `zbx-agt-in-prereq` | Apenas pré-requisitos |
| `zbx-agt-in-firewall` | Apenas firewall |
| `zbx-agt-in-ufw` | Apenas regras UFW |
| `zbx-agt-in-firewalld` | Apenas regras Firewalld |
| `zbx-agt-in-iptables` | Apenas regras IPTables |
| `zbx-agt-in-service` | Apenas serviço |
| `zbx-agt-in-docker` | Apenas Docker |
| `zbx-agt-in-config` | Apenas configuração do agente |

---

## 📤 Remoção

| Tag | Descrição |
|---|---|
| `zbx-agt-rm` | Executa toda remoção |
| `zbx-agt-rm-prereq` | Apenas pré-requisitos |
| `zbx-agt-rm-firewall` | Apenas firewall |
| `zbx-agt-rm-ufw` | Apenas regras UFW |
| `zbx-agt-rm-firewalld` | Apenas regras Firewalld |
| `zbx-agt-rm-iptables` | Apenas regras IPTables |
| `zbx-agt-rm-service` | Apenas serviço |
| `zbx-agt-rm-os` | Tasks específicas da distro |
| `zbx-agt-rm-cleanup` | Limpeza final |

---

# ▶️ Execução do Projeto

## Clone o repositório

```bash
git clone https://github.com/glaubergf/ansible-zabbix-agent.git

cd ansible-zabbix-agent
```

---

## Executar instalação completa

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in
```

---

## Executar remoção completa

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm
```

---

## Executar apenas firewall UFW

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-ufw
```

---

## Executar apenas firewall Firewalld

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-firewalld
```

---

## Executar apenas firewall IPTables

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-iptables
```

---

## Executar em modo simulação (Dry Run)

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in --check
```

---

# 🧠 Detecção Automática

O projeto detecta automaticamente:

- Distribuição Linux
- Família do sistema
- Gerenciador de pacotes
- Backend de firewall ativo

---

# 🧹 Limpeza e Remoção

A role de remoção remove:

- Pacotes
- Arquivos de configuração
- Serviços
- Regras de firewall
- Repositórios
- Dependências relacionadas

---

# 🤝 Contribuições

Contribuições são bem-vindas.

---

# 📜 Licença

Este projeto está licenciado sob os termos da:

**GNU General Public License v3**

https://www.gnu.org/licenses/gpl-3.0.html

---

# 🏛️ Aviso Legal

```text
Copyright (c) 2024-2026 Glauber GF (mcnd2)

Este programa é software livre: você pode redistribuí-lo e/ou modificá-lo
sob os termos da GNU General Public License conforme publicada pela
Free Software Foundation, na versão 3 da Licença ou posterior.

Este programa é distribuído na esperança de ser útil,
mas SEM NENHUMA GARANTIA.

Veja a Licença Pública Geral GNU para mais detalhes.
```