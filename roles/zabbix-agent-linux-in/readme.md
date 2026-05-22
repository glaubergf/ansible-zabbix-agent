# Zabbix Agent Linux Install Role

Role Ansible responsável pela instalação, configuração e integração do Zabbix Agent em distribuições Linux baseadas em Debian, RedHat e SUSE.

A role possui arquitetura modular, suporte multi-distribuição, múltiplos backends de firewall e integração opcional com Docker.

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
└── zabbix-agent-linux-in
    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── tasks
    │   ├── config.yml
    │   ├── docker.yml
    │   ├── firewall.yml
    │   ├── install.yml
    │   ├── main.yml
    │   ├── prereq.yml
    │   ├── repo.yml
    │   └── service.yml
    ├── templates
    │   └── zabbix_agent.conf.j2
    └── vars
        ├── debian.yml
        ├── redhat.yml
        └── suse.yml
```

---

# Fluxo da Role

A role executa automaticamente:

1. Coleta de pré-requisitos
2. Detecção da distribuição Linux
3. Configuração de repositórios oficiais do Zabbix
4. Instalação do agente Zabbix
5. Configuração do arquivo do agente
6. Configuração do firewall
7. Configuração opcional para Docker
8. Habilitação e inicialização do serviço

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

* Normalização da família Linux
* Coleta de service facts
* Detecção do backend de firewall
* Debug operacional

---

## repo.yml

Responsável por:

* Adicionar repositórios oficiais Zabbix
* Adicionar repositório oficial do Docker
* Preparar gerenciador de pacotes

Suporta:

* APT
* DNF/YUM
* Zypper

---

## install.yml

Responsável por:

* Instalação dos pacotes:
  * zabbix-agent
  * zabbix-agent2

---

## config.yml

Responsável por:

* Deploy do template:
  * `zabbix_agent.conf.j2`

* Configuração do:
  * Server
  * ServerActive
  * Hostname
  * ListenPort
  * Parâmetros adicionais

---

## firewall.yml

Responsável por:

* Detectar backend automaticamente:
  * UFW
  * firewalld
  * iptables

* Liberar porta do agente:
  * TCP/10050

---

## docker.yml

Responsável por:

* Adicionar usuário `zabbix` ao grupo `docker`
* Habilitar monitoramento Docker via agent

---

## service.yml

Responsável por:

* Habilitar serviço
* Iniciar serviço
* Validar status operacional

---

# Variáveis

## defaults/main.yml

Variáveis customizáveis pelo usuário.

Exemplo:

```yaml
zabbix_server: "192.168.0.10"
zabbix_server_active: "192.168.0.10"
zabbix_agent_hostname: "{{ inventory_hostname }}"
zabbix_agent_port: "10050"
```

---

## vars/

Variáveis específicas por distribuição:

### debian.yml

Variáveis específicas para sistemas Debian/APT.

### redhat.yml

Variáveis específicas para sistemas RedHat/DNF/YUM.

### suse.yml

Variáveis específicas para sistemas SUSE/Zypper.

---

# Handlers

## handlers/main.yml

Handlers utilizados para:

* Restart do serviço Zabbix Agent
* Reload quando configuração é alterada

---

# Firewall Support

## UFW

Distribuições Debian modernas.

## firewalld

Distribuições RedHat/SUSE modernas.

## iptables

Fallback legado para sistemas antigos.

---

# Template

## templates/zabbix_agent.conf.j2

Template Jinja2 responsável pela configuração dinâmica do agente.

Permite:

* Parametrização automática
* Reutilização multiambiente
* Padronização enterprise

---

# Tags

## Execução completa

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in
```

---

## Simular execução (check mode)

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in -C
```

---

# Tags disponíveis

| Tag                   | Descrição                          
| --------------------- | ----------------------------------
| zbx-agt-in-prereq     | Executa coleta de pré-requisitos
| zbx-agt-in-repo       | Executa configuração de repositórios
| zbx-agt-in-install    | Executa instalação de pacotes
| zbx-agt-in-config     | Executa configuração do agente
| zbx-agt-in-firewall   | Executa configuração de firewall
| zbx-agt-in-ufw        | Executa configuração de UFW
| zbx-agt-in-firewalld  | Executa configuração de firewalld
| zbx-agt-in-iptables   | Executa configuração de iptables
| zbx-agt-in-docker     | Executa configuração de monitoramento Docker
| zbx-agt-in-service    | Executa configuração do serviço

---

## Apenas repo

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-repo
```

---

## Apenas install

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-install
```

---

## Apenas configuração

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-config
```

---

## Apenas firewall

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-firewall
```

---

## Apenas Docker

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-docker
```

## Apenas serviço

```bash
ansible-playbook -i hosts main.yml -t zbx-agt-in-service
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
