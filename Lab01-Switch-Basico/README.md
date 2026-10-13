# Lab 1 – Rede Básica com Switch

## Objetivo
Conectar dois computadores a um switch e testar a comunicação entre eles na mesma rede.

## Topologia
- 2 PCs
- 1 Switch Cisco 2960

## Configuração
- PC0: IP 192.168.1.1 / Máscara 255.255.255.0
- PC1: IP 192.168.1.2 / Máscara 255.255.255.0
- Cabos de rede diretos conectando os PCs ao switch.

## Testes
- Comando: `ping 192.168.1.2` a partir do PC0.
- Resultado: Sucesso (4 pacotes enviados, 4 recebidos).

## O que aprendi
Entendi como um switch opera na camada de enlace para conectar dispositivos na mesma rede, e como o endereçamento IP e a máscara de rede permitem a comunicação.
