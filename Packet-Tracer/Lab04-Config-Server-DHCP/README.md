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
