# Lab 4 - Configuração de Servidor DHCP

## Objetivo
Configurar um roteador para atuar como servidor DHCP, distribuindo IPs, gateways e DNS automaticamente para os computadores da rede.

## Topologia
- 3 PCs (PC0, PC1, PC2)
- 1 Switch (2960)
- 1 Roteador (2911)
- O roteador está conectado na interface g0/1.

## Endereçamento
| Dispositivo | IP | Gateway | Método |
| :--- | :--- | :--- | :--- |
| PC0, PC1, PC2 | 192.168.1.x (automático) | 192.168.1.1 | DHCP |
| Roteador (g0/1) | 192.168.1.1 | - | Estático |

## Comandos Utilizados

### 1. Configuração da Interface
```text
enable
configure terminal
interface g0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

* **enable**: Entra no modo de execução privilegiada.
* **configure terminal**: Entra no modo de configuração global.
* **interface g0/1**: Seleciona a interface GigabitEthernet 0/1.
* **ip address**: Define o IP e a máscara da interface. Este IP será o gateway da rede.
* **no shutdown**: Ativa a interface (que vem desligada por padrão).
* **exit**: Sai do modo de configuração da interface.

### 2. Configuração do Pool DHCP
```text
ip dhcp pool rede1
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
exit
```

* **ip dhcp pool rede1**: Cria um pool de endereços DHCP e o nomeia como "rede1".
* **network**: Define a faixa de rede que será distribuída.
* **default-router**: Define o gateway que os PCs devem usar (o IP do próprio roteador).
* **dns-server**: Define o servidor DNS (usei o 8.8.8.8 do Google).
* **exit**: Sai do modo de configuração do pool.

### 3. Ativação do Serviço e Salvamento
```text
service dhcp
write memory
```

* **service dhcp**: Garante que o serviço de DHCP esteja ativo no roteador.
* **write memory**: Salva as configurações na memória NVRAM.

## Testes Realizados
- Configuração dos PCs para obter IP via DHCP.
- Comando `ipconfig` nos PCs para confirmar o recebimento do IP e gateway.
- Comando `ping 192.168.1.1` a partir dos PCs para testar a conectividade com o gateway.
- Comando `show ip dhcp binding` no roteador para verificar os IPs alugados.

## Resolução de Problemas (Troubleshooting)
**Problema:** Inicialmente, os PCs receberam um IP no formato 169.254.x.x (APIPA). Isso ocorre quando o PC não encontra o servidor DHCP e acaba gerando um IP aleatório para si mesmo.

**Solução:** O Packet Tracer pode demorar para processar a troca de mensagens do DHCP (processo DORA). Para forçar a atualização, foi necessário rodar os comandos `ipconfig /release` e `ipconfig /renew` no PC, ou alternar a configuração do PC de DHCP para Static e depois voltar para DHCP. Também foi utilizado o botão "Fast Forward Time" para acelerar a simulação.

## Aprendizados
- Configuração e funcionamento de um pool DHCP no roteador.
- Entendimento do processo DORA (Discover, Offer, Request, Acknowledge).
- Identificação de falhas de DHCP através do endereço APIPA (169.254.x.x).
