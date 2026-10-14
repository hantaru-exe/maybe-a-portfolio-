# Lab 2 – Duas Redes com Roteador

## Objetivo
Conectar duas redes diferentes através de um roteador e permitir a comunicação entre elas.

## Topologia
- 2 PCs, 2 switches, 1 roteador (Cisco 1941)
- PC0 na rede 192.168.10.0/24
- PC1 na rede 192.168.20.0/24

## Configuração
| Dispositivo | IP | Gateway |
|-------------|-----|---------|
| PC0 | 192.168.10.2 | 192.168.10.1 |
| PC1 | 192.168.20.2 | 192.168.20.1 |
| Roteador g0/0 | 192.168.10.1 | – |
| Roteador g0/1 | 192.168.20.1 | – |

## Comandos usados (roteador)
