# Anotações de Redes

## O que é uma Rede?

### Tipos de Redes
- **LAN (Local Area Network)**: Rede local em um prédio ou escritório
- **WAN (Wide Area Network)**: Rede que se estende por grandes distâncias (ex: Internet)
- **MAN (Metropolitan Area Network)**: Rede em uma cidade

---

## Modelo OSI (7 Camadas)

1. **Física**: Cabos, sinais, hardware
2. **Enlace de Dados**: Frames, MAC addresses
3. **Rede**: Roteamento, IP
4. **Transporte**: TCP, UDP, portas
5. **Sessão**: Gerenciamento de conexões
6. **Apresentação**: Criptografia, compressão
7. **Aplicação**: HTTP, FTP, DNS, SSH

---

## Modelo TCP/IP

- **Camada de Aplicação**: HTTP, HTTPS, SSH, FTP, DNS
- **Camada de Transporte**: TCP (confiável), UDP (rápido)
- **Camada de Internet**: IP, ICMP, ARP
- **Camada de Link**: Ethernet, WiFi

---

## Portas e Protocolos Comuns

| Porta | Protocolo | Uso |
|-------|-----------|-----|
| 80 | HTTP | Web não segura |
| 443 | HTTPS | Web segura |
| 22 | SSH | Acesso remoto seguro |
| 21 | FTP | Transferência de arquivos |
| 53 | DNS | Resolução de nomes |
| 25 | SMTP | Envio de emails |
| 110 | POP3 | Recebimento de emails |
| 3306 | MySQL | Banco de dados |

---

## Endereçamento IP e Sub-redes

### IPv4
- Formato: `192.168.1.1`
- Classes: A, B, C, D, E

### Máscaras de Sub-rede
- `/24` = `255.255.255.0`
- `/16` = `255.255.0.0`
- `/8` = `255.0.0.0`

### Cálculo de Sub-redes
```
Endereço: 192.168.1.0/24
Máscara: 255.255.255.0
Host: 192.168.1.1 até 192.168.1.254
Broadcast: 192.168.1.255
```

---

## Roteamento e Switching

### Roteamento
- Move pacotes entre redes diferentes
- Usa tabelas de roteamento
- Protocolos: OSPF, BGP, RIP

### Switching
- Conecta dispositivos na mesma rede (LAN)
- Usa MAC addresses
- Mais rápido que roteamento

---

## NAT, DHCP e DNS

### NAT (Network Address Translation)
- Traduz IPs privados para públicos
- Usado em roteadores residenciais

### DHCP (Dynamic Host Configuration Protocol)
- Atribui IPs automaticamente
- Porta 67/68 (UDP)

### DNS (Domain Name System)
- Converte nomes em IPs
- Porta 53 (UDP/TCP)

---

## Firewalls e Segmentação

### Firewalls
- Filtra tráfego baseado em regras
- Tipos: Stateless, Stateful, Proxy
- Publica e privada

### Segmentação
- VLANs separam redes logicamente
- DMZ (Demilitarized Zone) isola servidores web

---

## Conceitos de Segurança em Redes

- **Sniffing**: Captura de tráfego não criptografado
- **MITM**: Interceptação de comunicação
- **DoS/DDoS**: Negação de serviço
- **Spoofing**: Falsificação de identidade
