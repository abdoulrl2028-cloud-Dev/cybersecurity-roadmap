# Comandos Básicos de Redes

## Diagnóstico de Conectividade

### ping
Testa conectividade entre hosts.
```bash
ping google.com
ping -c 4 192.168.1.1  # Linux/Mac: 4 pacotes
ping -n 4 192.168.1.1  # Windows: 4 pacotes
```

### traceroute / tracert
Mostra o caminho que os pacotes percorrem até o destino.
```bash
traceroute google.com        # Linux/Mac
tracert google.com           # Windows
mtr google.com              # Alternativa interativa
```

---

## Configuração de Rede

### ipconfig / ifconfig
Exibe a configuração de rede do computador.
```bash
ipconfig                    # Windows
ifconfig                    # Linux/Mac (antigo)
ip addr show               # Linux (moderno)
ip route show              # Mostrar rotas
```

### Configurar IP manualmente (Linux)
```bash
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip route add default via 192.168.1.1
sudo systemctl restart networking
```

---

## Análise de Conexões

### netstat
Mostra conexões ativas e estatísticas de rede.
```bash
netstat -an                 # Todas as conexões
netstat -an | grep LISTEN  # Portas escutando
netstat -tulpn              # TCP/UDP com PIDs
ss -tulpn                   # Alternativa moderna
```

### lsof (List Open Files)
Mostra arquivos abertos e conexões de rede.
```bash
lsof -i                    # Todas as conexões
lsof -i :8080              # Conexões na porta 8080
```

---

## Varredura de Portas

### nmap
Ferramenta poderosa para descoberta de portas e hosts.
```bash
nmap 192.168.1.0/24         # Scan básico de rede
nmap -sV alvo.com           # Detectar versões
nmap -A alvo.com            # Scan agressivo
nmap -p- alvo.com           # Todas as 65535 portas
nmap -p 80,443,22 alvo.com  # Portas específicas
nmap -Pn alvo.com           # Ignora ping
```

---

## Consultas DNS

### nslookup
Consulta registros DNS.
```bash
nslookup google.com
nslookup -type=MX gmail.com
nslookup 8.8.8.8
```

### dig
Ferramenta DNS mais avançada.
```bash
dig google.com
dig google.com MX              # Registros de email
dig google.com +short          # Resposta curta
dig @8.8.8.8 google.com       # Usar DNS específico
dig google.com any             # Todos os registros
```

### host
Simples resolução de nomes.
```bash
host google.com
host 8.8.8.8
```

---

## Transferência de Dados

### curl
Faz requisições HTTP/HTTPS.
```bash
curl https://example.com
curl -H "User-Agent: Mozilla" https://example.com
curl -d "param=value" https://example.com
curl -i https://example.com       # Mostrar headers
```

### wget
Download de arquivos.
```bash
wget https://example.com/arquivo.zip
wget -O newname.zip https://example.com/arquivo.zip
```

---

## Análise de Tráfego

### tcpdump
Captura tráfego de rede.
```bash
sudo tcpdump -i eth0           # Capturar na eth0
sudo tcpdump -i eth0 -w dump.pcap  # Salvar em arquivo
sudo tcpdump -i eth0 port 80   # Apenas porta 80
```

### wireshark
Analisador gráfico de pacotes (GUI).
```bash
sudo wireshark
```

---

## Teste de Porta Aberta

### nc (netcat)
Swiss army knife da rede.
```bash
nc -zv target.com 80           # Verificar se porta está aberta
nc -l -p 9000                  # Abrir listener na porta 9000
```

---

## Informações WHOIS

### whois
Informações sobre domínios e IPs.
```bash
whois google.com
whois 8.8.8.8
```

---

## Teste de Velocidade

### iperf
Mede largura de banda entre dois hosts.
```bash
# Servidor
iperf -s

# Cliente
iperf -c server_ip
```

---

## Dicas Úteis

1. **Pipes e filtros**: `netstat -an | grep LISTEN`
2. **Grep para filtrar**: `ifconfig | grep -A1 inet`
3. **Combinações**: `nmap -sV target | grep open`
