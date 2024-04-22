---
Projeto: ansible-zabbix-agent
Descrição: Esse projeto tem o objetivo de instalar e configurar o pacote "zabbix-agent" em
           sistema baseado no Debian, ReHat e Suse.
Autor: Glauber GF (mcnd2)
Data: 2024-04-21
---

# Instalar e Configurar o "Zabbix Agent" com o Ansible baseado em Debian, RedHat e Suse.

![Image](https://github.com/glaubergf/ansible-zabbix-agent/blob/main/images/hosts_zabbix.png)

O objetivo desse projeto é executar com o Ansible a automatização para instalar e configurar o Zabbix Agent em máquinas Linux, baseado em distribuições Debian, RedHat e Suse.

O **[Zabbix Agent](https://www.zabbix.com/documentation/6.4/pt/manual/guides/monitor_linux?hl=Zabbix%2Cagent)** é o processo responsável pela coleta de dados. Ele é um componente essencial do Zabbix, que é uma plataforma de monitoramento de rede e sistemas. O Zabbix Agent é instalado nos dispositivos que você deseja monitorar e coleta dados específicos sobre esses dispositivos para enviar de volta ao servidor Zabbix.

Existem dois modos principais de operação para o Zabbix Agent:

* Modo Passivo:

  Neste modo, o Zabbix Agent espera passivamente por solicitações do servidor Zabbix. O servidor envia uma solicitação ao agente em intervalos regulares para obter dados de monitoramento, e o agente responde com as informações solicitadas. Este modo é mais comum em ambientes onde a comunicação de saída do dispositivo é limitada, como em firewalls ou em dispositivos de rede.

* Modo Ativo:

  Neste modo, o Zabbix Agent envia ativamente os dados de monitoramento para o servidor Zabbix em intervalos regulares. O agente inicia a comunicação com o servidor e envia os dados sem que o servidor precise solicitar. Esse modo é mais adequado para dispositivos com comunicação de saída permitida e oferece uma abordagem mais proativa para o monitoramento.

Em resumo, o Zabbix Agent é responsável por coletar dados de monitoramento nos dispositivos e enviá-los de volta ao servidor Zabbix, e pode operar tanto no modo passivo quanto no modo ativo, dependendo das necessidades e restrições do ambiente de rede.

O **[Ansible](https://docs.ansible.com/ansible/latest/getting_started/index.html)** fornece automação de código aberto que reduz a complexidade e funciona em qualquer lugar. Usar o Ansible permite automatizar praticamente qualquer tarefa. A organização e estruturação do projeto Ansible são fundamentais para garantir a eficiência e a manutenção do código.

Atualmente o Ansible pertence a **[Red Hat](https://www.redhat.com/pt-br/technologies/management/ansible)**.

Pressupondo que você já tenha o **Ansible** e as suas dependências instaladas, para executar o projeto, faça o clone do mesmo e em seguida certifique-se que esteja dentro do diretório raíz do projeto. Altere os dados relacionados a seu ambiente e de acordo com as suas necessidades.

Para instalar e configurar o zabbix agent, role "zabbix-agent-linux", execute o comando seguido com a opção "-t" ( --tags ), nome da "tag" que foi dado nas tarefas da role.

```
ansible-playbook -i host main.yml -t zbx-agt
```

Caso queira desinstalar, siga o mesmo acima, mas especificando a "tag" da role "rm-zabbix-agent-linux".

```
ansible-playbook -i host main.yml -t zbx-agt-rm
```

Para saber mais opções do Ansible, execute com a opção "-h" ( --help) para mostrar a ajuda para o uso de cada opção.

```
ansible --help
```

## Playbook

O **Playbook** define uma série de **roles** que serão aplicadas no alvo (hots). Cada role é associada a uma **tag** específica, permitindo que as **tarefas** sejam executadas de forma seletiva com base nessas tags.

Há duas **roles** nesse projeto, uma para instalar o zabbix agent (_zabbix-agent-linux_) e outra para desinstalar (_rm-zabbix-agent-linux_).

Segue as especificações das **tarefas** de cada role.

### zabbix-agent-linux

* _zabbix_agent_linux.yml_

Realiza uma série de tarefas relacionadas à instalação, configuração e gerenciamento do Zabbix Agent em diferentes sistemas operacionais (Debian, RedHat e Suse). Segue um resumo das tarefas realizadas:

    -> Instalação do Zabbix Agent:
    Instala o pacote do Zabbix Agent.
    Cria e configura o arquivo zabbix_agentd.conf.

    -> Configuração do Zabbix Agent:
    Atualiza as configurações do agente Zabbix, incluindo o servidor, servidor ativo e nome do host.

    -> Gerenciamento do Serviço:
    Verifica se o serviço do Zabbix Agent está ativo e o habilita para inicialização automática.
    Recarrega o serviço se houver alterações nas configurações.

    -> Gerenciamento do Firewall:
    Configura o iptables para permitir tráfego na porta TCP 10050 (para Debian).
    Configura o Firewalld para permitir tráfego na mesma porta (para RedHat e Suse).

Todas as tarefas são marcadas com a **tag** "zbx-agt", o que facilita a execução específica dessas tarefas em outras playbooks ou comandos Ansible usando tags.

### rm-zabbix-agent-linux

* _rm_zabbix_agent_linux.yml_

É destinada a remover o pacote e a configuração do Zabbix Agent de clientes em diferentes distribuições de sistemas operacionais. Segue um resumo das tarefas realizadas:

    -> Depuração da distribuição e os_family:
    Exibe a distribuição e a família do sistema operacional.
    Define a variável os_family com base na distribuição do Ansible.
    Usa o filtro vars para determinar a família do sistema operacional com base na distribuição.

    -> Remoção do pacote "Zabbix Agent":
    Remover o pacote do Zabbix Agent.
    Usa a variável zabbix_package_name para definir o nome do pacote com base na família do sistema operacional.

    -> Remoção dos arquivos de configuração do zabbix-agent:
    Remover os arquivos de configuração do Zabbix Agent.
    O caminho do arquivo é determinado com base na família do sistema operacional.

    -> Remoção da regra do iptables para tráfego na porta "TCP 10050":
    Remover a regra do iptables para a porta 10050.
    A ação é executada apenas em sistemas Debian.

    -> Remoção da regra do Firewalld para tráfego na porta "TCP 10050":
    Desativa a regra do Firewalld para a porta 10050.
    A ação é executada em sistemas RedHat e Suse.

As tarefas são marcadas com **tags** "zbx-agt-rm", facilitando a execução específica dessas tarefas em outras playbooks ou comandos Ansible usando tags.

# Licença

**GNU General Public License** (_Licença Pública Geral GNU_), **GNU GPL** ou simplesmente **GPL**.

[GPLv3](https://www.gnu.org/licenses/gpl-3.0.html)

------

Copyright (c) 2024 Glauber GF (mcnd2)

Este programa é um software livre: você pode redistribuí-lo e/ou modificar
sob os termos da GNU General Public License conforme publicada por
a Free Software Foundation, seja a versão 3 da Licença, ou
(à sua escolha) qualquer versão posterior.

Este programa é distribuído na esperança de ser útil,
mas SEM QUALQUER GARANTIA; sem mesmo a garantia implícita de
COMERCIALIZAÇÃO ou ADEQUAÇÃO A UM DETERMINADO FIM. Veja o
GNU General Public License para mais detalhes.

Você deve ter recebido uma cópia da Licença Pública Geral GNU
junto com este programa. Caso contrário, consulte <https://www.gnu.org/licenses/>.

*

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>