# Changelog

Todas as mudanças importantes deste projeto serão documentadas neste arquivo.

O formato é baseado em:
https://keepachangelog.com/pt-BR/1.1.0/

---

# [2.0.0] - 2026-05-21

## 🚀 Adicionado

- Estrutura modular para roles do Ansible
- Detecção automática de firewall
- Suporte modular para:
  - UFW
  - Firewalld
  - IPTables
- Tasks específicas por distribuição
- Tags granulares para execução parcial
- Novo README totalmente reescrito
- Melhor suporte para:
  - Debian
  - Rocky Linux
  - SUSE/openSUSE
  - Zorin OS

## 🔧 Alterado

- Refatoração completa das roles
- Separação das tasks em módulos independentes:
  - prereq
  - service
  - firewall
  - remove
  - cleanup
  - os
- Melhor organização de includes/imports
- Melhor tratamento de firewall backend
- Melhor controle de tags no include_tasks
- Melhor suporte ao modo `--check`

## 🐛 Corrigido

- Execução incorreta de módulos firewall via tags
- Problemas de include_tasks herdando tags indevidas
- Falha de detecção de firewall em alguns cenários
- Problemas de execução parcial por tags
- Ajustes no tratamento do UFW
- Correções relacionadas ao SSH e firewall

---

# [1.0.0] - 2024-04-21

## 🚀 Inicial

- Instalação do Zabbix Agent
- Remoção do Zabbix Agent
- Suporte Debian
- Suporte RedHat
- Suporte openSUSE
- Configuração básica de firewall