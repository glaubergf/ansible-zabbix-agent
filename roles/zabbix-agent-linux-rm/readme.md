# Zabbix Agent Linux Remove Role

Role Ansible para remoção completa do Zabbix Agent em distribuições Linux baseadas em Debian, RedHat e SUSE.

A role possui arquitetura modular, suporte multi-distribuição, múltiplos backends de firewall.

---

# Compatibilidade

## Distribuições suportadas

### Debian-based

* Debian
* Ubuntu
* Zorin OS
* Linux Mint
* derivados compatíveis com APT

### RedHat-based

* Rocky Linux
* AlmaLinux
* RHEL
* CentOS Stream
* derivados compatíveis com DNF/YUM

### SUSE-based

* openSUSE Leap
* SUSE Linux Enterprise
* derivados compatíveis com Zypper

---

# Estrutura da Role

```text
roles/
└── zabbix-agent-linux-rm
    ├── tasks
    │   ├── cleanup.yml
    │   ├── firewall
    │   │   ├── firewalld.yml
    │   │   ├── iptables.yml
    │   │   └── ufw.yml
    │   ├── firewall.yml
    │   ├── main.yml
    │   ├── os
    │   │   ├── debian.yml
    │   │   ├── redhat.yml
    │   │   └── suse.yml
    │   ├── prereq.yml
    │   ├── remove.yml
    │   └── service.yml
    └── vars
        └── main.yml
```

---

# Fluxo da Role

A role executa automaticamente:

1. Coleta de pré-requisitos
2. Detecção de família Linux
3. Parada e desabilitação do serviço do Zabbix Agent
4. Remoção dos pacotes do Zabbix Agent
5. Limpeza de configurações residuais
6. Remoção de regras de firewall específicas para cada backend (UFW, firewalld, IPTables)
7. Remoção de repositórios Zabbix em sistemas RedHat
8. Limpeza de diretórios residuais

---

# Compatibilidade

## Distribuições testadas

| Distribuição     | Status |
| ---------------- | ------ |
| Debian 13        | ✅      |
| Rocky Linux 9    | ✅      |
| openSUSE Leap 16 | ✅      |
| Zorin OS 18      | ✅      |

---

# Organização dos módulos

## prereq.yml

Responsável por:

* Coletar facts do sistema
* Detectar família Linux
* Detectar serviços instalados
* Detectar backend de firewall

---

## service.yml

Responsável por:

* Parar serviço do Zabbix Agent
* Desabilitar serviço no boot

---

## remove.yml

Responsável por:

* Remover pacotes Zabbix
* Purge de configurações residuais

---

## firewall.yml

Responsável por:

* Validar backend de firewall detectado
* Importar tasks específicas:
  * `ufw.yml`
  * `firewalld.yml`
  * `iptables.yml`

---

## cleanup.yml

Responsável por:

* Remover logs residuais
* Remover diretórios residuais

---

# Firewalls suportados

## UFW

Compatível com:
* Ubuntu
* Zorin OS
* Debian modernos

A role detecta automaticamente:
* regras IPv4
* regras IPv6
* índices numéricos do UFW

E remove corretamente:

```bash
ufw delete <RULE_NUMBER>
```

---

## firewalld

Compatível com:
* Rocky Linux
* AlmaLinux
* RHEL
* openSUSE

A role remove:

```bash
10050/tcp
```

de forma permanente e imediata.

---

## IPTables (fallback legado)

Compatível com:

* Debian antigos
* sistemas sem UFW/firewalld

A role:
* remove regra INPUT
* salva regras persistentes em:
  `/etc/iptables/rules.v4`

---

# Variáveis

## vars/main.yml

Responsável por definir variáveis específicas para a role.

A variável é permanente fixa para definir a porta do firewall usada pelo Zabbix Agent.

---

# Tags

## Execução completa

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm
```

---

## Simular execução (check mode)

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm -C
```

---

# Tags disponíveis

| Tag        | Descrição                          
| ---------- | ---------------------------------- 
| zbx-agt-rm-prereq     | Executa coleta de pré-requisitos
| zbx-agt-rm-firewall   | Executa remoção de regras de firewall
| zbx-agt-rm-iptables   | Executa remoção de regras de iptables
| zbx-agt-rm-firewalld  | Executa remoção de regras de firewalld
| zbx-agt-rm-ufw        | Executa remoção de regras de UFW
| zbx-agt-rm-remove     | Executa remoção de pacotes 
| zbx-agt-rm-cleanup    | Executa limpeza de arquivos residuais 
| zbx-agt-rm-service    | Executa parada e desabilitação do serviço 

---

## Apenas firewall

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm-firewall
```

---

## Apenas remoção de pacotes

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm-remove
```

---

## Apenas limpeza de arquivos residuais

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm-cleanup
```

---

## Apenas serviço

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-rm-service
```

---

# Recursos da Role

* Arquitetura modular
* Multi-distribuição
* Firewall automático
* Suporte Docker
* Idempotência
* Estrutura enterprise
* Fácil manutenção
* Fácil expansão
* Compatível com CI/CD
* Compatível com Ansible Lint

Podendo ser executada múltiplas vezes sem causar erros.

---

# Licença

GNU General Public License v3.0 (GPL-3.0)
