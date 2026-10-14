# Lab 3 – VLANs e Roteamento (Router-on-a-Stick)

## Objetivo
Segmentar a rede em duas VLANs (Vendas e TI) e permitir a comunicação entre elas através de um roteador configurado com sub-interfaces.

## Topologia
- 2 switches Cisco 2960
- 4 PCs (2 em cada switch)
- 1 roteador Cisco 1941
- Switch0 conectado ao roteador (g0/1) e ao Switch1 (g0/2)
- Switch1 conectado ao Switch0 (g0/1)

## Configuração
| Dispositivo | IP | Gateway | VLAN |
|-------------|-----|---------|------|
| PC0 | 192.168.10.2 | 192.168.10.1 | 10 (Vendas) |
| PC1 | 192.168.20.2 | 192.168.20.1 | 20 (TI) |
| PC2 | 192.168.10.3 | 192.168.10.1 | 10 (Vendas) |
| PC3 | 192.168.20.3 | 192.168.20.1 | 20 (TI) |

---

## Comandos usados (Switch0)

```
enable
configure terminal

# Cria a VLAN 10 (Vendas)
vlan 10
name Vendas

# Cria a VLAN 20 (TI)
vlan 20
name TI
exit

# Configura a porta f0/1 para o PC0 (VLAN 10)
interface f0/1
switchport mode access              # Define a porta como acesso (para PC, não trunk)
switchport access vlan 10           # Coloca a porta dentro da VLAN 10
exit

# Configura a porta f0/2 para o PC1 (VLAN 20)
interface f0/2
switchport mode access
switchport access vlan 20
exit

# Configura a porta g0/1 como trunk (vai para o roteador)
interface g0/1
switchport mode trunk                # Modo trunk: carrega várias VLANs com etiqueta 802.1q
exit

# Configura a porta g0/2 como trunk (vai para o Switch1)
interface g0/2
switchport mode trunk
exit
```

---

## Comandos usados (Switch1)

```
enable
configure terminal

# Cria as mesmas VLANs do Switch0 (precisa existir nos dois)
vlan 10
name Vendas
vlan 20
name TI
exit

# Porta f0/1 para o PC2 (VLAN 10)
interface f0/1
switchport mode access
switchport access vlan 10
exit

# Porta f0/2 para o PC3 (VLAN 20)
interface f0/2
switchport mode access
switchport access vlan 20
exit

# Porta g0/1 como trunk (vai para o Switch0)
interface g0/1
switchport mode trunk
exit
```

---

## Comandos usados (Roteador)

```
enable
configure terminal

# Ativa a interface física (sem IP - ela é só o "canal")
interface g0/0
no shutdown
exit

# Sub-interface para a VLAN 10
interface g0/0.10
encapsulation dot1q 10               # Associa esta sub-interface à VLAN 10 (etiqueta 10)
ip address 192.168.10.1 255.255.255.0  # Gateway da VLAN 10
exit

# Sub-interface para a VLAN 20
interface g0/0.20
encapsulation dot1q 20               # Associa esta sub-interface à VLAN 20 (etiqueta 20)
ip address 192.168.20.1 255.255.255.0  # Gateway da VLAN 20
exit

# Desliga a g0/1 (não está sendo usada)
interface g0/1
shutdown
exit
```

---

## Testes
- PC0 → PC2 (mesma VLAN): 4 pacotes, 0% de perda.
- PC0 → PC3 (VLANs diferentes): primeiro pacote timeout (ARP), depois 3 respostas, 25% de perda.
- PC0 → gateway: 4 pacotes, 0% de perda.

## O que aprendi
- Criar VLANs e associar portas de acesso.
- Configurar trunk entre switches.
- Configurar router-on-a-stick com sub-interfaces.
- Entender por que o primeiro pacote entre redes diferentes pode dar timeout (ARP).
