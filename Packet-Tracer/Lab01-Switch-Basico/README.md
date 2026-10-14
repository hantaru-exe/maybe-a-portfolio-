# Lab 1 – Rede Básica com Switch

## Objetivo
Conectar dois computadores a um switch e testar a comunicação entre eles na mesma rede.

## Topologia
- 2 PCs (PC0 e PC1)
- 1 Switch Cisco 2960

## Configuração
| Dispositivo | IP | Máscara |
|-------------|-----|---------|
| PC0 | 192.168.1.1 | 255.255.255.0 |
| PC1 | 192.168.1.2 | 255.255.255.0 |

## Teste
Comando: `ping 192.168.1.2` (executado no PC0)
Resultado: 4 pacotes enviados, 4 recebidos, 0% de perda.

## O que aprendi
- Como configurar IP em um PC no Packet Tracer.
- Como funciona a comunicação dentro da mesma sub-rede.
- Como testar conectividade com o comando `ping`.
