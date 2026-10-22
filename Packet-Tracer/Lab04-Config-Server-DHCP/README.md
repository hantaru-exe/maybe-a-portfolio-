# Lab 4 – Configuração de Servidor DHCP

## Objetivo
Configurar um roteador para atuar como servidor DHCP, entregando IPs, gateways e DNS automaticamente para os PCs da rede, eliminando a necessidade de configuração manual em cada máquina.

## Topologia
- 3 PCs (PC0, PC1, PC2)
- 1 Switch (2960)
- 1 Roteador (2911)
- O roteador está conectado na porta g0/1.

## Configuração
| Dispositivo | IP | Gateway | Método |
|-------------|-----|---------|--------|
| PC0, PC1, PC2 | 192.168.1.x (automático) | 192.168.1.1 | DHCP |
| Roteador g0/1 | 192.168.1.1 | - | Estático |

---

## 🛠️ Comandos usados e seus significados

### 1. Configuração da Interface (O Gateway)

`enable`
* Entra no modo de execução privilegiada (permite ver e salvar configurações).

`configure terminal`
* Entra no modo de configuração global (permite alterar o funcionamento do equipamento).

`interface g0/1`
* Entra na interface GigabitEthernet 0/1 (a porta onde o cabo do switch está conectado).

`ip address 192.168.1.1 255.255.255.0`
* Define o IP do roteador e a máscara de sub-rede. Esse IP será o Default Gateway de todos os PCs.

`no shutdown`
* Liga a interface. Por padrão, as portas do roteador vêm desligadas (administratively down).

`exit`
* Sai do modo de configuração da interface e volta para o modo global.

### 2. Configuração do Servidor DHCP (O "Pote" de IPs)

`ip dhcp pool rede1`
* Cria um pool de endereços DHCP e dá o nome de "rede1" a ele. O roteador agora atua como um servidor DHCP.

`network 192.168.1.0 255.255.255.0`
* Define qual é a rede e a máscara que serão entregues aos PCs (a faixa de IPs disponíveis).

`default-router 192.168.1.1`
* Diz aos PCs qual é o Gateway (porta de saída). Neste caso, é o IP da interface g0/1 do próprio roteador.

`dns-server 8.8.8.8`
* Diz aos PCs qual servidor DNS eles devem usar para traduzir nomes (como google.com) em IPs. O 8.8.8.8 é o DNS público do Google.

`exit`
* Sai do modo de configuração do pool DHCP.

### 3. Garantindo o Serviço e Salvando

`service dhcp`
* Garante que o serviço de DHCP está ativado no roteador (em alguns simuladores, ele pode vir desligado por padrão).

`write memory`
* Salva todas as configurações na memória NVRAM. Se o roteador for reiniciado, as configurações não serão perdidas.

---

## 🧪 Testes e Solução de Problemas (Troubleshooting)

- **Configuração dos PCs:** No menu "IP Configuration" do PC, a opção **DHCP** foi selecionada.
- **Teste de IP:** O comando `ipconfig` no Command Prompt do PC mostra que ele recebeu um IP na faixa `192.168.1.x` e o gateway `192.168.1.1`.
- **Teste de Roteamento:** O comando `ping 192.168.1.1` no PC funcionou.
- **Verificação no Roteador:** O comando `show ip dhcp binding` mostrou a tabela com os IPs alugados para cada PC.

### ⚠️ Problema encontrado e solução (O "Pulo do Gato")
**Problema:** Inicialmente, os PCs pegaram um IP `169.254.x.x` (APIPA - endereço que o PC cria sozinho quando não acha o servidor DHCP).
**Solução:** O Packet Tracer às vezes demora para simular o processo DORA (Discover, Offer, Request, Acknowledge). Para resolver, foi necessário forçar a atualização no PC usando `ipconfig /release` e `ipconfig /renew`, ou alternar o PC para IP fixo (Static) e voltar para DHCP, além de usar o botão "Fast Forward Time" (Avançar Tempo) no simulador.

## O que aprendi
- Como configurar um pool de endereços DHCP.
- A importância do processo DORA para a concessão de IPs.
- Como diagnosticar problemas de rede (se o PC está com APIPA, o DHCP não funcionou).
- Como forçar a atualização do IP no PC quando o simulador não atualiza automaticamente.
