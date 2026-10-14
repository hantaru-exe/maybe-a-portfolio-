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
| Roteador g0/0 | 192.168.10.1 | - |
| Roteador g0/1 | 192.168.20.1 | - |

## Comandos usados (roteador)
```
enable
configure terminal
interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
interface g0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
write memory
```

## Teste
Ping do PC0 (192.168.10.2) para o PC1 (192.168.20.2): **4 pacotes enviados, 4 recebidos, 0% de perda.**

## O que aprendi
- Como configurar IP em interfaces de roteador.
- O que é gateway e por que ele é necessário para comunicação entre redes.
- Como o roteador encaminha pacotes entre redes diretamente conectadas.

## Problema encontrado e solução
O switch ainda estava com a VLAN 10 do Lab 1 configurada, isolando o PC0. 
Solução: resetar o switch com `erase startup-config` e `reload`.
