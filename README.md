Configuração de Firewall com iptables

Este repositório contém scripts e configurações para gerenciar e configurar o iptables no Linux. O iptables é uma ferramenta poderosa usada para configurar o firewall e gerenciar o tráfego de rede no sistema.
	
O que é o iptables?

O iptables é uma ferramenta para configurar o firewall no Linux. Ele permite que você defina regras para permitir, negar ou modificar o tráfego de rede com base em diferentes parâmetros como IP, portas, protocolos e interfaces de rede.
Principais funcionalidades:

    Filtragem de pacotes (firewall).
    NAT (Network Address Translation): Modificação de endereços IP.
    Controle de tráfego por interface.

Como Usar

Aqui estão alguns comandos e configurações comuns para usar o iptables.
Limpar Regras

Se você quiser limpar todas as regras existentes do iptables, use o comando:

bash

sudo iptables -F

Isso remove todas as regras de todas as cadeias na tabela filter (a tabela padrão).
Configurar NAT

Para mascaramento de IP (usado para compartilhar a conexão de internet), adicione a seguinte regra:

bash

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

Este comando cria uma regra de NAT que mascarará os pacotes que saem pela interface eth0.
Configurar Redirecionamento de Portas

Se você quiser redirecionar o tráfego de uma porta para outro servidor, você pode usar o DNAT. Exemplo de redirecionamento de porta 80 para um servidor interno:

bash

sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.100:8080

Isso fará com que qualquer tráfego TCP na porta 80 seja redirecionado para o IP 192.168.1.100 na porta 8080.
Exemplo de Script

Aqui está um exemplo simples de script iptables para definir regras de firewall básicas:

bash

#!/bin/bash

# Limpar todas as regras existentes
iptables -F

# Definir políticas padrão
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

# Permitir tráfego de loopback
iptables -A INPUT -i lo -j ACCEPT

# Permitir tráfego de entrada na porta 22 (SSH)
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Permitir tráfego de entrada na porta 80 (HTTP)
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Permitir tráfego de entrada na porta 443 (HTTPS)
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Bloquear tráfego por padrão
iptables -A INPUT -j DROP

Este script limpa as regras atuais, define as políticas padrão de aceitação para todas as cadeias e permite tráfego nas portas 22 (SSH), 80 (HTTP), e 443 (HTTPS), enquanto bloqueia qualquer outro tráfego.
Comandos Comuns

Aqui estão alguns comandos comuns para manipular regras de iptables:

    Listar regras:

    bash

sudo iptables -L

Mostrar regras da tabela NAT:

bash

sudo iptables -t nat -L

Adicionar regra (exemplo para permitir SSH):

bash

sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

Remover regra (exemplo para remover regra de SSH):

bash

sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT

Salvar as regras atuais (dependendo da distribuição):

    Para Debian/Ubuntu:

    bash

sudo iptables-save > /etc/iptables/rules.v4

Para RHEL/CentOS:

bash

    sudo service iptables save

Restaurar as regras salvas:

    Para Debian/Ubuntu:

    bash

        sudo iptables-restore < /etc/iptables/rules.v4

Persistência das Regras

Para garantir que as regras de iptables persistam após o reboot, você pode usar os seguintes métodos:

    Debian/Ubuntu: Use o iptables-persistent:

        Instale o pacote:

        bash

        sudo apt install iptables-persistent

        As regras serão salvas automaticamente em /etc/iptables/rules.v4 e restauradas durante a inicialização.

    RHEL/CentOS: Use service iptables save para salvar as regras.

Problemas Comuns

    O firewall está bloqueando conexões: Se você bloquear acidentalmente o tráfego necessário (como SSH), use um live CD/USB ou outra interface de rede para corrigir as regras.

    As regras não persistem após a reinicialização: Certifique-se de que as regras estão sendo salvas corretamente e configuradas para carregar durante a inicialização do sistema.
