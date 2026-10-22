# Lab 5 - Configuração de DNS Interno

## Objetivo
Configurar um servidor DNS local para traduzir nomes de domínio (ex: www.empresa.com) em endereços IP, permitindo que os computadores da rede acessem serviços internos sem precisar decorar os IPs.

## Topologia
- 1 PC
- 1 Switch (2960)
- 1 Servidor
- 1 Roteador (2911)
- O roteador está conectado na interface g0/0.

## Endereçamento
| Dispositivo | IP | Gateway | Método |
| :--- | :--- | :--- | :--- |
| PC | 192.168.1.x (automático) | 192.168.1.1 | DHCP |
| Servidor | 192.168.1.10 | 192.168.1.1 | Estático |
| Roteador (g0/0) | 192.168.1.1 | - | Estático |

## Comandos Utilizados

### 1. Configuração do Roteador (IP, DHCP e Apontamento do DNS)
```text
enable
configure terminal
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

ip dhcp pool REDE
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 192.168.1.10
exit

write memory
```

* **interface g0/0**: Seleciona a interface física onde o switch está conectado.
* **ip address**: Define o IP do gateway da rede.
* **ip dhcp pool REDE**: Cria o pool de distribuição de IPs.
* **dns-server 192.168.1.10**: Este é o comando crucial deste lab. Ele diz aos PCs (via DHCP) que o servidor DNS da rede é o IP do nosso Servidor.
* **write memory**: Salva as configurações.

### 2. Configuração do Servidor (Interface Gráfica)
1. Na aba **Desktop > IP Configuration**, a opção foi alterada para **Static**.
2. IP configurado: `192.168.1.10` / Máscara: `255.255.255.0` / Gateway: `192.168.1.1`.
3. Na aba **Services > DNS**:
   - O serviço foi ativado (botão **On**).
   - Foi adicionado um registro (A Record) com o Name `www.empresa.com` e o Address `192.168.1.10`.

## Testes Realizados
- **No PC:** Comando `ipconfig /all` para confirmar que o PC recebeu o IP via DHCP e que o campo "DNS Servers" aponta para `192.168.1.10`.
- **No PC:** Comando `ping www.empresa.com` para testar a resolução de nomes.

## Resolução de Problemas (Troubleshooting)
Durante a execução do lab, dois problemas foram encontrados e solucionados:
1. **Bug visual no Packet Tracer:** A tela de configuração do Servidor ficou com o fundo escuro e as letras invisíveis (fonte preta no fundo preto). Solução: Alterar o tema do Packet Tracer em Options > Preferences > Interface.
2. **Falha na resolução de nomes:** O ping para o nome `www.empresa.com` retornou "Ping request could not find host". O ping direto para o IP `192.168.1.10` funcionou. Solução: O serviço de DNS no servidor estava desligado (off). Ao ligar o serviço e adicionar o registro novamente, o ping no domínio funcionou.

## Aprendizados
- Como configurar um servidor DNS local e criar registros do tipo A.
- Como instruir os clientes a usar o servidor DNS correto através do pool DHCP no roteador.
- A importância de isolar problemas de rede: se o ping no IP funciona mas o ping no nome falha, o problema está no DNS.
